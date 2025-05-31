---
category : blog
tags : [CodeWarrior, PalmOS]
title: From The Factory Floor October 1999
hidden: true
---

**NOTE:** This blog post was originally hosted on the [MacTech web
site](http://www.mactech.com/). I've made every attempt to preserve this
post with their original content. Many web links are no longer valid, so
they have been removed and replaced with *emphasized* text.

From The Factory Floor October 1999
-----------------------------------

    Volume Number: 15 (1999)
    Issue Number: 9
    Column Tag: From the Factory Floor

A Palm Update, Part 1
=====================

*By Eric Cloninger, Phillip Shoemaker, and Dave Mark, ©1999 by
Metrowerks, Inc., all rights reserved.*

A lot has changed since we last covered the world of Palm development.
Things are really heating up these days with new Palm hardware, new Palm
software, and a new release of CodeWarrior for Palm OS. This month's
interview is with Metrowerks' own Eric Cloninger and with Phillip
Shoemaker from Palm Computing.

Eric Cloninger is the technical lead, product manager, and utility
infielder for CodeWarrior for Palm OS. When he's not pounding the
keyboard, you can find him showing his son how to throw a slider or
lamenting the Rockies' pitching staff. 

Phillip Shoemaker is the Manager of Developer Tools at Palm Computing.
When he's not harassing his engineers, he can be found riding his
mountain board down rocky terrain. 

**Dave:** In the early days, all Palm development was done on a Mac.
What is the situation these days?

**Eric:** Palm developers can take advantage of the benefits that the
Macintosh provides. If you recall from our previous discussions (From
the Factory Floor, March 1998), Palm devices use the Motorola MC68328
and MC68EZ328 processors. These processors, nicknamed the DragonBall and
DragonBall EZ, are 68000 core processors with built-in serial and LCD
controllers. The device runs at 16Mhz and has between 512KB and 4MB of
RAM. If you look carefully, you'll recognize that a Palm device has more
computing power than the original Macs that we all bought in 1984!

**Phillip:** It has always made sense to develop for a 68k on a 68k and,
as Eric just mentioned, the Dragonball processor is a 68K. It's easier
for us to write our tools and OS where we can take advantage of the 68K
emulation that the MacOS provides-so our development tools have always
been first on the Macintosh and later on other platforms. A lot of our
developers have strong Mac backgrounds, some came from Apple, some from
Newton, some from Metrowerks, and the tools show their Mac heritage.

**Dave:** There's a Palm developer's conference coming up in October.
What can you tell me about the conference?

**Phillip:** This is the third developer's conference for the Palm
Computing Platform[TM], now named PalmSource 99[TM]. We have added
more tracks to the conference, which now spans four days. It's packed
with sessions targeted towards doing new business development, marketing
your applications, etc.

The majority of the conference is targeted towards doing serious
development on our platform. Some of the sessions we have planned
describe how to write Conduits to transfer data between your Mac or PC
and the Palm device. Other sessions will describe how to take advantage
of the wireless capabilities of the Palm VII, as well as how to write
localized applications for English, Japanese, and other languages.

There will be several labs for developing on the Palm OS and showing how
to use our tools. The labs will be open late, until 2:00 am, I think, so
Mac hackers should feel right at home.

**Dave:** Is Palm planning to release a new set of SDKs there as well?

**Phillip:** Yes we are. We expect to have a final version of all our
SDKs a few weeks before the conference. Metrowerks will incorporate our
SDKs into the CodeWarrior environment in time to bring to the
conference.

**Dave:** What can you tell me about the SDK?

**Phillip:** The latest SDK for the Palm Computing Platform supports a
variety of connected organizers, including the Palm III, Palm IIIx, Palm
IIIe, Palm V, and the Palm VII. This also includes devices from
Qualcomm, Franklin, IBM, and Symbol Technologies.

The SDK includes updates to our headers and libraries to support all
devices, as well as updates to the Mac OS-hosted simulator and the Palm
OS emulator (aka CoPilot). The SDK also includes sources to many sample
applications, including the organizer applications that are in the
devices' ROM. Additionally, the new SDK contains documentation on all
the OS calls and a detailed tutorial.

Using the updated SDK, developers can write applications that run on the
original Pilot 1000 devices, the Palm Pilot, the Palm III, Palm V, and
Palm VII. Developers can write applications that utilize the wireless
functionality in the Palm VII, as well as write applications for
Japanese-enabled devices.

**Dave:** Eric, what can you tell me about the new version of
CodeWarrior that will be released at the conference?

**Eric:** The latest CodeWarrior tools for the Palm Computing platform
will be based on CodeWarrior Pro 5 that was released in June. This means
that Palm developers have access to the new CodeWarrior browser, the new
XML import/export facilities, as well as updated compilers and
libraries. Palm has added features to Constructor and the PalmRez post
linker that allows users to create applications for Japanese versions of
the OS. Those users who already use CodeWarrior Professional will see
slightly newer versions of the IDE, MSL, and the compilers.

**Dave:** OK, you've convinced me. Let's say I go to PalmSource 99,
attend the sessions and get the tools. Where do I start? What do I do?
What kind of resources are available to me?

**Eric:** Well, you can start right now, before you go to the
conference. CodeWarrior for Palm OS Release 5 is available from Palm or
Metrowerks. The samples and tutorial that Phil mentioned earlier are
available for the 3.0 version of the OS (Release 6 will have versions
3.1 and 3.2). There's a lot to learn about the core OS that will take
most developers a while to learn.

**Phil:** We can also walk through some detailed code on the OS in next
month's MacTech. In the mean time, here are some web sites that
interested developers can start looking at:

-   *http://www.palm.com* - Palms' main web site.
-   *http://www.palm.com/devzone* - Palms' developer zone
-   *http://www.palmsource.com* - Information on the Palm Computing
    Platform developers conference
-   *http://ls.palm.com* - List server run by Palms' developer group.
    Users can sign up to receive email forums on the OS, conduits, and
    emulator.
-   *http://www.metrowerks.com/pda/palm/x* - Metrowerks' main site for
    the Palm product. Metrowerks is planning to add a tools support site
    located off this site where users can download tools and documents
    that aren't on the CD, such as the HackMaster sample, Eric's PRC
    viewer, the CodeWarrior FAQ, etc. We don't have a definitive
    location for this yet.
-   *http://news.massena.com* - Darrin Massena's private news server is
    a high signal-to-noise ratio news server where many experienced
    developers hang out.
-   *http://www.roadcoders.com* - A good place to get tools, SDKs,
    samples, and utilities.
