---
layout: post
title: "Inventing a New Project Format for C/C++"
date: 2012-07-25
categories: 74objects
source: "Originally published on 74objects.com"
---

This past spring we rolled-out a new way compile our source code in the [Jamoma](http://jamoma.org/) project. After a number of months of real-world use in managing our code, the transition has been big success. I here propose that this system be moved beyond Jamoma for use by other software developers who care about portability, ease of use, ease of maintenance, and requisite flexibility.

## The Maintenance Nightmare

We had been using Xcode projects on the Mac, Visual Studio on Windows, Makefiles on Linux, etc. The problem with maintaining all these independent projects is that a change in one requires changes in the others, which typically are changes that need to be made by someone running a different operating system. In the mean-time, the build is broken on that other platform. Or maybe instead the developer was extra conscientious and checked-in the change on a branch, using this work-flow:

- create new branch
- make change (add a file to an Xcode project)
- check-in the change
- check out the branch on a windows computer
- make the change in visual studio
- check-in the change
- check out the branch on a linux computer
- fix and test the Makefile
- check-in the change
- merge the branch to master
- delete the temporary branch
- delete the temporary branch on the Mac
- delete the temporary branch on the Windows machine
- delete the temporary branch on the Linux box
- Realize that, oops, forgot to update the iOS target
- etc.

In an open source project like Jamoma most of the work is done on an unpaid/volunteer basis.  That means a lot of these steps don’t happen in a timely manner and the result is a project that is in a state of perpetual brokenness.  (One side-effect of that state is that the project is hostile to potential new developers joining).

With our the new **YAMLProj** format, you simply add the file in the project on one platform, and it is added for all of the other platforms.

This is example is the **YAMLProj** for Jamoma’s [TrajectoryLib](https://github.com/jamoma/JamomaDSP/tree/master/extensions/TrajectoryLib) extension.

```
sources:
 - TrajectoryLib.cpp
 - TTTrajectory.cpp
 - sources/Bean2D.cpp
 - sources/Hypocycloid2D.cpp
 - sources/Bspline2D.cpp
 - sources/Limacon2D.cpp
 - sources/Scarabaeus2D.cpp
 - sources/Butterfly2D.cpp
 - sources/Linear2D.cpp
 - sources/Slinky3D.cpp
 - sources/Linear3D.cpp
 - sources/Spiral3D.cpp
 - sources/Circular2D.cpp
 - sources/Lissajous2D.cpp
 - sources/Torus3D.cpp
 - sources/CircularHelix3D.cpp
 - sources/Lissajous3D.cpp
 - sources/Viviani3D.cpp
 - sources/Epitrochoid2D.cpp
 - sources/LogSpiral2D.cpp
 - sources/Gear2D.cpp
 - sources/Rose2D.cpp

includes:
 - "."
 - "includes"
 - "../../library/includes"
 - "../../../Foundation/library/includes"

libraries:
 - FOUNDATION
 - DSP
```

To drive the point home, here is sequence of steps for adding a file to a project, as compared to the 16+ step behemoth presented at the outset:

- make change (add a source file to the project)
- check-in the change

No need to create a branch to prevent breaking the build on other platforms (though you could if you thought there were going to be problems).

## Ease of Use

In addition to easing the maintenance burden, the system must also be easy for developers of all experience levels to use.  Face it: not all developers are command-line junkies; a lot of people develop software by dragging files from the Mac Finder into Xcode and clicking the icon.  That’s a good workflow if you want a non-intimidating experience that gets a quick result and makes you feel good.

Without writing an app for umpteen platforms, some of which wouldn’t even make sense, we’re not going to have drag-n-drop gui windows.  Drag-n-drop gui windows can also be incredibly difficult to use and present the user with a complete cognitive cacophony that does *not* feel easy to use (e.g. [Eclipse](http://www.eclipse.org/)).  So dispensing with that we have text files.

Text files can be intimidating.  They have been used, misused, and abused in various ways.  Some people even have baggage associated with them.  The text file needs some sort of structure to it.  Some text file formats for creating structure are hideous (ever looked at pbxproj file?), some are unforgiving (JSON), others are just tedious (XML).  We chose YAML.  It doesn’t require massive amounts of decoration, it is clear, it is easy and straightforward.

### But what about CMake?

[CMake](http://www.cmake.org/) sucks.  There.  I said it.  I’ll upset some people.  Whatever.

It’s too complicated.  We don’t need our projects to fly the [International Space Station](http://www.nasa.gov/mission_pages/station/main/index.html).  We need to compile a few source files and have it in a text file format that is not intimidating.  In order to use CMake you probably have to compile some other open-source software, which might fail with errors that only the devs understand.  Then you need it to work for compiling a lib for the iOS or some new platform that we haven’t heard about yet and you’re screwed.

Sorry.  No CMake.  We went down that road with Jamoma and threw it out in disgust. Don’t bother trying say anything contrary.  I won’t be listening.

To the contrary, **YAMLProj** is super-simple, exceptionally clear, uncomplicated, and unintimidating.  All the while still addressing the cross-platform maintenance issues in a reasonable way.

## Making it Go

The implementation is a simple Ruby script.  The Ruby script reads the **YAMLProj**, parses the YAML to generate the intermediate format and the compiles from the intermediate format.  On Windows the intermediate format it generates is a VisualStudio project.  On the Mac and Linux it generates a good old Makefile.

For completeness’ sake, here is the Makefile generated on a Mac for the aforementioned TrajectoryLib in Jamoma.

```
NAME = TrajectoryLib

CC_32 = g++-4.2 -arch i386
CC_64 = g++-4.2 -arch x86_64

#########################################

SRC = TrajectoryLib.cpp
SRC += TTTrajectory.cpp
SRC += sources/Bean2D.cpp
SRC += sources/Hypocycloid2D.cpp
SRC += sources/Bspline2D.cpp
SRC += sources/Limacon2D.cpp
SRC += sources/Scarabaeus2D.cpp
SRC += sources/Butterfly2D.cpp
SRC += sources/Linear2D.cpp
SRC += sources/Slinky3D.cpp
SRC += sources/Linear3D.cpp
SRC += sources/Spiral3D.cpp
SRC += sources/Circular2D.cpp
SRC += sources/Lissajous2D.cpp
SRC += sources/Torus3D.cpp
SRC += sources/CircularHelix3D.cpp
SRC += sources/Lissajous3D.cpp
SRC += sources/Viviani3D.cpp
SRC += sources/Epitrochoid2D.cpp
SRC += sources/LogSpiral2D.cpp
SRC += sources/Gear2D.cpp
SRC += sources/Rose2D.cpp

#########################################

INCLUDES = -I.
INCLUDES += -Iincludes
INCLUDES += -I../../library/includes
INCLUDES += -I../../../Foundation/library/includes

#########################################

LIBS = ../../../Foundation/library/build/JamomaFoundation.dylib
LIBS += ../../../DSP/library/build/JamomaDSP.dylib

#########################################

OPTIMIZATION_DEBUG = -O0
OPTIMIZATION_RELEASE = -O3

OPTIONS = -shared -msse3 -mfpmath=sse -gdwarf-2

WARNINGS = -Wall -Wno-unknown-pragmas -Wno-trigraphs
DEFINES = -DTT_PLATFORM_MAC

#########################################

CFLAGS = $(OPTIONS) $(DEFINES) $(INCLUDES) $(WARNINGS)
LDFLAGS = $(LIBS)

#########################################

Debug:
	mkdir -p build
	mkdir -p /usr/local/jamoma/extensions
	touch /usr/local/jamoma/extensions
	$(CC_32) $(SRC) $(LDFLAGS) $(CFLAGS) $(OPTIMIZATION_DEBUG) -o build/$(NAME)-i386.ttdylib
	$(CC_64) $(SRC) $(LDFLAGS) $(CFLAGS) $(OPTIMIZATION_DEBUG) -o build/$(NAME)-x86_64.ttdylib
	lipo build/$(NAME)-i386.ttdylib build/$(NAME)-x86_64.ttdylib -create -output build/$(NAME).ttdylib
	cp build/$(NAME).ttdylib /usr/local/jamoma/extensions

Release:
	mkdir -p build
	mkdir -p /usr/local/jamoma/extensions
	$(CC_32) $(SRC) $(LDFLAGS) $(CFLAGS) $(OPTIMIZATION_RELEASE) -o build/$(NAME)-i386.ttdylib
	$(CC_64) $(SRC) $(LDFLAGS) $(CFLAGS) $(OPTIMIZATION_RELEASE) -o build/$(NAME)-x86_64.ttdylib
	lipo build/$(NAME)-i386.ttdylib build/$(NAME)-x86_64.ttdylib -create -output build/$(NAME).ttdylib
	cp build/$(NAME).ttdylib /usr/local/jamoma/extensions

clean:
	rm -f *.o
	rm -rf build

install:
	cp build/$(NAME).ttdylib /usr/local/jamoma/extensions
```

This is using the Apple Developer Tools-installed GCC version 4.2.  If the Ruby script had detected a newer version of GCC or the Intel (ICC) compiler it would have used that instead.  The compiler can be defined in the **YAMLProj** if a specific compiler is required or considered more desirable.

### What about Editing and Debugging?

On Windows a VisualStudio project is created, so there’s nothing to worry about there.  Developers on the Mac are used to being spoiled with the Xcode debugger and it first glance it looks like we’re throwing out Xcode projects here to use Makefiles.

In fact, we don’t have to throw out Xcode projects — we just need to change them a bit.  Xcode projects can be configured to use as a source code editor that builds using the Makefile generated by our **YAMLProj**.  It can then debug the project, complete with breakpoints.

Here is a quick video showing how to do it.

## Next Steps

The existing implementation is primarily in a single Ruby method called generate_makefile which you can [read on Github](https://github.com/jamoma/JamomaSupport/blob/b8cb5f0a4c816c723c7e4b433eb5e27c28bb813b/jamomalib.rb#L349). The problem with this implementation is that it is tied directly to Jamoma.  To be of general use it needs to be factored-out and made to standalone.  It needs a group of people peer-reviewing it.  It needs real-world use outside of Jamoma.

So who will do it?  YAMLProj awaits your arrival.
