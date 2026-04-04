---
layout: post
title: "The death of the LFOs"
date: 2003-11-04
categories: electrotap
source: "Originally published on electrotap.com"
---

Electrotap Weblog

			[![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_01.gif)](http://www.electrotap.com/)

			[![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_02.gif)](http://www.electrotap.com/)

			![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_03.gif)

			[![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_04.gif)](/web/20041021072710/http://www.electrotap.com/taptools/)

			[![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_05.gif)](/web/20041021072710/http://www.electrotap.com/jade/)

			[![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_06.gif)](/web/20041021072710/http://www.electrotap.com/chrome/)

			![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_07.gif)

			![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_08.gif)

			![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_09.gif)

			![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_10.gif)

			![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_11.gif)

			[![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_12.gif)](/web/20041021072710/http://www.electrotap.com/teabox/)

			[![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_13.gif)](/web/20041021072710/http://www.electrotap.com/sensors/)

			[![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_14.gif)](/web/20041021072710/http://www.electrotap.com/cables/)

			![](/web/20041021072710im_/http://www.electrotap.com/images/header-comp_15.gif)

			

			

			

			

			

			

			

			

			

			

			

 	

 			  var d=new Date(); var weekday=new Array("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"); var monthname=new Array("Jan","Feb","Mar","Apr","May","Jun","July","Aug","Sep","Oct","Nov","Dec"); document.write(weekday[d.getDay()] + " "); document.write(monthname[d.getMonth()] + " "); document.write(d.getDate() + ", ");	document.write(d.getFullYear());

	 

 [Order](/web/20041021072710/http://www.electrotap.com/order/)

 [Electrotap Weblog](/web/20041021072710/http://www.electrotap.com/blog/)

 [User Discussion Forum](http://www.electrotap.net/mailman/listinfo/siliconprairie/)

 [Latest News](/web/20041021072710/http://www.electrotap.com/news/)

 [Articles](/web/20041021072710/http://www.electrotap.com/articles/)

 [Resources & Related Links](/web/20041021072710/http://www.electrotap.com/resources/)

 [Contact & Support](/web/20041021072710/http://www.electrotap.com/contact/)

				[Home](http://www.electrotap.com/)

## The Electrotap Weblog

The Electrotap Weblog is a blog for clearing ideas, brainstorming, and reflecting on life - basically those things that don't lend themselves well to more formal areas of our website. If you have any thoughts or reactions to any of our blog entries, please feel free to drop us a line...

[[Previous entry: "Efficiently Blue"](http://www.electrotap.com/blog/entries/00000011.shtml)] [[Main Index](http://www.electrotap.com/blog/)] [[Next entry: "A new resource for Max"](http://www.electrotap.com/blog/entries/00000013.shtml)]

**"The death of the LFOs"**

**11/04/2003

It looks like Tap.Tools blue will not be seeing the standard LFO implementation that I dreamed up on this Blog several weeks ago. Here's why:

First off, it is a royal pain in the rear to implement intelligently. The code is becoming convoluted as I try to implement it, and that is not a good thing.

To top that off, I'm finding that the CPU can bog down significantly because I'm doing things with signals that would normally be reserved for Max control messages. An example: The blue.verb~ external is currently running around 5-6% on my cpu. Turn on the LFO and apply it to the base delay time and the cpu usage jumps to 20%. That just isn't going to fly.

It seemed like a nice idea to have this stuff all built in. But the strength of max is to wire and modulate parameters with each other - so I think I'll leave that which is good for Max to Max. Comments?

Replies: 2 comments**

hi tim-
thats too bad. sounded almost too good to be true...
howzabout including the lfo's, etc in tap.blue and allowing an easy way to hook 'em up-- maybe using named instances of the tap object you want to modulate?

Posted by [Oliver Delano](mailto:odelano@speakeasy.net) @ 11/14/2003 01:14 PM CST

Well, I guess I shouldn't rule anything out... I'm feeling a bit burned out on the thing at the moment. I'm going to keep pluging away at other things and maybe I'll come back to it.

Thanks for the feedback - it's good to know how others think about these things!

-tap

Posted by [Tim Place](mailto:tap@electrotap.com) @ 11/14/2003 10:58 PM CST

  [About Us](/web/20041021072710/http://www.electrotap.com/about_us/) |
 [Privacy Policy](/web/20041021072710/http://www.electrotap.com/privacy/) | [Contact Us](/web/20041021072710/http://www.electrotap.com/contact/) |
 Copyright ©2004, Electrotap L.L.C.
