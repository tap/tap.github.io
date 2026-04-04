---
layout: post
title: "A Plugtastic Day"
date: 2014-11-13
categories: 74objects
source: "Originally published on 74objects.com"
---

Today is a glorious day.  First of all, the result of the last couple years of effort at Cycling ’74 was officially released.  The early reception on Max 7 has been more than fabulous!  Yay!

![](https://cycling74.com/wp-content/themes/c74-blog-style/img/max7/max7-img-workflow-9.png)

Not nearly as interesting, I have finally returned to [InboxZero](https://www.youtube.com/watch?v=z9UjeTMb3Yk) for the first time since my son was born over 4 years ago.  That’s a bit embarrassing and it’s been draining, but everything left hanging out in my inbox was actually interesting (at least I didn’t keep the trash).

What made this process interesting was that I could see trends.  The last three email threads were from different years, different contexts, and people.  All were related to C++ code generation and on-the-fly compilation.  This has been an area of extreme interest to me. The oldest email, from 2010, and perhaps the genesis of my interest in code generation was from the Plugtastic alpha testing.

## Plugtastic?

Unless you know me personally and have a good memory, you probably don’t know about Plugtastic.  Plugtastic took a Max patcher and exported it to an AudioUnit plug-in.  The trick is that all of the objects needed to be Jamoma Graph and AudioGraph objects.  So essentially it was a Jamoma AudioGraph using Max as the IDE.

When you exported the plug-in the graph was analyzed, C++ source code was generated from it, and then the code was compiled into the binary component.

Eventually, however, I was drawn in too many directions to keep the project going.  Most of the initial work I did while on paternity leave after my son was born.  But the demands of a second child, my work for Cycling ’74, continuing work on the core Jamoma infrastructure, and the need to engage my whole self (meaning physical activity and not just mental activity) ended up spreading me too thin.  That said, it’s still a project that I find interesting and compelling.

The source code is now incorporated into Jamoma.  If working on this project sounds interesting, that’s the place to look and to contribute your talents!
