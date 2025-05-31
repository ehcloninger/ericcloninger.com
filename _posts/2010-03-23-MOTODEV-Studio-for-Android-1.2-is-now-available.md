---
category : blog
tags : [MOTODEV, Android, Release, i18n]
title: MOTODEV Studio for Android 1.2 is now available
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello MOTODEV Studio users,

An new version of MOTODEV Studio for Android is available yesterday, 22
March 2010. Thisnew release provides support for the Android 2.1
(Eclair) SDK and includes version 0.9.6 of the Google ADT plugins. We've
added many new features for this release, including the ability to
install into your own Eclipse environment, 64-bit host support, Windows
7 support, as well as a large number of great improvements to existing
features. We're very happy with this new release and we hope you will be
as well.

A longer description of the update and how to install it is available on
the MOTODEV discussion boards at
*http://community.developer.motorola.com/t5/MOTODEV-Studio-for-Android/ANN-MOTODEV-Studio-1-2-is-now-...*

Thank you for using MOTODEV Studio for Android.

Eric Cloninger

Sr. Product Manager

------------------------------------------------------------------------

**&gt;&gt;&gt;&gt; Content of Discussion Board Post &lt;&lt;&lt;&lt;**

------------------------------------------------------------------------

Hello Everyone,

I'm attending the EclipseCon conference in Santa Clara this week and
this is also the week of CTIA in Las Vegas. There is a lot of excitement
around mobile applications and we're spending a lot of time at these
events interacting with mobile developers. At these events, we're
talking a lot about the developer ecosystem (that's you) and so it
pleases me greatly to announce that MOTODEV Studio 1.2 is now available
for download.

MOTODEV Studio for Android 1.2 supports the Android 2.1 SDK and includes
the ADT 0.9.6 plugins. Release notes, installation instructions, and the
installer are available on the MOTODEV download site.

There is a lot of cool stuff in MOTODEV Studio this time and you will
have to read through the install guide, release notes and online help to
get the full story, but here are some highlights:

Install MOTODEV Studio into your own Eclipse environment. You've been
asking for this and we've listened. You can point your existing Eclipse
installation at our update site and install the MOTODEV plugins directly
into your own version of Eclipse Galileo. There are some caveats, so you
will need to read the install guide carefully. As this is a new feature
and the number of permutations out there are endless, we're going to ask
that you work with us to resolve any problems and be patient. At the
moment, you must use the update site to install the plugins. We're
working to get the plugins into a downloadable archive, but we didn't
have time to properly test that for our release date. We'll make a
separate announcement when a downloadable archive is available.

-   64-bit installers. Another popular request from our users is now
    available. When you go to the download page for MOTODEV Studio, you
    will now be given a choice of 32-bit and 64-bit installers. 64-bit
    packages are also available on the update site, so make sure you
    choose the correct one for your system.
-   Develop your way. We've tested MOTODEV Studio against 32 and 64 bit
    versions of Windows XP, Windows 7, Mac OS (Leopard and Snow
    Leopard), Ubuntu 9.x, and Fedora 11. Other host combinations may
    work, but if one does not, your best option is to try one of the
    OSes that we've tested.
-   New emulator view options. We've added a much faster way to display
    the emulator view (on Windows and Linux) and cleaned up the
    confusion about how to use MOTODEV Studio with the external
    emulator. This new feature uses native graphic calls instead of
    relying on VNC running as a process in the emulator, although VNC is
    still available as an option if you run into problems.
-   Device Manager improvements. The Device Manager view now shows the
    target for all connected devices and AVDs. We've also cleaned up the
    UI a bit to get rid of the buttons that used a lot of real estate.
    Those buttons are now available as context menu items for each
    device in the view. Another useful new feature of the Device Manager
    view is that you can take screenshots of a device or a running
    emulator without having to bring up the DDMS perspective.
-   Support for Motorola Android handsets. MOTODEV Studio is tested with
    every new Motorola handset that goes into the market, so you know
    that you can test your application when new phones are available.
-   Localization editor. We've done some exciting work in the
    localization editor and we think it's going to make your
    localization task much simpler. We've added support for multiple
    line strings, styled text, and we now preserve embedded XML comments
    in the source file. But the most exciting thing is...
-   Automated translation of strings using Google Translate. With this
    feature, you can take an entire set of strings and choose to
    translate them to another language. In a matter of minutes, you can
    have your app running in a new language. How cool is that? But,
    please, exercise some restaint and caution when using this feature.
    The last thing we all want is for your great idea becoming the next
    "All your base are belong to us" joke because of a poor translation.
    Get a native speaker to approve your app before submitting it to the
    market.
-   The development team has worked hard the last few months to get all
    these great new features created and tested and the docs team has
    done a great job of putting the instructions into a clear and
    readable style. We hope that you use these tools and they make your
    development task a lot easier. I would love to be able to say that
    this release has zero problems, but there will always be things that
    don't work for people. If you find a problem, let us know on these
    discussion boards. We'll do our best to work with you to resolve
    problems.

There are some known issues with the release. We've documented them as
best we can, with workarounds where we've found them. If you read
nothing else (besides the install guide), please look at the known
issues before posting your bug reports. It could be that we've already
identified the problem and have a workaround.

If you have any problems with the update or other questions or comments
about MOTODEV Studio for Android, please post your feedback in the
MOTODEV Studio Discussion board.

Thank you for using MOTODEV Studio for Android.

Eric Cloninger

Product Line Manager

Eclipse Sequoyah Project Lead
