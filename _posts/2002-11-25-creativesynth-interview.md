---
layout: post
title: "CreativeSynth Interview"
date: 2002-11-25
categories: electrotap
source: "Originally published on electrotap.com"
---

We’re opening a time capsule here.  It is now 2009 and a lot of things have changed — and the excellent CreativeSynth.com is no longer with us.  I obtained permission from Mr. CreativeSynth himself, Darwin Grosse, to re-publish this interview from November 2002.

Back in 2002 (7 years ago!) I was fairly ‘green’ as a coder / designer / artist / entrepreneur / musician and was feeling my way around with Tap.Tools, which was also in a very different place than it is today.  And Jade, which has since evolved into [Jamoma](http://jamoma.org/), had just been released.

***Tim Place****, creator of the [Tap.Tools](http://shop.electrotap.com/products/taptools) and Jade development environment, allowed himself to be grilled by your intrepid editor. Since Tim is a bit hesitant to blow his own horn too greatly, I’ll say it – his development tools are great, and Jade is a serious music environment that needs to be examined by anyone doing serious Max/MSP work.*

*This interview was done via email – thanks to Tim for his willingness to go through the process…*

## Tim, why don’t you give us a quick overview of your background?

My primary activity is composition. The doctoral diploma I’m working on will say ‘Composition,’ when I’m done with the degree here at the University of Missouri – Kansas City. I think most people who know me though, know that I’m not here to just write traditional pieces for orchestras and chamber groups and whatsuch.

My father is a very creative electrical engineer, which I would characterize as [possessing] values such as creative problem solving, practical innovation, and inventive spirit. They apply to my general approach to things in life, but most significantly to my music. And while there are times that my musical vision can be realized fully within the acoustic world (usually with fairly extended avant-garde techniques), typically I find adding another element to be critical to communicating my musical vision.

## How well accepted is your “vision”? Do you find the academic community willing to embrace this perspective?

Well, academia is an odd place. I’m extremely fortunate to be where I am because I do have a great deal of flexibility in how I approach my degree – much more so than at other institutions. Given that however, it can still be an uphill battle at times.

In the U.S. it is generally perceived that the serious composer may incorporate electronics, but the bulk of their work will be with purely acoustic forces. So if I’m hoping to get a composer/composition gig at an academic institution, there is concern that a body of works which mostly involves electronics will reduce my ‘marketability.’ I noted this in some of the attitudes of faculty when I was selecting a school for my doctorate – even though all of my works with electronics involve live performers, most of which are playing orchestral instruments! In Europe there seems to be a much more open-minded attitude about this.

My mentors at UMKC want to see me succeed, and knowing the academic market, try to encourage me to balance my portfolio, etc. So they’ll challenge me at the outset of a project – I think that is good – but then no matter what I choose to do, they fully support me in every imaginable way once I get going on something. Like I said earlier, I’m extremely fortunate to be here.

## It sounds like a great environment. Now, you’ve been an active Max/MSP developer – creating tools like the Tap.Tools and the new Jade development environment. How do you maintain the balance between academic work and commercial development?

Ask me again in 6 months. I’m not really sure… The big unknown is Jade. I’ve tried to be thorough with the documentation, but there are probably ways it can improve – which I won’t know about until people tell me about it. Also, since it has just been released I can’t really gauge how many people will be interested enough to buy it. I guess Tap.Tools doesn’t worry me too much because I’ve been supporting that for nearly two years as public alpha and beta versions.

## The Tap.Tools are a popular set of objects for Max/MSP users. Tell us about their development.

In attempting to bring my interactive music to life there have been a few stages of development. The first is just learning Max/MSP. I more or less had to learn Max/MSP on my own – and it took a good 6 months before I actually made it get through a piece of music. Where I am now (UMKC) we teach Max/MSP as a course, but for most students it is still unreasonable to expect them to be able to grapple with all of the issues involved in creating time-based art with the software. It has been my experience that it still takes another semester for them to really make it fly.

One of the things that I found helped this in my own development was downloading objects others had built to do x, y, or z. I could use it out of the box, but I could open them up and modify them too. This was very helpful, but still there was no accessible (i.e. free or cheap) set of higher level stuff – pitch shifting, compression, reverb, etc. So I ended basically building all of this stuff for my music. Somewhere along the way I guess I picked up enough C to start building externals to do a few of the things I wanted.

After a couple of years, I finally felt like I could do something with Max – in part because I had built this little arsenal of tools. So after gaining so much by lurking on the Max Listserve and downloading others’ work, I finally had the opportunity to give back and make my objects available.

## Well, that makes it sound like it’s a hodge-podge of objects, when in practice it “feels” much more coherent than that. What are the major categories of objects that you provide – and what was the impetus in making them?

[Laughing] I’ve been exposed! The tap.tools really did just start as a hodge-podge of objects. I guess they’ve developed quite a bit since then. One of my chief concerns is working with audio, so there is an emphasis on that. Within that there are ‘high-level’ objects (effects, processors) and ‘low-level* objects that are typically the building blocks I use to create the high-level processes.

Here is an example. I wanted a reverb to use in a piece. It couldn’t really be a VST plugin because it would be potentially illegal to distribute the plugin with my score and software. So I did some digging and decided that I wanted to combine a few algorithms based on one by J. A. Moorer. The problem was that it needed a comb filter with a low-pass filter in the feedback loop. So I made an external, [tap.comb~] (with some generous help from David Zicarelli), and then made a patch for reverb, tap.verb~, which is built around the external.

Another object is tap.crossfade~, essential for creating a wet/dry mix control. This one I could have done as a patch, but the external is faster and more flexible. The same thing with tap.pan~. When you use over a hundred of these in a project, that speed really adds up.

So beyond the lower level building blocks and the higher-level effects and processors there are objects I built for control purposes. This might be to take an audio signal and generate a control (tap.sift~ or tap.bink~ for example), or to take a video signal using Jitter to do the same (tap.jit.motion+ for example). Some additional objects just help me manage that control stream.

Some objects were actually written for other people. Paul Rudy, a composer here in Kansas City was working on this piece for Bass Clarinet and MSP and was running into a nightmare of problems trying to manage several dynamic hierarchies of gain structure. So I looked at and thought it would be much simpler if there was an object to do x, and then build the patch around that. So I created tap.elixir~ to help with gain structure management. It has come in handy several times since…

## Lately, licensing has been a bit of a hot-spot in the Max/MSP/Jitter community. What is the license that the tap.tools works under, how did you choose this approach and how do you think this affects its use?

Well I’ve just moved Tap.Tools out of beta and up to 1.0, so I took the opportunity to re-examine the licensing. The licensing of Tap.Tools has been always been a perplexing situation for me. I truly desire for the objects to be accessible (meaning cheap and/or free), and to be educational (meaning they are open source and well commented), but at the same time I don’t really want others to go running off with my work and making a fortune on it without my benefit (or reimbursement, depending on how you look at it).

This combines with the fact that developing the Tap.Tools comes at a personal expense. My upgrade of CodeWarrior to keep the Tap.Tools up for OS X will be several hundred dollars – not to mention the time spent making help files (which I obviously don’t [need] for myself), responding to questions that folks have, and just supporting the package and paying for server space, etc.

Tap.Tools are now shareware. I am aware of over 200 people using them pretty regularly. I was hoping that maybe in the first week I could bring in enough to afford the needed Code Warrior upgrade, but only 4 people actually registered in the past two weeks (compared with 162 downloads of it). I figured that most people will take Shareware as freeware, but I still figured I’d have about 10% pay the nominal ($45) fee. Guess I was wrong…

I still think shareware is the way to go with it. Because it is now shareware I felt freer with the license to let people do anything they darn well please with it. They can make a million dollars with the Tap.Tools and that is just fine provided they gave me $45 of it for the license. Some people will be ticked, and morally opposed, etc. Oh well. That’s for their conscience to grapple with, not mine.

A tougher license to make reasonable was the one for Jade. Jade is also a version of shareware (I guess), but if you don’t pay for it there are significant restrictions built into the software – unlike Tap.Tools, which simply uses the honor system. It will be interesting to see the two methods side-by-side.

##

## Can you tell us more about your just-released project – Jade?

Jade is my solution to everything. Okay, so maybe that is a little over-the-top. But seriously, Jade basically bundles together solutions to my most common needs when creating a composition or installation. I can frame Jade by presenting the problems that I think it helps to resolve:

As I said earlier, Max/MSP is hard for a lot of musicians and really requires you to pay your dues before you start doing things with it. This is not helped by some of the idiosyncrasies of the software. I think Max/MSP excels like no other when it comes building instruments and effects processors. But when it comes to structuring a piece over time, a lot of folks just sort of sit and stare at the computer screen wondering what to do. How do I time my events? How do I automate events? How do I keep track of these hundreds of parameters when I want to change the order things happen in? It is a pretty complex issue.

There is also the issue of reusability of components. Object-oriented programming constantly tries to promise that if you do something once you can just call it and re-use it. But it isn’t that simple, especially in Max. You need to have enough structure that you know how do develop an object so that it can be reused over and over. Max gives you no constraints – but you need some, even if you develop them yourself. After my first couple of pieces/projects with Max I found myself frustrated at how long it would take me to try and take a piece of a previous project and incorporate it into a new project.

Then there is the issue of distribution. If you want to send a Max-built project to a performer (who doesn’t own Max) there are two possibilities: send the project with the Max Runtime or create a standalone app. A common problem though, even among experienced Max users, is that once on location for a performance or installation some of the variables or parameters need some adjustments. This can be really frustrating, especially if the patch wasn’t written to allow all of the variables to be controlled.

These are some of the issues that I’ve dealt with in creating the interactive component for my pieces. I’m obviously biased, but I think Jade deals with them admirably. I know people who haven’t ever used Max, but have spent a few weeks with Jade and created a piece music using modules that I have pre-packaged with the software. I think that speaks rather loudly, though Jade is the most powerful when used in conjunction with Max to build your own modules for the system.

Jade also deals with perennial Max/MSP issues like saving/loading presets, managing CPU easily and effectively, etc. All of my music runs in Jade now. Because of the structural framework it forces me to build my patches so that they will be re-usable in other projects, which is a big bonus in the long run. I could go on and on, but at some point you will probably fall asleep (if you haven’t already)…

## Not at all – can you give us a brief “walk-through” of making a simple Jade-based composition? Sometimes, products like this seem so complex that it is difficult to get a “vision” of using it.

Sure. I think what can initially overwhelm someone is that there is a lot to look at and a bunch of files, etc. It’s like when I introduce a class to Pro Tools or Digital Performer, every last ounce of the screen is filled with buttons and doodads that do something. What I think helps with Jade is understanding the paradigm. I like to use the paradigm of doing a gig with a bunch of hardware boxes.

If you are going to do gig with hardware boxes the first thing you do is select the gear you want. ‘I want a reverb, a delay unit, a couple of CD players, a compressor, and a mixer.’ Once you’ve selected the gear (and decided where you want to put it / how you want to stack it) then you have to wire it up. Finally, you will probably want to label the mixer so you remember what is plugged into where.

What I just described is a text file used by Jade called the Configure Script. Like a script used in a play, this script contains instructions on what pieces of gear (in Jade they are called modules) to use and how to hook it up and label it. If you tell Jade to create a new performance setup it will load a set of default scripts with, for example, a VST plugin.

Jade also has two other scripts. One sets all of the knobs and sliders to the correct position when Jade starts up, or when you manually tell it to Initialize. The other script is an event list which can be triggered by other processes in Jade, by shortcut keys, MIDI triggers, video analysis, etc.

If you deal with each chunk/module/unit on the screen one at a time I think the interface becomes a little easier to digest. Also the online help system can answer questions quickly. I tried to create lots of ways to get at information about modules quickly. That means bookmarks in the PDF documentation, getting an HTML page from any module’s menu, etc.

## Well, now that you’ve developed a solution for everything – what next on the horizon? Do you see yourself getting more involved with Jitter? And how much work will OS X (and potentially Windows) represent for you?

Hah! Now I’ve got someone else saying I’ve developed a solution for everything – next we take over the world! On a more serious note, I’ve got about 10 pages of lists of ideas for improvements and additions to Jade. Some of this is just making more modules to process audio. Some are ways to automagically generate the scripts.

One of those things is better support for Jitter. You can use Jade to do all sorts of stuff for video just like you can audio. But it isn’t very well documented, and there aren’t very many pre-built modules, or libraries to help with building the modules. So that is the next thing. In a way Jade will probably never be ‘finished’ – there will always be things I (or others) can add. It is stable, and the framework is in place so that I can write music more efficiently. And that was the initial purpose. As an example, the [technical side of an] installation I created for ICMC this past year (with Jesse Allison) took two weeks to create from scratch. For me that is lightning pace – and it is because of Jade.

Probably before I get to really clean up the Jitter side of Jade I will be working on the port to OS X. I love OS X. Besides all of the stability stuff people always site, I like that you can really get in and tweak your system and see all of the background processes, etc. The programming tools are all well documented, many are open-source, etc. I think it will be terrific to have Max/MSP/Jitter in OS X.

As far as Windows is concerned, I haven’t made up my mind. I used to be a hard-core Windows guy – hated the Mac, etc. Back in 1998 I bought Powerbook so that I could run Max, and I’ve totally flipped on the issue. I think back to the problems I have had, and that I have to help others with still, and it just doesn’t seem like Windows will be a great platform for interactive music. ‘Computers get stage-fright too,’ is something I like say. I guess I’m more comfortable with a Mac (especially OS X) that has stage-fright than a Windows machine that has stage fright. It seems like it would be nice to get Tap.Tools running for windows. Right now it would be hard to justify the expense though…

## Thanks for spending the time answering these questions. Here’s for some blatant self-promotion – GO!

Thanks for giving me the opportunity! I love your site, and read various columns frequently. I don’t have a lot of blatant self-promotion to do… Other than to say that I’ll have an interactive piece, Dandelions for Alto Sax and computer, on an upcoming [Centaur Release in the CDCM series](http://www.amazon.com/CDCM-Computer-Music-Vol-33/dp/B00009IC65) (expected release later this fall).

Thanks Darwin!

*Timothy Place’s various work can be found at [[http://blog.74objects.com](http://blog.74objects.com/) and [http://electrotap.com](http://electrotap.com/)], and is highly recommend by practically everyone that has used it.*