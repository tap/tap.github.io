---
layout: post
title: "Writing DSP Objects"
date: 2009-12-03
categories: 74objects
source: "Originally published on 74objects.com"
---

In my last article I talked about [the structure of the Jamoma Platform](/74objects/2009/12/01/refactoring-jamoma.html).  That’s a bit abstract to be of much direct use.  A primer on how to write a DSP object seems to be in order.

So… let’s imagine we want to write a simple unit generator for audio processing.  One of the simplest filters we can write is a one-pole lowpass filter.  In pseudo code, it might look like this:

```
static float previous_output = 0.0;
static float feedback_coefficient = 0.5; // default is half way between 0 Hz and Nyquist

float processOneSample(float input)
{
    float output = (previous_output*feedback_coefficient) + ((1.0-feedback_coefficient)*input);
    previous_output = output;
    return output;
}
```

Simple, right?  Like most simple questions, the answer is only simple until you start asking more questions…  Let’s brainstorm a few practical questions about this simple filter:

- How do we set the coefficient?
- How do we efficiently process in blocks of samples instead of one sample at a time?
- how do we handle multiple channels?
- what if the number of channels changes on the fly?
- is the audio processed in a different thread than the object is created and deleted on?  how do we handle thread safety?
- do we want to have a “bypass” built-in so we can audition the effect?
- How do we wrap this for a host environment like Max/MSP?
- How do we wrap this as an AudioUnit plug-in?
- What if we want to swap this unit generator out for another in real-time, without having to recompile any code?
- How do we handle denormals and other similar gremlins that can cause performance headaches in realtime DSP code?

One more question: how do you get all of this without it sucking the life and love out of making cool DSP code? Funny you should ask, because that’s the very reason for the Jamoma DSP framework. Let’s look at an how we would write this object using Jamoma DSP.

## Example Class: TTLowpassOnepole

First, the header file: TTLowpassOnepole.h:

```
#include "TTDSP.h"

class TTLowpassOnePole : TTAudioObject {
	TTCLASS_SETUP(TTLowpassOnePole)
	TTFloat64		mFrequency;	///< filter cutoff frequency
	TTFloat64		mCoefficient;	///< filter coefficients
	TTSampleVector		mFeedback;	///< previous output sample for each channel

	TTErr updateMaxNumChannels(const TTValue& oldMaxNumChannels);
	TTErr updateSr();
	TTErr clear();
	TTErr setfrequency(const TTValue& value);
	inline TTErr calculateValue(const TTFloat64& x, TTFloat64& y, TTPtrSizedInt channel);
	TTErr processAudio(TTAudioSignalArrayPtr inputs, TTAudioSignalArrayPtr outputs);
};
```

The TTDSP.h header includes everything needed to create a subclass of TTAudioObject. We will see some of the magical joy of TTAudioObject shortly. In the class definition there is a macro called TTCLASS_SETUP. This creates prototypes for the constructor, destructor, and glue code for class registration, etc.

This class implementation thus follows as:

