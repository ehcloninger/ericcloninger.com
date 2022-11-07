---
category : blog
tags : [MOTODEV, Android, Release]
title: MOTODEV Studio 1.3 is now available
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

Hello MOTODEV Studio users,

A new version of MOTODEV Studio for Android is available today, 28 July
2010. This new release provides support for the Android 2.2 (Froyo) SDK
and includes version 0.9.7 of the Google ADT plugins. For this release,
we've added support for building C and C++ projects using the Android
Native Development Kit (NDK), automatic discovery of SDK addons,
improvements to the database view, new templated activity wizards, and a
bunch of new code snippets for you to use in your own projects.

A longer description of the update and how to install it is available on
the MOTODEV discussion boards at
*http://community.developer.motorola.com/t5/MOTODEV-Studio-for-Android/ANN-MOTODEV-Studio-1-3-is-now-...*

Thank you for using MOTODEV Studio for Android.

Eric Cloninger

Sr. Product Manager

------------------------------------------------------------------------

**&gt;&gt;&gt;&gt; Content of Discussion Board Post &lt;&lt;&lt;&lt;**

------------------------------------------------------------------------

Hello Everyone,

Now that the MOTODEV Studio team is focused again on software
development (and not the FIFA World Cup), we are pleased to bring you
MOTODEV Studio for Android version 1.3. This new version supports the
Android 2.2 (Froyo) SDK and includes the ADT 0.9.7 plugins.

The release notes, installation guide, and installers are available on
the MOTODEV Studio download site. Highlights of version 1.3 are as
follows:

-   Build native apps using the Android Native Development Kit (NDK).
    This is a new feature and will probably change in subsequent
    releases, but we're excited to have it in this release. You will
    need to download, install, and configure the NDK manually before
    using this feature.
-   Use the database tool to open or create databases on your computer
    and inspect or modify them.
-   Generate database management classes for SQLite databases that are
    included in your projects. Create a database, include it as an asset
    in your project, get access to it using generated classes and share
    your data with other apps using the automated Content Provider
    wizard.
-   Analyze memory usage (search for memory leaks, reduce memory
    consumption) using MAT. Use the HPROF dump feature of DDMS to
    observe your apps' behavior and use MAT to collate and inspect the
    results.
-   Download and configure SDKs and platforms, SDK add-ons, samples,
    language packs, and NDKs all from a single dialog called "Download
    Components" (available on the MOTODEV menu).
-   Create and execute Monkey tests from the Device Manager view.
-   Create a variety of new activities based upon templates using the
    New Activity wizard such as a preferences dialog with a few clicks
    of the mouse or a scrolling list attached to a database.
-   Use code snippets for Bluetooth, SQL, Web and Web Services, Sensors,
    and General UI Utilities
-   Create widget projects and widget providers using new wizards
-   Connect to git code repositories with the eGit plugin

There are known issues with the release. We've documented them as best
we can, with workarounds where we've found them. If you read nothing
else (besides the install guide), please look at the known issues before
posting your bug reports. It could be that we've already identified the
problem and have a workaround.

One known issue that I'd like to highlight is for users of 64-bit
Windows 7. Late in the testing cycle, we discovered that there are
issues with the installer when you are running as a non-administrator.
Please read the known issues for the workarounds for this problem. We
have a solution that we're working on and will announce soon. In the
future, we are working to have installers that operate correctly on
64-bit Windows 7 systems.

If you have any problems with the release, please post in the MOTODEV
Studio Discussion board. We hope you enjoy what we've put together and
we're interested in getting your feedback on what you like and what
you'd like to see in a future release.

Thank you for using MOTODEV Studio for Android.

Eric

http://developer.motorola.com/docstools/library/MOTODEV_Studio_for_Android_Release_Notes/#Known_Issues
