---
layout: post
title: "Custom Data-Types in Max Part 4: Passing Object Pointers"
date: 2010-02-06
categories: 74objects
source: "Originally published on 74objects.com"
---

How do you pass data between objects in Max?  If the data is a simple number or a symbol then the answer is easy.  What happens when you are trying to pass around audio vectors, dictionaries, images, or some other kind of object?  The implementation of Jamoma Multicore for Max deals with these issues head-on as it provides an illustration of how this problem can be tackled.
This is the fourth article in a series about working with custom data types in Max.  In the first two articles we laid the groundwork for the various methods by discussing how we wrap the data that we want to pass.  The third article demonstrated the use of Max’s symbol binding as means to pass custom data between objects.  This article will show an example of passing pointers directly between objects without using the symbol table.  In this series:

- [Introduction](/74objects/2009/03/25/custom-data-types-1.html)
- [Creating “nobox” classes](/74objects/2009/03/31/custom-data-types-2.html)
- [Binding to symbols](/74objects/2009/04/07/custom-data-types-3.html) (e.g. table, buffer~, coll, etc.)
- [Passing objects directly](http://74objects.com/2010/02/06/custom-data-types-4) (e.g. Jamoma Audio Graph)
- Hash-based reference system (similar to Jitter)

## A Peer Object System

Jamoma Audio Graph for Max is implemented as what might be called a Peer Object System.  What is meant is that for every object that a user creates and manipulates in a Max patcher, there is a matching object that exists in a parallel system. As detailed in [Designing an Audio Graph](/74objects/2010/02/03/designing-multicore.html), a Jamoma Audio Graph object has inlets and outlets and maintains connections to other objects to create a graph for processing audio through the objects.  The implementation of Jamoma Audio Graph for Max then has the task of creating and destroying these objects, sending them messages, and making the connections between them.  Once the objects are connected Jamoma Audio Graph will take care of itself.   The end result is that no audio processing actually happens in the Max objects for Jamoma Audio Graph — instead the Max objects are a thin façade that helps to set up the relationships between the objects as they exist in something akin to a parallel universe.

## A Patcher

[![A Jamoma Multicore patcher in Max](https://web.archive.org/web/2013if_//assets/images/74objects/2010-02-screen-shot-2010-02-05-at-10-46-22-am.png)](/assets/images/74objects/2010-02-screen-shot-2010-02-05-at-10-46-22-am.png) For context, let’s take a look at a Max patcher using Jamoma Audio Graph. In this patcher we have 4 Jamoma Audio Graph objects, identified by the ≈ symbol at the tail of the object name.  Each of these Max objects have a peer Audio Graph object internal to themselves.  Each Audio Graph object then contains a Jamoma DSP object that performs the actual signal processing. For example, the **jcom.overdrive≈** object contains a pointer to a  Jamoma Audio Graph object that contains an instance of the Jamoma DSP overdrive class.  The attributes of the overdrive class, such as *bypass*, *mute*, and *drive* are then exposed as Max attributes so that they can be set in the patcher. Remember that each connection may carry N channels of audio.  The **jcom.oscil≈** is, in this case, producing a stereo signal which is then propagated through the processing graph down to the **jcom.dac≈ object.**

## Configuring the Graph

The exciting work doesn’t begin until the start message is sent to the **jcom.dac≈** object.  As with all Jamoma Audio Graph externals, jcom.dac≈ Max external has a peer object.  In this case the peer object that it wraps is the **multicore.output** object.  This is the same **multicore.output** object that is shown in the Ruby examples in the [Designing an Audio Graph](/74objects/2010/02/03/designing-multicore.html) article. When the *start* message is sent, the **jcom.dac≈** object performs the following sequence:

- Send a *multicore.reset* message to all objects in the patcher.  This message sends a *reset* message to the peer objects underneath, which tells them to forget all of their previous connections.
- Send a *multicore.setup* message to all objects in the patcher.  This message tells the objects to try and connect to any object below it in the patcher.
- Tell the audio driver to start running.  When it is running it will periodically request blocks of samples from us, which in turn means that we will ask the other objects in the graph to process.

The processing happens completely within the Jamoma Multicore objects, thus not involving the Max objects at all.  It is the set up of the network of objects in the graph (steps 1 and 2) that involve our passing of custom data types in Max.

## Diving into the code

For a full source listing of the j**com.dac≈** object, you can find the code in [Jamoma Audio Graph source code repository](http://github.com/tap/JamomaAudioGraph/blob/master/implementations/MaxMSP/dac/jcom.dac.cpp).  We’ll abstract the important parts from that code below.  Let’s start with the method that is executed when the start message is sent:

```
TTErr DacStart(DacPtr self)
{
	MaxErr			err;
	ObjectPtr		patcher = NULL;
	long			vectorSize;
	long			result = 0;
	TTAudioGraphInitData	initData;

	self->multicoreObject->mUnitGenerator->getAttributeValue(TT("vectorSize"), vectorSize);

 	err = object_obex_lookup(self, gensym("#P"), &patcher);
 	object_method(patcher, gensym("iterate"), (method)DacIterateResetCallback, self, PI_DEEP, &result);
 	object_method(patcher, gensym("iterate"), (method)DacIterateSetupCallback, self, PI_DEEP, &result);

 	initData.vectorSize = vectorSize;
 	self->multicoreObject->init(initData);
 	return self->multicoreObject->mUnitGenerator->sendMessage(TT("start"));
}
```

As previously discussed, the last we thing we do is send a *start* message to our peer object, the **multicore.output**, so that the audio driver will start pulling audio vectors from us. Prior to that we iterate the Max patcher recursively (so the messages go to subpatchers too) to send the *multicore.setup* and *multicore.reset* messages.   To do this, we send the *iterate* message to the patcher and pass it a pointer to a method we define.  Those two methods are defined as follows.

```
void DacIterateResetCallback(DacPtr self, ObjectPtr obj)
{
	TTUInt32	vectorSize;
	method		multicoreResetMethod = zgetfn(obj, gensym("multicore.reset"));

	if (multicoreResetMethod) {
		self->multicoreObject->mUnitGenerator->getAttributeValue(TT("vectorSize"), vectorSize);
		multicoreResetMethod(obj, vectorSize);
	}
}

void DacIterateSetupCallback(DacPtr self, ObjectPtr obj)
{
	method multicoreSetupMethod = zgetfn(obj, gensym("multicore.setup"));

	if (multicoreSetupMethod)
		multicoreSetupMethod(obj);
}
```

These functions are called on every object in the patcher.  If we start with the last function, we can see that we first call zgetfn() on the object, *obj*, which is passed to us.  If that object possesses a *multicore.setup *method then we will receive a pointer to that method.  Otherwise we receive NULL.  If that method exists then we call it. The *multicore.reset *method works the same way.  The only difference is that the method takes an additional argument — the vector size at which the **jcom.dac≈** is processing.

### The Other End

At the other end of this calling sequence are the remaining objects in the patcher.  The [full ](http://github.com/tap/JamomaAudioGraph/blob/07306e2dff9b86b82839f005c17696ebbc08d4ea/implementations/MaxMSP/oscil/jcom.oscil.cpp)**[jcom.oscil≈](http://github.com/tap/JamomaAudioGraph/blob/07306e2dff9b86b82839f005c17696ebbc08d4ea/implementations/MaxMSP/oscil/jcom.oscil.cpp)**[ source code](http://github.com/tap/JamomaAudioGraph/blob/07306e2dff9b86b82839f005c17696ebbc08d4ea/implementations/MaxMSP/oscil/jcom.oscil.cpp) will show how this Max object is implemented.  In brief, we have two message bindings in the main function:

```
class_addmethod(c, (method)OscilReset, "multicore.reset",	A_CANT, 0);
	class_addmethod(c, (method)OscilSetup, "multicore.setup",	A_CANT,	0);
```

These two methods respond to the those called by the **jcom.dac≈** object.  They both have an A_CANT argument signature, which is how you define messages in Max that use function prototypes different than the standard  method prototypes.  These messages can’t be called directly by the user, and they are not listed in the object assistance, but we can send them from other parts of Max such as our **jcom.dac≈** object.  The *reset* message (for forgetting about all previous connections) is simply passed on to the oscillator’s Multicore peer object:

```
TTErr OscilReset(OscilPtr self)
{
	return self->multicoreObject->reset();
}
```

The *setup* method, as we discussed, tells our object that we need to try and make a connection to any object below us in the patcher. To do this we wrap our peer Multicore object’s pointer up into a Max atom.  That, together with the outlet number (zero), are passed as arguments to the *multicore.connect* message which is sent out our outlet.

```
TTErr OscilSetup(OscilPtr self)
{
	Atom a[2];

	atom_setobj(a+0, ObjectPtr(self->multicoreObject));
	atom_setlong(a+1, 0);
	outlet_anything(self->multicoreOutlet, gensym("multicore.connect"), 2, a);
	return kTTErrNone;
}
```

### One More Time…

That took care of the **jcom.oscil≈** object.  Once it sends the *multicore.connect* message out its outlet, its work is done.  But what happens with that message when it is received?

In our example it is going to a **jcom.overdrive≈** object.  The source code for **jcom.overdrive≈** isn’t going to be very helpful though.  It uses a magic class-wrapper that wraps any Jamoma DSP object as a Multicore object using 1 line of code.  That’s really convenient for coding, but not for seeing how all of the parts communicate.  So for our discussion, we will look at the **[jcom.dcblocker≈](http://github.com/tap/JamomaAudioGraph/blob/master/implementations/MaxMSP/dcblocker/jcom.dcblocker.cpp)**[ source code](http://github.com/tap/JamomaAudioGraph/blob/master/implementations/MaxMSP/dcblocker/jcom.dcblocker.cpp) instead — beginning with the main() function.

```
class_addmethod(c, (method)DCBlockerReset,	"multicore.reset",	A_CANT, 0);
	class_addmethod(c, (method)DCBlockerSetup,	"multicore.setup",	A_CANT, 0);
	class_addmethod(c, (method)DCBlockerConnect,	"multicore.connect",	A_OBJ, A_LONG, 0);
```

You should recognize the *multicore.reset* and *multicore.setup* messages.  Those are exactly the same as they were for our oscillator.  We now also have a *multicore.connect* message.  The oscillator was generating a signal but has no signal inputs, so it had no need for a *multicore.connect* message.  Any object that requires an input, however, will require this message binding.  How that method is implemented?

```
TTErr DCBlockerConnect(DCBlockerPtr self, TTMulticoreObjectPtr audioSourceObject, long sourceOutletNumber)
{
	return self->multicoreObject->connect(audioSourceObject, sourceOutletNumber);
}
```

We simply wrap a call to our peer object’s *connect* method, sending the audioSourceObject (which is the peer object that the **jcom.oscil≈** object sent us), and the outlet number from which that object was sent.  If you compare this to the *connect* message from the Ruby example in [Designing an Audio Graph](/74objects/2010/02/03/designing-multicore.html), it may illuminate the process.

### Some Final Details

The example code that we’ve seen from Jamoma Audio Graph demonstrates the passing of custom data (pointers to C++ objects) from one object to the next through the *multicore.connect* message. Because we are sending this custom data type, and not all inlets of all objects will understand this data type, it would be nice if we could protect users from hooking up the objects in a way that will not function.  For this task, Max makes it possible to give outlets type information.  When the type of an outlet is specified, a user will not be able to connect the patch cord to any inlet that doesn’t accept the specified message. To get this functionality, in DCBlockerNew(), we create our outlet like this:

```
self->multicoreOutlet = outlet_new(self, "multicore.connect");
```

So instead of the customary *NULL* for the argument to outlet_new(), we specify that this outlet will be sending only *multicore.connect* messages.

## Surfacing for Air

Jamoma Audio Graph provides a fairly intense example of passing custom data types in Max.  However, it presents not just the basics of how you would pass a pointer, but rather a context for why you might want to pass a custom type, and a real-world example to show what you can do.  I think that objective has been accomplished.
