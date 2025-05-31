---
category : blog
tags : [MOTODEV, Android, Release]
title: MOTODEV Studio 3.0
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello everyone!

It's my pleasure to announce that MOTODEV Studio 3.0 is now available
for download. This update brings in Eclipse 3.7 (aka "Indigo") and the
"tools_r12" version of the ADT plugins.

MOTODEV Studio 3.0 supports Android SDK version 12. We initially planned
to release Studio 3.0 after the Android SDK tools_r13 package was
released. The recent kernel.org hack attack has had a *ripple effect on
projects* stored there. We anticipated that tools_r13 would be released
by now, but it hasn't due to those attacks. We've tested Studio with
both the current r12 tools and the tools_r13 branch on kernel.org.
**For now, work with the current r12 tools with confidence.** We will
continue to test with tools_r13 on kernel.org. When those tools are
released we will ensure that Studio works properly.

We've made quite a few changes and have some exciting new features in
MOTODEV Studio 3.0. There are also some changes to the way some Studio
features work and we will go through those in this posting. If you need
more detail on what's changed, a great source of information is the
*MOTODEV Studio release notes* and the *Installation Guide* .

### Updates

Over the last year, we've worked to improve our update mechanisms. If
you want to update MOTODEV Studio 2.x to 3.0, you need only go to the
MOTODEV menu and choose "Update MOTODEV Studio". This operation uses the
Eclipse mechanism to connect to the MOTODEV update server. Instead of
looking all over the Internet for different update packages, the MOTODEV
site has all the packages you need in one place.

![image03.png](/assets/images/{{ page.id }}/original(1).jpg)

If you want to make a clean start with Studio 3.0, you can download the
full installer package. If you are a "power user" and already use
Eclipse for desktop or server development, you can install MOTODEV
Studio as plugins into your existing Eclipse Indigo environment. Both of
these options are available from our *downloads page* .

Regardless of which way you install, you will get the latest Eclipse
platform release, the most recent Android ADT plugins, and the many
features that MOTODEV Studio provides.

### Themes

With every release, the Studio development team and I look for areas to
improve. Rather than adding features haphazardly, we like to focus on
what we consider "pain points" and see how we can make developers' lives
easier.

### Plugin maintenance

One of the themes of every release is updating to the latest Eclipse
platform and Android plugins. For Eclipse, it's a simple matter as the
release schedules are published well in advance. The Android plugins
aren't on such a schedule, so there are times when we wake up one
morning and notice there are new plugins available. Usually we see this
coming as the Android tools team works in the open and their repo is
*visible at kernel.org* .

On a similar vein, we've previously announced that we are deprecating
our support for 32-bit MacOS systems. This policy takes effect as of
this release. Users of 32-bit Macs should continue to use MOTODEV Studio
2.2.2 with the r12 Android SDK for as long as they can. While we haven't
done anything in the plugin archive to prevent users from installing the
plugins on 32-bit Macs, we will no longer be testing these
configurations or answering support issues involving them.

### Feature Discovery

A major theme of the current release was to make things easier to find.
We've added a lot of functionality the last 2 1/2 years and sometimes
people are surprised by the appearance of a feature. So, we've
re-ordered our menus, cleaned up the toolbar, and added more context
menu shortcuts.

The most noticeable change in this "discovery" theme is the addition of
a new "Videos View". In addition to creating new features, we've added
the ability to view YouTube videos from our playlists within the IDE.
We're planning to add a series of videos on using the tools and
developing for Android.

![image01.png](/assets/images/{{ page.id }}/original(2).jpg)

### No Boring Code

In earlier releases we added the "Code Snippets" view and the ability to
generate adapters based on the fields of a database. Now comes a big
feature that I know is going to make lot of developers happy. In Studio
3.0, you can now generate a lot of Activity code based on a designed
layout. Not only that, you can modify the layouts and re-generate the
code without destroying what has already been done. So now, instead of
writing all those adapters to toggle a variable when a checkbox is
pressed, just design your layout, generate the code, and enter your
business logic.

