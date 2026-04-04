---
layout: post
title: "Debugging Max externals on Windows"
date: 2010-04-09
categories: electrotap
source: "Originally published on electrotap.com"
---

I was working some earlier today on some tricky bugs in Jamoma on Windows. I never would have figured them out if I hadn’t been using a debugger. In doing this, I realized that Cycling ‘74 has posted excellent instructions on debugging using Xcode on a Mac, but there isn’t much in the way of instructions for debugging on Windows.

My development environment on Windows is the one suggested by Cycling ’74’s SDK: Microsoft Visual Studio (Visual C++). Like the Mac, debugging cannot be done using the full Max environment — you must use the Max runtime. Also like the Mac, you should do debug builds of your externals rather than release builds. For Jamoma there is a single ‘solution’ file that includes all of the projects for the various externals, so this is quite easy.

The Max Runtime on both platforms does not use the searchpath, which can make things a little trickier for testing. On the Mac you can create an alias to your folder full of externs or whatever and that works. Making a shortcut like this on Windows doesn’t work. You need to have your externals built into the Cycling ’74 folder somewhere.

After all of that set up, it is pretty much the same. You tell Visual C++ to start the debugger, it asks you to chose a program, and you choose the Max Runtime. Setting break-points and everything is the same, thought the interface is not quite as friendly.

Hopefully this will be useful information to have out there. I have recently heard people suggest that they didn’t know you could use the debugger on Windows. If you couldn’t, it would be very hard to fix problems like the ones I was working on today.
