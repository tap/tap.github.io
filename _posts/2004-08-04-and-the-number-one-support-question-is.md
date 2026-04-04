---
layout: post
title: "And the number one support question is..."
date: 2004-08-04
categories: electrotap
source: "Originally published on electrotap.com"
---

Electrotap Weblog

			[![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_01.gif)](http://www.electrotap.com/)

			[![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_02.gif)](http://www.electrotap.com/)

			![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_03.gif)

			[![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_04.gif)](/web/20041021083213/http://www.electrotap.com/taptools/)

			[![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_05.gif)](/web/20041021083213/http://www.electrotap.com/jade/)

			[![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_06.gif)](/web/20041021083213/http://www.electrotap.com/chrome/)

			![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_07.gif)

			![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_08.gif)

			![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_09.gif)

			![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_10.gif)

			![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_11.gif)

			[![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_12.gif)](/web/20041021083213/http://www.electrotap.com/teabox/)

			[![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_13.gif)](/web/20041021083213/http://www.electrotap.com/sensors/)

			[![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_14.gif)](/web/20041021083213/http://www.electrotap.com/cables/)

			![](/web/20041021083213im_/http://www.electrotap.com/images/header-comp_15.gif)

			

			

			

			

			

			

			

			

			

			

			

 	

 			  var d=new Date(); var weekday=new Array("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"); var monthname=new Array("Jan","Feb","Mar","Apr","May","Jun","July","Aug","Sep","Oct","Nov","Dec"); document.write(weekday[d.getDay()] + " "); document.write(monthname[d.getMonth()] + " "); document.write(d.getDate() + ", ");	document.write(d.getFullYear());

	 

 [Order](/web/20041021083213/http://www.electrotap.com/order/)

 [Electrotap Weblog](/web/20041021083213/http://www.electrotap.com/blog/)

 [User Discussion Forum](http://www.electrotap.net/mailman/listinfo/siliconprairie/)

 [Latest News](/web/20041021083213/http://www.electrotap.com/news/)

 [Articles](/web/20041021083213/http://www.electrotap.com/articles/)

 [Resources & Related Links](/web/20041021083213/http://www.electrotap.com/resources/)

 [Contact & Support](/web/20041021083213/http://www.electrotap.com/contact/)

				[Home](http://www.electrotap.com/)

## The Electrotap Weblog

The Electrotap Weblog is a blog for clearing ideas, brainstorming, and reflecting on life - basically those things that don't lend themselves well to more formal areas of our website. If you have any thoughts or reactions to any of our blog entries, please feel free to drop us a line...

[[Previous entry: "Tap.Tools and Max 4.5"](http://www.electrotap.com/blog/entries/00000023.shtml)] [[Main Index](http://www.electrotap.com/blog/)] [[Next entry: "Other Blogs..."](http://www.electrotap.com/blog/entries/00000025.shtml)]

**"And the number one support question is..."**

**08/04/2004

The number one support question I get for Tap.Tools usually goes something like this:

> I installed TapTools and it all looks correct, but when I want to use one of them, a message appears in max window : error: can't fragload tap.typecheck~...** (for example)...

The answer is pretty simple. It has to do with how Max works.

Max has something called a *searchpath*. What this is, is a smattering of folders and subfolders in which Max looks for objects and files when you use one in a Max patch. When call up a graphic or an external object (such as a Tap.Tools object) Max starts looking through these folders for it and then takes the first one with the name that you specified.

This means that if there are two versions of something in your searchpath, Max will take the first one. This often generates the infamous "can't fragload" errors. These errors occur if an older version of an object is called that is incompatible with a newer version of a library. For example, if you have a newer version of MSP and there is an old filtergraph~ external in your searchpath, you will get this error and be unable to use the filtergraph~ object.

Tap.Tools is the same way. If you have older objects floating around on your hard drive, they can create conflicts leading fragload errors. Fortunately, the solution is pretty simple. Search your hard drive for files with the same name. In our example that was "tap.typecheck~" - if you find multiple copies, trash them. Usually this will do the trick.

The other cause is if Max simply cannot find the library required by an object. In our case, this would mean that Max can't find the TapTools.lib or TapTools_lib.dll file - which should normally be in the same folder as the Max application. If you recently upgraded to a new version of Max, you may need to move this file to your new Max folder.

A couple of alternative locations exist too. Be sure to search your hard drive for conflicting versions if you continue to have problems with fragload errors.

That covers the gist of it. You can also search the [Max/MSP list archives](http://www.cycling74.com/community/index.html) for "fragload" and dig up all of the talk about related issues there.

Good luck!
 -Tim

  [About Us](/web/20041021083213/http://www.electrotap.com/about_us/) |
 [Privacy Policy](/web/20041021083213/http://www.electrotap.com/privacy/) | [Contact Us](/web/20041021083213/http://www.electrotap.com/contact/) |
 Copyright ©2004, Electrotap L.L.C.
