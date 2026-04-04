---
layout: post
title: "Efficiently Blue"
date: 2003-10-30
categories: electrotap
source: "Originally published on electrotap.com"
---

Electrotap Weblog

			[![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_01.gif)](http://www.electrotap.com/)

			[![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_02.gif)](http://www.electrotap.com/)

			![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_03.gif)

			[![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_04.gif)](/web/20041021070122/http://www.electrotap.com/taptools/)

			[![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_05.gif)](/web/20041021070122/http://www.electrotap.com/jade/)

			[![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_06.gif)](/web/20041021070122/http://www.electrotap.com/chrome/)

			![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_07.gif)

			![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_08.gif)

			![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_09.gif)

			![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_10.gif)

			![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_11.gif)

			[![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_12.gif)](/web/20041021070122/http://www.electrotap.com/teabox/)

			[![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_13.gif)](/web/20041021070122/http://www.electrotap.com/sensors/)

			[![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_14.gif)](/web/20041021070122/http://www.electrotap.com/cables/)

			![](/web/20041021070122im_/http://www.electrotap.com/images/header-comp_15.gif)

			

			

			

			

			

			

			

			

			

			

			

 	

 			  var d=new Date(); var weekday=new Array("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"); var monthname=new Array("Jan","Feb","Mar","Apr","May","Jun","July","Aug","Sep","Oct","Nov","Dec"); document.write(weekday[d.getDay()] + " "); document.write(monthname[d.getMonth()] + " "); document.write(d.getDate() + ", ");	document.write(d.getFullYear());

	 

 [Order](/web/20041021070122/http://www.electrotap.com/order/)

 [Electrotap Weblog](/web/20041021070122/http://www.electrotap.com/blog/)

 [User Discussion Forum](http://www.electrotap.net/mailman/listinfo/siliconprairie/)

 [Latest News](/web/20041021070122/http://www.electrotap.com/news/)

 [Articles](/web/20041021070122/http://www.electrotap.com/articles/)

 [Resources & Related Links](/web/20041021070122/http://www.electrotap.com/resources/)

 [Contact & Support](/web/20041021070122/http://www.electrotap.com/contact/)

				[Home](http://www.electrotap.com/)

## The Electrotap Weblog

The Electrotap Weblog is a blog for clearing ideas, brainstorming, and reflecting on life - basically those things that don't lend themselves well to more formal areas of our website. If you have any thoughts or reactions to any of our blog entries, please feel free to drop us a line...

[[Previous entry: "The music of Zygmunt Krauze"](http://www.electrotap.com/blog/entries/00000010.shtml)] [[Main Index](http://www.electrotap.com/blog/)] [[Next entry: "The death of the LFOs"](http://www.electrotap.com/blog/entries/00000012.shtml)]

**"Efficiently Blue"**

10/30/2003

While I'm waiting for the Windows Jitter SDK (so that I can get Tap.Tools 1.5 done), I've been continuing to play with ideas for Tap.Tools Blue. The last couple of days I've been working on some speed boosts...

The big speed boost I'm playing with right now involves using function pointers to replace switches and if statements in my code (known as branching). It turns out to make a rather dramatic improvement in performance. For example, 50 copies an svf filter object (like MSP's svf~) in MSP with a switch() statement runs at 54% of the CPU on my laptop. Using function pointers, the same 50 copies run at 12% cpu.

I had no idea the difference would be that big. There are also improvements in the panner, crossfade, wavetable oscillator, and other objects. It looks like Tap.Tools Blue is going to be a significant speed bump over the previous Tap.Tools - and I find that exciting!

-tap

  [About Us](/web/20041021070122/http://www.electrotap.com/about_us/) |
 [Privacy Policy](/web/20041021070122/http://www.electrotap.com/privacy/) | [Contact Us](/web/20041021070122/http://www.electrotap.com/contact/) |
 Copyright ©2004, Electrotap L.L.C.
