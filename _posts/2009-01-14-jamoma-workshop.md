---
layout: post
title: "Jamoma Workshop"
date: 2009-01-14
categories: electrotap
source: "Originally published on electrotap.com"
---

Last week we had an intensive development workshop for Jamoma in Brussels, Belgium. The workshop was hosted by iMAL under the direction of Yves Bernard.

![](/web/20090114034544im_/http://www.electrotap.com/images/BrusselsWorkshop1.jpg)

During the week our ranking at SourceForge soared up to round 2600. This places Jamoma in the top 2% of all open source projects on SourceForge in terms of current development activity. Impressive!

We accomplished a lot during the week. A brief summary of developments:

	- FunctionLib: a new library of mathematical functions that are accessible throughout the Jamoma system. A new jcom.map object uses this functionality to create a sort-of “super scale object” swiss-army-knife for patching.
	- RampLib: the ramp library has been re-written so that the shape of the ramp is function from the FunctionLib, as defined by the user. The RampUnit then defines the mechanism for driving ramps. In addition to scheduler-based and queue-based RampUnits we also created an async unit, which is particularly well-suited to Jitter modules.
	- DataspaceLib: In previous discussions this was the ‘UnitLib’ (a final name has not officially been settled). This is not yet implemented in parameters and messages, but we have a working prototype. It allows to say that a parameter or message belongs to a dataspace such as ‘gain’ and then that parameter or message will be able to handle input values in various formats (e.g. decibels, midi, linear, etc.).
	- Refining the OpenSoundControl namespace. In general we are making the namespace more consistent and rational. We also discussed a variety of issues in terms of OSC syntax and began preparing some papers on the subject. Of particular focus is the use in Jamoma of the ‘:’ character to denote that we are addressing the metadata of a node instead of the node value itself.
	- Interpolation between presets in each module. Also a variety of other features and improvements to the preset system.
	- Implemented setting and getting parameters of attributes of parameters (for example: the power parameter of the ramp/function attribute of the bitdepth module-parameter).
	- Lots of bug fixes (and probably the introduction of some new bugs too ;-)
	- Charting out future development: this includes support for node-based tree structures in both the namespace and the patcher messaging model. We also have more work to do on wildcard support in our OSC parsing, and a variety of other tasks.
	- Implementation of an ‘Oblique Strategies’ component done by Alexander. Whenever we are in a bind for a decision to make, we now have the perfect component to provide the answer. “Do the words need changing?”
	- New build script for the Mac that doesn’t produce miles of undigestible output in the terminal. When we left Brussels it was having some difficulty, but it should be in working order now.

Overall I think the workshop was a tremendous success. I though we would get a lot done, but we far exceeded my expectations. We also had a good public presentation on the final day (Saturday). After doing this I think it became more than abundantly clear that we should have done this at the beginning instead. Next time we should definitely take this into account. We got some very good feedback from the public presentation part, especially from Olivier Pasquet.

We also brought a number of people up to speed on the less flashy aspects of Jamoma, such as how to do your own build from the latest source. While this may sound dry, it actually will be a tremendous help in moving Jamoma forward by having more people spreading out the work, and allow us to do even more cool things. Hooray!

![](/web/20090114034544im_/http://www.electrotap.com/images/BrusselsWorkshop2.jpg)

Additional information about the workshop can be found here:

	- [http://www.arj.no/2007/12/10/jamoma-workshop-in-brussels/](http://www.arj.no/2007/12/10/jamoma-workshop-in-brussels/)
	- [http://www.jamoma.org/jamoma_blog//54](http://www.jamoma.org/jamoma_blog//54)