```
#include "TTLowpassOnePole.h"

#define thisTTClass		TTLowpassOnePole
#define thisTTClassName		"lowpass.1"
#define thisTTClassTags		"audio, processor, filter, lowpass"

TT_AUDIO_CONSTRUCTOR
{
	addAttributeWithSetter(Frequency,	kTypeFloat64);
	addAttributeProperty(Frequency, range, TTValue(2.0, sr*0.475));
	addAttributeProperty(Frequency, rangeChecking, TT("clip"));

	addMessageWithArgument(updateMaxNumChannels);
	addMessage(updateSr);
	addMessage(clear);

	// Set Defaults...
	setAttributeValue(TT("maxNumChannels"), arguments); // This attribute is inherited
	setAttributeValue(TT("frequency"), 1000.0);
	setProcessMethod(processAudio);
}

TTLowpassOnePole::~TTLowpassOnePole()
{
	; // Nothing special to do for this class
}

TTErr TTLowpassOnePole::updateMaxNumChannels(const TTValue& oldMaxNumChannels)
{
	mFeedback.resize(maxNumChannels);
	clear();
	return kTTErrNone;
}

TTErr TTLowpassOnePole::updateSr()
{
	TTValue	v(mFrequency);
	return setFrequency(v);
}

TTErr TTLowpassOnePole::clear()
{
	mFeedback.assign(maxNumChannels, 0.0);
	return kTTErrNone;
}

TTErr TTLowpassOnePole::setFrequency(const TTValue& newValue)
{
	TTFloat64	radians;

	mFrequency = newValue;
	radians = hertzToRadians(mFrequency);
	mCoefficient = TTClip(radians / kTTPi, 0.0, 1.0);
	return kTTErrNone;
}

inline TTErr TTLowpassOnePole::calculateValue(const TTFloat64& x, TTFloat64& y, TTPtrSizedInt channel)
{
	y = mFeedback[channel] = TTAntiDenormal((x * mCoefficient) + (mFeedback[channel] * (1.0 - mCoefficient)));
	return kTTErrNone;
}

TTErr TTLowpassOnePole::processAudio(TTAudioSignalArrayPtr inputs, TTAudioSignalArrayPtr outputs)
{
	TT_WRAP_CALCULATE_METHOD(calculateValue);
}
```

## Breaking it Down

To understand what’s happening here, let’s start at the bottom and work our way back up toward the top.

### processAudio

This method accepts an input and an output.  The input and output arguments are arrays of multichannel audio signals.  That is to say the each of the input and output can contain zero or more multichannel signals, and each of those signals may have zero or more channels.  The audio signal has a vector size which indicates how many samples are contained for each channel that is present.

In most cases an object is only functioning on one multichannel input signal and one multichannel output signal.  Also, in most cases, the number of input channels and output channels are the same (e.g. 2 inputs and 2 outputs).  Furthermore, it is quite common that each channel is processed in parallel, and can be considered independent of the other channels.

Given this set of somewhat common set of assumptions, we can avoid the work of handling all of this audio processing machinery and just call the TT_WRAP_CALCULATE_METHOD macro.  Calling that macro will invoke the named calculation method to be used for processing one sample on one channel of one signal.  The calculate method is inlined, so we do not give up the performance benefits of processing by vector.

### calculateValue

As we had previously alluded, this method calculates one output value for one input value.  You can think of this method in the mathematical terms

> y = f(x)

This method may be called directly or, as just discussed, called to crunch numbers for the vector-based audio processing method.

### setFrequency

As we will see shortly, attributes can be set using a default setter method that works most of the time.  In this case we need to do some customized work when the “Frequency” attribute is set.  Namely, we need to calculate the feedback coefficient.  We want to do that here so that the coefficient isn’t calculated every time our audio processing method is called.

This is the first time we’ve seen the TTValue data type, but we’ll be seeing a lot more of it.  This is the standard way of passing values.  TTValue can contain zero or more of any common data type (ints, floats, pointers) or special types defined in the Jamoma Foundation (symbols, objects, strings, etc.).

### clear

This method is quite simple: it resets all of the feedback samples for each audio channel to zero.  It can be invoked by a user if the filter ‘blows-up’.

### updateSr

This method is slightly special.  Just as we have a “Frequency” attribute, we have an “sr” attribute, which is the sample-rate of the object.  The trick is that we inherit the “sr” attribute from TTAudioObject.

Some objects may ignore the sample rate, or will function properly when the sample rate changes by virtue of the face that the member variable changed values.  In our case we need to take further action because our coefficient needs to be re-calculated.  The “updateSr” method is a notification that we will receive from our base class when the base class’ “sr” attribute is modified.

### updateMaxNumChannels

Just like the updateSr() method, this method is a notification sent to us by our base class.  In this case, the notification is sent when the base class has a change in its “maxNumChannels” attribute.

