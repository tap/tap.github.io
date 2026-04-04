---
layout: post
title: "Multichannel Audio Signals in Max"
date: 2009-01-14
categories: electrotap
source: "Originally published on electrotap.com"
---

![](/web/20090114023056im_/http://www.electrotap.com/images/lydbaer-sketch.jpg)

A sub-project within the overall Jamoma development has been on working with multichannel systems in a flexible and dynamic way. Doing this with static single-channel MSP signals has been one area that is particularly challenging. To this point we have been using a [very clever hack](http://electrotap.net/blog/show/108) by Trond Lossius to ease the patching of multichannel and ambisonic modules in Jamoma.

At the workshop in Oslo last week we began work on the ‘lydbær’ (soundberry) project within Jamoma. This is a proper implementation of a multichannel, 64-bit, flexible vector size, and dynamically re-patchable signal processing chain based on [TTBlue](http://ttblue.org/).

![](/web/20090114023056im_/http://www.electrotap.com/images/lydbaer-op.png)

While we just began, and we have not yet completed a specification, the proof of concept shown above is actually functioning. The example below shows that you can split the outputs to multiple destinations (fanning patchcords) as well as combining patchcords at an inlet to have them automatically summed.

![](/web/20090114023056im_/http://www.electrotap.com/images/lydbaer-initialsuccess.png)