![image00.png](/assets/images/{{ page.id }}/original(3).jpg)

We are working on a video to show this feature in action. In the mean
time, give it a try with a blank project and see how it works for you.
We're planning to add more functionality to this feature during the next
release cycle, so any suggestions on ways to improve its' behavior are
appreciated.

### Emulator Improvements

We wanted to improve the workflows around running and debugging
applications, so we've added several features here. The first one
improves your ability to install applications: just drag and drop your
.apk files onto an item in the Device Management view to start the
install process. You can drag from the Eclipse Project Explorer view or
from your computers' desktop manager.

How many times have you gone to launch your app and end up starting an
emulator by mistake? Even though you have an emulator running, the
launch configuration may be pointing to a different AVD. MOTODEV Studio
now checks when you're launching an Android app to see if there are any
compatible AVDs already running. If there are, it asks whether it should
use a running instance rather than starting a new one. I'm guessing,
based on our experience, this is what developers want about 90% of the
time.

### App Validator

The App Validator is a part of MOTODEV Studio, but it's also its own
tool. We integrate the tool into the IDE and ship it as part of the
product. For this release, we've added new checkers that will notify you
if you are requesting too many permissions, are missing declarations for
large screens, and if you are repeating resource IDs across different
files.

In App Validator 0.8, we added the ability to decompile the unobfuscated
Java code in your .apk files. We used this information to determine if
you were missing permissions and several other features. Now, we're
using this information to look for possible programming mistakes. App
Validator 0.9 scans code looking for API calls that cause database
cursors to open. It then looks for subsequent calls to close the cursor.
If none are found (or it appears that the number of closes is less than
the number of opens), you will receive a warning. This is an early
feature, so treat the results with some skepticism.

### Grab Bag

Themes are a nice tool for project planning, but there are always some
features that defy having a specific theme. That doesn't mean they don't
have value and shouldn't be included, but these things tend to have less
of an impact.

For example, we've improved the preference page for the App Validator.
In previous releases, users were forced to know the command line
switches for a particular function. Now the App Validator has a nice GUI
preference page.

![image02.png](/assets/images/{{ page.id }}/original(4).jpg)

Developers who use the database feature a lot should try out the updated
database creation wizard. Now, you can create databases and tables from
the same dialog and bind them to your Android app using the code
generation wizard. If you use databases in your app, you will find this
to be a huge time-saver.

### The Long and Winding Road

The road ahead looks to be interesting. Ice Cream Sandwich will be
making an appearance soon, and the work of the GoogleTV team is
available for the adventurous. The MOTODEV Studio team is starting to
plan our next release and our activities for 2012. While we don't know
what the future holds for all things Android, we can at least put down a
bit of a roadmap.

To start, we've contributed a new feature to *Android SDK tools* . This
change will allow developers to discover Motorola SDK add-ons (emulator
images) from the SDK and AVD Manager. This has been a feature of MOTODEV
Studio, but once the ability for the SDK and AVD Manager to work with
the MOTODEV site is available in the SDK tools, we will start phasing it
out of MOTODEV Studio.

There's also some new NDK (Native Development Kit) changes in store for
Eclipse users. Doug Schaefer from Wind River contributed code to the
AOSP for handling C/C++ projects in a nice way. Doug previously
contributed code for C++ support to the Eclipse Sequoyah project and
he's improved it significantly for the AOSP.

We hope that both of these features will be in the "tools_r14" branch
that could be out in early October. We usually don't know about these
things until we see the stabilization branch occur in the git repo. We
know we will have to update MOTODEV Studio to version 3.0.1 to work with
the new plug-ins, so expect to see an update release shortly after the
"r14" plug-ins and SDK are released.

We're beginning to start some investigation into features for 3.1. As
always, if you have an idea for a workflow improvement or even something
bigger, just drop us a note on the *MOTODEV Studio discussion boards* .
We're always on the lookout for new ideas that will make your lives
easier.

Thank you for using MOTODEV Studio!

Eric