The “maxNumChannels” attribute is an indicator of the maximum number of channels the object should be prepared to process in the audio processing method.  As such, we use this notification to take care of memory allocation for anything in our instance that is related to the number of channels we process.

### The Destructor

As the comment says, we don’t have anything special to take care of in this case.  We still define the destructor so that we can be explicit about what is happening regarding object life-cycle.

### The Constructor

Obviously, to experienced C++ programmers anyway, the constructor is what gets called when a new instance of our class is created.  But what we do in this constructor is what makes everything else we’ve been through actually work.

First, we use a macro to define the function prototype.  We do this because it is the same every single time, and this ensures that we don’t screw up the initialization (or lack of initialization) of members or super-classes.

Next, we define attributes.  In our case we have only one attribute, and that attribute has a custom setter method (the setFrequency() method).  It is represented by the mFrequency member variable.  Attributes can be given properties.  In this case we limit the range of the values for our attribute to things that will actually work.

In addition to attributes, which have a state and are represented by data members, we have messages.  These are stateless methods that will be invoked any time our object receives the said message.  Messages might have no arguments, as in the case of the “sr” and “clear” messages.  If they do have arguments the arguments will passed as a TTValue reference, as in the case of the “updateMaxNumChannels” method.

Finally we set defaults.  This means default attribute values, but it also means the initial audio processing and value calculation methods.  These methods may be changed on the fly during operation, though in our case we only have one of each.

## Gift Wrap

To summarize, we now have an object with the following features from our original list:

- We set the coefficient using an attribute for cutoff frequency, which is automatically updated when the sample rate changes.
- We efficiently process in blocks of samples (instead of one sample at a time) using the processAudio method.
- processAudio also handles N channels of input and output transparently.
- It is no problem if the number of channels changes on the fly, this is all handled properly.
- The audio may be processed in a different thread than the one on which the object is created and deleted.  Thread safety for this circumstance is handled by the environment.
- We did not discuss it, but we do have a “bypass” attribute that we inherited, among others, so we got this functionality for free.
- We can swap any object inheriting from TTAudioObject for another in real-time.  The attributes and messages are called by dynamically bound symbols, so there are no linking problems or related concerns.
- We did not discuss it but the calculateValue method handles denormals using a library function.

So now we just need to use the object.  TTAudioObject classes have been used directly and in combinations with each other to create Max/MSP objects, Pd objects, VST plug-ins, AudioUnit plug-ins, etc.  Some examples of these can be found in the [Jamoma DSP Github repository](http://github.com/tap/JamomaDSP).  Others include the [Tap.Tools, sold by Electrotap](http://shop.electrotap.com/products/Taptools).

### The Magic Wand

One of the benefits of our dynamically-bound, message-passing TTAudioObjects is that we can use introspection on objects to find out about them at runtime.  That means we can load an object by name, ask what attributes it has and what types they are, and then create a new interface or adapter to the object.  One manifestation of this a class wrapper for Cycling ’74′s Max environment.

Given our TTAudioObject that implements a onepole lowpass filter, all that is required to make a full-blown Max/MSP object complete with Max attributes is this:

```
#include "TTClassWrapperMax.h"

int TTCLASSWRAPPERMAX_EXPORT main(void)
{
	TTDSPInit();
	return wrapTTClassAsMaxClass(TT("lowpass.1"), "jcom.onepole~", NULL);
}
```

The first symbol we pass is the symbol name of the TTAudioObject.  The second argument is the name of the Max class we generate.  It really is this easy.

At the time of this writing, no one that I’m aware of has written a similar class wrapper for PureData, SuperCollider, AudioUnits, etc.  but there is no reason that this kind of wrapper couldn’t work for any of those target environments.

It’s fun stuff!  As the Jamoma Foundation and DSP projects have evolved over the last six years the code for classes has become increasingly flexible and also increasingly clear.  It’s possible to really focus on the task in the code without having to worry about all of the glue and filler typically involved in writing audio code with C and C++ APIs.
