---
layout: post
title: "Automated Installation Woes"
date: 2010-04-09
categories: electrotap
source: "Originally published on electrotap.com"
---

I ran into an issue in an installation recently where I had my application built in Max run automatically on startup – by adding it to the startup items in the User Preferences. The application would make a jitter window go full screen and begin processing. Intermittently the computer, upon startup, would bring the finder window to the forefront after my application had been run. I’m not sure what in the startup routine was causing it to bring the finder to the front, but it would also bring the menubar in front of my beautiful video.

After hunting for a solution through the Max lists, I couldn’t find anything about bringing the max application to the front. A bit more searching led me to this solution.

[http://www.macosxhints.com/article.php?story=20060717092834812](http://www.macosxhints.com/article.php?story=20060717092834812)

For my problem, I could simply delay launching the app until the startup routine was done and then proceed. My Automator routine looked like this:

	- Pause: 30 seconds
	- Launch Application Indy_Pool_Blue.app

I saved the script as an app and then added it to my startup items.

The other solution described on the page above used an applescript to solve the issue. Combined with tap.applescript, I could see this solving a wide variety of issues.

```
delay 20
tell application "Indy_Pool_Blue"
 launch
end tell
```

You could also use applescript to make the application active. After a little searching I found this gem:

```
tell application "iTunes"
activate
end tell
```

I hope this helps somebody else out in making their installation more reliably autonomous.
