---
category : blog
tags : [MOTODEV, Android, Android-SDK]
title: MOTODEV Studio for Android 1.2.1 Update available
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello MOTODEV Studio users,

A new version of MOTODEV Studio for Android became available on Friday,
May 21, 2010. This update provides support for Android 2.2 (Froyo) SDK
and includes version 0.9.7 of the Google ADT plugins.

A longer description of the update and how to install it is available on
the MOTODEV discussion boards at
*http://community.developer.motorola.com/t5/MOTODEV-Studio-for-Android/ANN-MOTODEV-Studio-Update-1-2-...*

Thank you for using MOTODEV Studio for Android.

Eric Cloninger

Sr. Product Manager

------------------------------------------------------------------------

**&gt;&gt;&gt;&gt; Content of Discussion Board Post &lt;&lt;&lt;&lt;**

------------------------------------------------------------------------

The MOTODEV group had a great time at Google I/O last week and it was
fun meeting many of you at our booth, during the sessions, and at the
after-hours MOTOBOWL event. We hope that those of you who attended also
had a great time.

On Thursday, after the Android announcements during the keynote session,
Google released the 2.2 (Froyo) SDK. In order to use this SDK with
Eclipse or MOTODEV Studio, you will need a newer version of the Android
Development Tools plugins, version 0.9.7. MOTODEV Studio Update 1.2.1
has these plugins as well as a few new features and a number of bug
fixes that we've put in place since version 1.2 was released in March.

The install guide, release notes and online help have the full story,
but here are some highlights:

-   Supports Android Tools r6 (i.e. Froyo and earlier)
-   The MOTODEV Studio plugins are available as an archive package, so
    you can install into an existing Eclipse installation without having
    access to the Internet
-   The Startup Options property dialog now helps you calculate screen
    resolution and scale values based upon the screen dimensions of your
    monitor and the device being emulated
-   The Device Management view now displays the Android target for
    emulated devices, not just for connected ones
-   The Device Properties dialog now has a CSV Export option that lets
    you save handset properties
-   You can now refresh local databases to reflect changes in remote
    data without having to disconnect and reconnect

**Some things to remember**

There are some known issues with the release.

If you received a USB flash drive from us at Google I/O or MOTOBOWL, you
should not use the 1.2.1 update on the flash drive. We created that
image in the anticipation of Google's SDK release but without the
details of how it would be distributed. The Froyo SDK now has the
samples as a separate installation, which required our engineering team
to make a slight change to the workings of the New Project wizard.
Please update from the online site or use the separate 1.2.1 archive
file from the download page.

The new SDK release from Google actually updates all the previous
platform releases for 1.5, 1.6, and 2.1. If you are taking advantage of
features specific to these releases, you will also need to download
those platforms using the SDK updater from Google.

In order to get this update out quickly (only 1 day after Google), the
engineering and test teams had to make some practical decisions. The
product was tested thoroughly using the code that was checked into
kernel.org several weeks ago. However, there may be situations in the
SDK itself where problems may arise. When the SDK was released, we
restarted our testing, but primarily on Windows. We knew you would be
anxious to get started on your own development and deliberately bypassed
the full test regimen. So, if you find problems, let us know with a post
in the MOTODEV Studio Discussion Boards and we'll fix them in the
shortest time frame that we can manage or at least try to find a
suitable workaround.

Thank you for using MOTODEV Studio for Android.
