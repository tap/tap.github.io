---
layout: post
title: "daydreaming about taptools..."
date: 2003-09-27
categories: electrotap
source: "Originally published on electrotap.com"
---

Electrotap Weblog

			[![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_01.gif)](http://www.electrotap.com/)

			[![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_02.gif)](http://www.electrotap.com/)

			![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_03.gif)

			[![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_04.gif)](/web/20041021052423/http://www.electrotap.com/taptools/)

			[![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_05.gif)](/web/20041021052423/http://www.electrotap.com/jade/)

			[![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_06.gif)](/web/20041021052423/http://www.electrotap.com/chrome/)

			![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_07.gif)

			![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_08.gif)

			![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_09.gif)

			![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_10.gif)

			![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_11.gif)

			[![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_12.gif)](/web/20041021052423/http://www.electrotap.com/teabox/)

			[![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_13.gif)](/web/20041021052423/http://www.electrotap.com/sensors/)

			[![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_14.gif)](/web/20041021052423/http://www.electrotap.com/cables/)

			![](/web/20041021052423im_/http://www.electrotap.com/images/header-comp_15.gif)

			

			

			

			

			

			

			

			

			

			

			

 	

 			  var d=new Date(); var weekday=new Array("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"); var monthname=new Array("Jan","Feb","Mar","Apr","May","Jun","July","Aug","Sep","Oct","Nov","Dec"); document.write(weekday[d.getDay()] + " "); document.write(monthname[d.getMonth()] + " "); document.write(d.getDate() + ", ");	document.write(d.getFullYear());

	 

 [Order](/web/20041021052423/http://www.electrotap.com/order/)

 [Electrotap Weblog](/web/20041021052423/http://www.electrotap.com/blog/)

 [User Discussion Forum](http://www.electrotap.net/mailman/listinfo/siliconprairie/)

 [Latest News](/web/20041021052423/http://www.electrotap.com/news/)

 [Articles](/web/20041021052423/http://www.electrotap.com/articles/)

 [Resources & Related Links](/web/20041021052423/http://www.electrotap.com/resources/)

 [Contact & Support](/web/20041021052423/http://www.electrotap.com/contact/)

				[Home](http://www.electrotap.com/)

## The Electrotap Weblog

The Electrotap Weblog is a blog for clearing ideas, brainstorming, and reflecting on life - basically those things that don't lend themselves well to more formal areas of our website. If you have any thoughts or reactions to any of our blog entries, please feel free to drop us a line...

[[Previous entry: "What's on tap?"](http://www.electrotap.com/blog/entries/00000001.shtml)] [[Main Index](http://www.electrotap.com/blog/)] [[Next entry: "Jade's secret new goodies..."](http://www.electrotap.com/blog/entries/00000003.shtml)]

**"daydreaming about taptools..."**

**09/27/2003

Hey, here's what I think is in the pipeline for [Tap.Tools](http://www.sp-intermedia.com/taptoolsmax/). New info and plans for expanding what Tap.Tools are.

First up, the Tap.Tools. I've been working the past couple of months on Tap.Tools 2, which should be a big step up from Tap.Tools 1.5 - which I've also been working on (already Tap.Tools 1.5 has about twice the number of objects of Tap.Tools 1.0). What's the big deal about Tap.Tools 2?

First, the notion of what a Tap.Tools object is has changed. A Tap.Tools object is no longer a Max/MSP/Jitter object. It is a C++ object**. Many of them will be wrapped-up and distributed as Max objects, but Tap.Tools objects could easily be used in other contexts. I've just built an Audio Units plugin in C++ recently as one example. Other options would be making versions of Tap.Tools for Pd, Reaktor, jMax, SuperCollider, etc. using the nice an neat C++ objects. Perhaps I could even distribute just the C++ library, but I'm not sure at the moment - if anyone has thoughts on this, let me know!

This also makes building new objects easier because I they all inherit from a Tap.Tools base class that provides some things for free. The benefits of abstracting the DSP objects from the Max objects are many!

One initial concern was **performance**. After a number of experiments I've been able to structure the Tap.Tools classes to be very efficient. When compared to the same DSP code implemented directly in the Max object, there is a very slight performance hit (about 0.01% on the CPU meter). When combining several of the new Tap.Tools objects inside of one Max object, compared to several Max objects doing the same thing, the new library is much faster than doing things the old way. That should mean some of the most intensive Tap.Tools Max objects will be seeing speed increases in the future as they are turned into externals (tap.verb~, tap.shift~, etc.).

I also want to add attributes for all objects, but eliminate the need for the dependency on Jitter... Not sure what will shake out there.

Are there any **recommendations from people about Tap.Tools?** No promises on implementing them, but I am listening to all ideas...

later,
-Tim

**Replies: 1 Comment**

what an odd time we live in: max/msp on windows, and tap.tools possibly in reaktor?!?

the c++ library sounds like a major step forward. very exciting. does this essentially mean you have a library of c++ functions that are called by individual max objects, and then respond with the result? what are the thread related implications? ie would all the c++ tap.tools code be running in an entirely different thread than max's?

i would love to see a kick-*** compressor (multi-band?) as a part of tap.tools. how's about some analogue like saturation as well? i realize this can be done with waveshaping, but i haven't been able to find a good resource for transfer functions...

my main max-fantasy (video) involves distributed processing. i realize processors are getting faster, but that means i just use more objects... distributing processing over multiple machines (via bucket, or frame), though, would be limitless. i've got some ideas on this, but performance and syncing are major issues. but a seperate c++ thread? hmmmmm.

oliver.

Posted by [Oliver Delano](mailto:odelano@speakeasy.net) @ 10/03/2003 06:03 AM CST

  [About Us](/web/20041021052423/http://www.electrotap.com/about_us/) |
 [Privacy Policy](/web/20041021052423/http://www.electrotap.com/privacy/) | [Contact Us](/web/20041021052423/http://www.electrotap.com/contact/) |
 Copyright ©2004, Electrotap L.L.C.
