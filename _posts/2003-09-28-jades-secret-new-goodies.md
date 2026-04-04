---
layout: post
title: "Jade's secret new goodies..."
date: 2003-09-28
categories: electrotap
source: "Originally published on electrotap.com"
---

Electrotap Weblog

			[![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_01.gif)](http://www.electrotap.com/)

			[![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_02.gif)](http://www.electrotap.com/)

			![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_03.gif)

			[![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_04.gif)](/web/20041021060737/http://www.electrotap.com/taptools/)

			[![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_05.gif)](/web/20041021060737/http://www.electrotap.com/jade/)

			[![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_06.gif)](/web/20041021060737/http://www.electrotap.com/chrome/)

			![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_07.gif)

			![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_08.gif)

			![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_09.gif)

			![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_10.gif)

			![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_11.gif)

			[![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_12.gif)](/web/20041021060737/http://www.electrotap.com/teabox/)

			[![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_13.gif)](/web/20041021060737/http://www.electrotap.com/sensors/)

			[![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_14.gif)](/web/20041021060737/http://www.electrotap.com/cables/)

			![](/web/20041021060737im_/http://www.electrotap.com/images/header-comp_15.gif)

			

			

			

			

			

			

			

			

			

			

			

 	

 			  var d=new Date(); var weekday=new Array("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"); var monthname=new Array("Jan","Feb","Mar","Apr","May","Jun","July","Aug","Sep","Oct","Nov","Dec"); document.write(weekday[d.getDay()] + " "); document.write(monthname[d.getMonth()] + " "); document.write(d.getDate() + ", ");	document.write(d.getFullYear());

	 

 [Order](/web/20041021060737/http://www.electrotap.com/order/)

 [Electrotap Weblog](/web/20041021060737/http://www.electrotap.com/blog/)

 [User Discussion Forum](http://www.electrotap.net/mailman/listinfo/siliconprairie/)

 [Latest News](/web/20041021060737/http://www.electrotap.com/news/)

 [Articles](/web/20041021060737/http://www.electrotap.com/articles/)

 [Resources & Related Links](/web/20041021060737/http://www.electrotap.com/resources/)

 [Contact & Support](/web/20041021060737/http://www.electrotap.com/contact/)

				[Home](http://www.electrotap.com/)

## The Electrotap Weblog

The Electrotap Weblog is a blog for clearing ideas, brainstorming, and reflecting on life - basically those things that don't lend themselves well to more formal areas of our website. If you have any thoughts or reactions to any of our blog entries, please feel free to drop us a line...

[[Previous entry: "daydreaming about taptools..."](http://www.electrotap.com/blog/entries/00000002.shtml)] [[Main Index](http://www.electrotap.com/blog/)] [[Next entry: "Inspired by Auvi"](http://www.electrotap.com/blog/entries/00000004.shtml)]

**"Jade's secret new goodies..."**

**09/28/2003

Jade has been getting some work lately. Some time soon I'll be posting a version 1.5 beta that contains the new goodies, but they won't be documented initially - at least not in the first couple of betas.

The main new thing is how to build modules. With the addition of a pile of new modules, the process for making a module has been vastly simplified. There are two new objects which are used - sp.mhub and sp.mp. The mhub (module-hub) is not-quite-similar to the plugconfig object, while the mp (module-parameter) object is like the pp object if you are familiar with building Pluggo plugins with Max. When the beta comes out (or the release if you want to wait) you can take a look - there is a world of difference.

I've also been ironing a few bugs. I'm pretty proud of Jade 1.1, with all of the help from the people of the [siliconprairie list](http://www.sp-intermedia.com/group.html) - especially [John Hudak](http://www.johnhudak.net/) - it seems like it runs pretty darn solid. That makes me happy. Nevertheless, I did come across a few things this past summer when I was putting together my installation for SIGGRAPH with Jesse Allison, which I addressed at the time or flagged for later review. So those fixes will be implemented in 1.5 as well.

The real big change for Jade 1.5 is (sadly) that it will be available only for Mac OS X. The number of issues I've been fighting against just piled up too high to make the same app run in both places. If I had the money to hire some help I would love to continue support for OS 9, but I don't. This causes a couple of headaches. For example one of my pieces, Dark Forest, only runs properly in OS 9 at the moment. Bummer. It has just become too much of hassle to keep up the OS 9 side - for example, the laptop I'm typing this on won't boot into OS 9 at all...

Once I get the 1.5 beta humming, I may divulge a few details on what Jade 2 will look like. I don't anticipate a Jade 2 will actually be released for over a year, but it is fun to daydream and brainstorm.

Let me know your thoughts...
 -Tim

Replies: 2 comments**

"With the addition of a pile of new modules, the process for making a module has been vastly simplified."

woohooo!
bring it on, baby!!

Posted by [Oliver Delano](mailto:odelano@speakeasy.net) @ 10/02/2003 06:23 AM CST

The above might be misleading, I now realize. Much of that "pile" of new modules won't actually be released until Jade 2.

My brain gets a little confused with itself when I'm living in this netherworld where I'm developing 2 versions of something at the same time.

Anyway, the new module building utilities will be present. My hope is to release the first Jade 1.5 beta tomorrow.

g'night,
 -Tim

Posted by [Tim Place](mailto:tap@electrotap.com) @ 10/02/2003 11:40 PM CST

  [About Us](/web/20041021060737/http://www.electrotap.com/about_us/) |
 [Privacy Policy](/web/20041021060737/http://www.electrotap.com/privacy/) | [Contact Us](/web/20041021060737/http://www.electrotap.com/contact/) |
 Copyright ©2004, Electrotap L.L.C.
