---
category : blog
tags : [Samsung, Eclipse, Tizen]
title: Develop Samsung Gear and Android Apps with the Tizen IDE for Wearable
hidden: true
---

**NOTE:** This blog post was originally hosted on the Samsung Developers site. This content is no longer available on the original site. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Today, I'm going to show you how you can develop Samsung Gear and
Android Java apps using the Tizen IDE for Wearable.

Previous videos in this series have shown how to install the IDE and
Tizen SDK for Wearable. The Tizen IDE is a branded version of the
Eclipse IDE, so you can install the Google Android plugins for Eclipse
from the Google installation repository. This is a fairly
straight-forward exercise, but if you haven't done this before, it may
not be obvious, so I'll go through all the steps here.

I've recorded this as a video, but I also have this as a set of written
instructions. If you want the shorter version without all the
explanations and pictures, skip down the the TL;DR instructions below.

1.  First, let's go to the Android developers website. Locate the SDK
    link, probably at the bottom of the page and click it.
2.  Google bundles Eclipse with the ADT plugins, but you won't be using
    this version, so ignore their bundled edition of Eclipse + ADT.
3.  Locate the link the left sidebar named Revisions and click it. Then
    click on the link titled ADT Plugin.
4.  On this page will be a link titled Installing the Eclipse Plugin.
    You can also search Google for the phrase Installing the Eclipse
    Plugin and the first result should take you to this page. Scroll
    part ways down the page to a section titled Download the ADT plugin.
5.  In this section is an URL you need to copy. This URL is not for a
    browser, but to use in the Tizen IDE. As of September 2014, the URL
    is below. You can copy it from here, but it may change, so make note
    of the instructions in case you need to reference Google's original
    site.

        https://dl-ssl.google.com/android/eclipse/

6.  Start by going to the Help menu and choosing Install New Software.
7.  In the Add Repository dialog, give any name you like and then paste
    the URL that you copied from the web. After a few moments, the
    dialog will be filled with the contents of the online software
    repository that Google maintains for these plugins. There are six
    separate features in this repository. Check all of them for now,
    although you will probably into a problem that you will have to fix
    in a moment.

    If you encounter an installation problem, there is a solution. The
    problem is a conflct in version numbers with the Native Development
    Tools that allow you to develop C libraries for Android. Uncheck
    this box to continue installing the Android Development Tools.
    Chances are if you're writing C libraries, you're probably not
    building or debugging them with an IDE. At least not Eclipse.
    
    While it would be nice to have these plugins for completeness sake,
    most likely what you want to do is write and debug Java code. The 5
    other features shown here enable you to do just that. The cause of
    the problem is a conflict in version numbers of a common feature
    used by both Tizen and Android NDK. In the future, this may be
    fixed, so it's always worth trying to install them. If it works in a
    future version, then it's been resolved.

8.  You will have to accept a dialog stating that Google does not sign
    their JAR files. They never have. If this bothers you greatly, you
    can download a MD5 checksummed version of the plugins from their
    site and manually download them and install using these same
    instructions.
9.  Once the plugins are installed, you will need to restart the Tizen
    IDE to begin using the Android plugins.

**TL;DR**

These instructions are the short and sweet way to get working with
Android in any Eclipse IDE as fast as possible, but they aren't
guaranteed to work all the time. They work as of the time I wrote them,
but some of the details or links may change, so refer to the full
instructions if they stop working.

1.  Start the IDE. Go to Help-&gt;Install New Software.
2.  Press the Add button in the upper right corner.
3.  Give any name you like and use this URL for
    Location(https://dl-ssl.google.com/android/eclipse/). Check all
    features except Android Native Development Tools.
4.  Let the plugins download, approve that Google is installing unsigned
    JARs on your system, and restart the IDE.
5.  PROFIT!

### Wrap It Up; I'll Take It

At this point, you should be able to create both Samsung Gear projects
and Android Java apps from the same IDE. The Eclipse perspective
switcher in the upper right corner allows you to change your editing
settings between the Java view for Android and the Tizen Web view for
Samsung Gear projects.

This shows why so many developers like to use Eclipse. With one common
environment, they can develop for desktop, web and mobile at the same
time.

Thanks for stopping by today. I look forward to chatting with you about
tools and SDKs at the Samsung Developer Conference in November or at a
future developer conference. Let me know in the comments the things that
are important to you.

Eric (@ericc)

------------------------------------------------------------------------

### About Eric Cloninger

I have twenty six years of experience in desktop and mobile software
development. Fifteen years of those years were spent creating
development tools and managing teams of developers. The last eight years
have been devoted to product management of development tools and working
with open source developer communities.Specialties: Windows, Macintosh,
Linux, Android, Eclipse, development tools, user experience design,
product management, open source, technical writing, technical marketing,
event planning, C, C++, and Java.
