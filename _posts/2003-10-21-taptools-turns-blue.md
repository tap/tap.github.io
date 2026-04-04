---
layout: post
title: "TapTools turns Blue"
date: 2003-10-21
categories: electrotap
source: "Originally published on electrotap.com"
---

Electrotap Weblog

			[![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_01.gif)](http://www.electrotap.com/)

			[![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_02.gif)](http://www.electrotap.com/)

			![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_03.gif)

			[![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_04.gif)](/web/20041021064449/http://www.electrotap.com/taptools/)

			[![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_05.gif)](/web/20041021064449/http://www.electrotap.com/jade/)

			[![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_06.gif)](/web/20041021064449/http://www.electrotap.com/chrome/)

			![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_07.gif)

			![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_08.gif)

			![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_09.gif)

			![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_10.gif)

			![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_11.gif)

			[![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_12.gif)](/web/20041021064449/http://www.electrotap.com/teabox/)

			[![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_13.gif)](/web/20041021064449/http://www.electrotap.com/sensors/)

			[![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_14.gif)](/web/20041021064449/http://www.electrotap.com/cables/)

			![](/web/20041021064449im_/http://www.electrotap.com/images/header-comp_15.gif)

			

			

			

			

			

			

			

			

			

			

			

 	

 			  var d=new Date(); var weekday=new Array("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"); var monthname=new Array("Jan","Feb","Mar","Apr","May","Jun","July","Aug","Sep","Oct","Nov","Dec"); document.write(weekday[d.getDay()] + " "); document.write(monthname[d.getMonth()] + " "); document.write(d.getDate() + ", ");	document.write(d.getFullYear());

	 

 [Order](/web/20041021064449/http://www.electrotap.com/order/)

 [Electrotap Weblog](/web/20041021064449/http://www.electrotap.com/blog/)

 [User Discussion Forum](http://www.electrotap.net/mailman/listinfo/siliconprairie/)

 [Latest News](/web/20041021064449/http://www.electrotap.com/news/)

 [Articles](/web/20041021064449/http://www.electrotap.com/articles/)

 [Resources & Related Links](/web/20041021064449/http://www.electrotap.com/resources/)

 [Contact & Support](/web/20041021064449/http://www.electrotap.com/contact/)

				[Home](http://www.electrotap.com/)

## The Electrotap Weblog

The Electrotap Weblog is a blog for clearing ideas, brainstorming, and reflecting on life - basically those things that don't lend themselves well to more formal areas of our website. If you have any thoughts or reactions to any of our blog entries, please feel free to drop us a line...

[[Previous entry: "Wrapping up @ BEK"](http://www.electrotap.com/blog/entries/00000008.shtml)] [[Main Index](http://www.electrotap.com/blog/)] [[Next entry: "The music of Zygmunt Krauze"](http://www.electrotap.com/blog/entries/00000010.shtml)]

**"TapTools turns Blue"**

10/21/2003

I've spent some time the last few days, here in Bergen, working on Tap.Tools 2 - which I am now calling Tap.Tools Blue. It has been very productive, here are some of the developments I've been working on...

I've added a couple of (c++) objects including tap_clip and tap_lfo and I've made more efficient objects of tap_zerox, etc. The Max externals which I build from these now begin with a "blue." prefix instead of a "tap." prefix. This allows me to sidestep some backwards compatibility issues - the "tap." prefix will remain in use for a set of abstractions I will build to keep backwards compatibility.

With some of the optimizations, blue.verb~ is now running at more than double the speed of the old tap.verb~. This is really exciting, though I know I can't expect this type of speed boost with all of the objects. This speed boost comes even with the addition of a number of new features, which I hope to make standard among the audio externals. These features at the moment are:

- dcblocking
- limiting
- clipping
- lfos
- wet/dry mix
- gain
- downsampling
- mute
- bypass

I've also updated the look and feel of the help patches; I think they are much clearer now.

Well, I've finished my dessert (a waffle with strawberries) and my coffee is about done too. Time get back to the code!

 -tap

  [About Us](/web/20041021064449/http://www.electrotap.com/about_us/) |
 [Privacy Policy](/web/20041021064449/http://www.electrotap.com/privacy/) | [Contact Us](/web/20041021064449/http://www.electrotap.com/contact/) |
 Copyright ©2004, Electrotap L.L.C.
