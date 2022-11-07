---
category : blog
tags : [MOTODEV, Android, Release]
title: MOTODEV Studio 3.1 now available
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

Happy New Year everyone!

I hope the holidays were good for everyone. I took some time away from
work and cleaned out 10 years of accumulated boxes from our garage. I
also ran Ethernet cable through the attic to my kids' rooms for their
computers and the Xbox. So, I earned points with everyone in the house.

I used my "bonus time" for several days to write an app for my *DROID
RAZR* . As odd as it may seem, I don't get to write a lot of code. The
chance to sit down and design again is actually a joy. I can't tell you
how excited I was to look up one of those nights and see that it was 2
a.m. It's been several years since I've done that. My app isn't ready to
unveil, but this has been a great experience as both a developer and as
a product manager.

Most of the time, when I demo MOTODEV Studio or the App Validator, it's
in short stretches for 10 or 15 minutes at a time. During the holiday
break, I used MOTODEV Studio 3.1 and App Validator for 3 days and I took
copious notes. I love hearing from users about how they love our tools,
but I also like hearing how we can do better. In the case of my own
development, I loved the wizards for creating activities and the string
editor. We're starting our next wave of development and my ideas as well
as yours are crucial to making a great experience for all Android
developers.

### Installers Available

As I mentioned in mid-December, we made MOTODEV Studio 3.1 available as
an online update. I'm now happy to announce that the installer packages
are available for *download at the MOTODEV site* . In my *previous post*
, I mentioned that this package contains the latest Google ADT plugins
(version 16.0.1). Some highlights of this new release are

-   The Localization Files Editor automatic translation feature now
    supports the Google Translate v2 API
-   Generated database classes now follow the Java naming standards
-   Automatic generation of code that saves and restores the app state
-   New code snippets, including some for the new Ice Cream Sandwich
    SDK, such as Finding Faces

![Screenshot.png](/assets/images/{{ page.id }}/original(1).jpg)
**Find Faces code snippet in MOTODEV Studio 3.1**

These things are in addition to many bug fixes that we've made in the
product--bug fixes we've made in open source components and updates to
open source components from Eclipse. Overall it's another stable release
and I didn't have any problems with it for my own development, plus the
hundreds of man hours of testing that went in before it was released.

### There's a New Kid in Town

For the past 18 months, we've talked about the MOTODEV App Validator and
it's been a vital part of MOTODEV Studio. With this release, we're now
providing another way to download and install the tool. App Validator is
now available as a separate download from the *tools download page*
(scroll down a bit).

Developers who want to test their apps but don't want to use MOTODEV
Studio are encouraged to download this free command-line tool.
Installation is a matter of simply extracting the archive wherever you
want the tool to run, preferably somewhere on your $PATH. The tool
starts from a batch file or shell script called 'appvalidator' and runs
on Windows, Mac, and Linux. Support is available on our *discussion
boards* , the same as for MOTODEV Studio.

### Farewell (features)

Every release means evaluating requirements and determining what makes
sense to support. This release was no different.

Two years ago, we started a project of translating our products to
Portuguese, Spanish, and Chinese. We went so far as to translate some of
our open-source pieces as well. It was a fun project, but it is a lot of
work to manage the changes of string resources between versions. While
it would be nice to have translators work using the same Agile methods
and tools that our developers do, the truth is they don't and probably
won't for many years. So, we've removed the translations from MOTODEV
Studio and the language packs from the download pages.

In our *Download Components* screen, we've removed the tool that allowed
users to download SDK addons (emulator images) via MOTODEV Studio. This
is because developers can download these files *via our web site* and
the Google SDK manager provides this function now. We've submitted our
link to Google and as soon as it's approved and activated, you will be
able to download Motorola addons directly from the SDK Manager.

Finally, as for host OS support, not a lot changed. We always evaluate
where we stand and I'm seeing a lot more users switching to 64-bit this
last quarter. Linux developers are picking up, but the thing to point
out is that we test on Ubuntu and Fedora using VirtualBox. If you have
your own favorite combination (and most Linux users do), please help us
by trying to eliminate as many variables as possible.

### To 2012 (and beyond)

Once again, I'd like to thank everyone for their support and feedback of
MOTODEV Studio and our tools in 2011. We look forward to working with
you again in 2012.

Eric
