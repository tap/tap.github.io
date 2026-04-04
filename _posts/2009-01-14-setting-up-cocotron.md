---
layout: post
title: "Setting Up Cocotron"
date: 2009-01-14
categories: electrotap
source: "Originally published on electrotap.com"
---

#### Setting up the Environment

I was very happy to get a comment from the main impetus behind Cocotron the other day. He pointed out some important things that were not at all obvious:

	- The main Cocotron site is out of date
	- The best information can be had on the Cocotron mailing list (hosted by Google)

This information doesn’t seem to be all in one place, so I’ll summarize how I got Cocotron working (I’m on OS 10.5.1 and using Xcode 3 at the moment).

First, I checked out the Cocotron source via SVN from the URL provided here:

[http://code.google.com/p/cocotron/source](http://code.google.com/p/cocotron/source)

Second, You need to install the Cocotron Dev Tools (CDT) — this is what provides the ability to cross-compile from Xcode: meaning compile for Windows and the Mac both from Xcode. The link on the site is out of date, the only link I found to a more recent version was in the Cocotron forums. That link is:

[http://www.cocotron.org/Tools/Downloads/InstallCDT-2007-10-10.zip](http://www.cocotron.org/Tools/Downloads/InstallCDT-2007-10-10.zip)

Third, the forum details some extra steps required because Apple changed some things in Xcode 2.5/3.0. The relevant post to the forum is here:

[http://groups.google.com/group/cocotron-dev/browse_thread/thread/aa427e195be9a86a](http://groups.google.com/group/cocotron-dev/browse_thread/thread/aa427e195be9a86a)

What that says to do is run this in the terminal:

```
ditto ~/Library/Application\ Support/Xcode ~/Library/Application\ Support/Developer/Shared/Xcode
```

Fourth, you need to actually compile the Cocotron frameworks.

	- Build ApplicationServices.xcodeproj
	- Build AppKit.xcodeproj
	- Build Foundation.xcodeproj
	- Build Cocoa.xcodeproj

So that sets up the environment. Now what?

#### Actually building something

To test that it actually works (and to grab a sample Xcode project with the correct target settings), I downloaded the TextEditor example from this page:

[http://www.cocotron.org/Code/Examples/](http://www.cocotron.org/Code/Examples/)

I compiled the Mac version, and it worked fine.

So I compiled the Windows version, copied it to a flash drive to try on the Windows machine. Hmmm. Didn’t work because I needed to copy the frameworks (dlls) to the Windows directory in the application bundle. That is documented here:

[http://www.cocotron.org/Code/Documentation/Things_You_Need_To_Know/](http://www.cocotron.org/Code/Documentation/Things_You_Need_To_Know/)

It actually took a minute to find out where these build frameworks were located. The answer is in /Developer/Cocotron. So I copied the AppKit, Cocoa, and Foundation dlls into the same folder as TextEditor.exe. Over on Windows, TextEditor.exe launches, but any time I try to click on a menu it crashes.

That documentation I last referred to also said to copy the frameworks. It isn’t readily apparent what it is referring to, since there are several options. I choose to try the option that had the exact name as spelled in the documention — so that meant copying /System/Library/Frameworks/AppKit.framework and friends. One downside to this is that it made our nice little text editor leap from 4MB to over 100MB. The other downside is that it made no difference.

So it isn’t flawless, obviously. Of course by checking out from SVN I could have just gotten things at a bad time when a few things are out of sync or not fully tested.

For writing plug-ins to run in Max/MSP we are really just interested in using the Foundation though, and not the AppKit. So maybe I’ll have better luck with that when I try it…
