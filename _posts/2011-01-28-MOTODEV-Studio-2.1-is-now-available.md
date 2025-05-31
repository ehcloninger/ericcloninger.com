---
category : blog
tags : [MOTODEV, Android, Release]
title: MOTODEV Studio 2.1 is now available
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello Everyone!

A new version of MOTODEV Studio for Android was released on 28 January
2011. This new release provides support for the Android 2.3
(Gingerbread) SDK and the Android 3.0 (Honeycomb) Preview SDK. MOTODEV
Studio 2.1 ships with ADT plugins version 9, which have been renumbered
by Google since the last major release.

A description of the release with a list of new features is available on
the *MOTODEV discussion boards* . If you are planning to develop apps
for the Honeycomb Preview SDK, please spend a few moments reading the
*known issues* in the *release notes* . The installers are available at
the *MOTODEV Studio downloads page* and you can update an existing
version of MOTODEV Studio using the "Check for Updates" from the Help
menu.

Thank you for using MOTODEV Studio for Android.

Eric Cloninger

Product Line Manager

------------------------------------------------------------------------

**&gt;&gt;&gt;&gt; Content of Discussion Board Post &lt;&lt;&lt;&lt;**

------------------------------------------------------------------------

Hello everyone!

I hope you're excited as we are about Wednesday's announcement of the
Honeycomb Preview SDK. This means you can now start preparing your apps
to run on the next generation of Android mobile devices, including the
Motorola XOOM™.

MOTODEV Studio is ready for you to download as well. We've been
preparing this release for several months to work with the Gingerbread
SDK and we were ready for the Honeycomb preview as well. The Honeycomb
preview works with MOTODEV Studio with some limitations, which are
outlined in the "known issues" section of the release notes. Keep in
mind that if you are working on projects that will be shipping in the
Android Market immediately, you should continue to work with the
Gingerbread SDK.

Now, on to some of the cool new features of MOTODEV Studio 2.1. (A full
list is available in the release notes)

-   Improvements to the Localization Files Editor to provide support for
    array items. You can now edit cells in this editor "live" and there
    are new menu bar items for adding and deleting items from the
    editor.
-   The Device Management View supports multiple selection so you can
    operate on more than one device at a time. This means you can start
    or stop multiple emulators and even deploy the same app to multiple
    handsets at the same time.
-   We've improved the way the Code Snippets window displays and
    provided the ability to disable the popup preview.
-   Support for obfuscating your application code. Google announced
    several months ago that the SDK had the ability to obfuscate code
    with ProGuard. MOTODEV Studio now has some nice features that add
    project support for this tool as well.
-   Improvements to the Database Management View that allow you to
    completely disable updates from devices. You can also delete tables
    directly from the editor by right-clicking.
-   There is a new "Run As" option that installs the app on the target
    device, but does nothing after that. This is useful for developers
    of preference panels and home screen widgets.
-   There is a question at startup about erasing passwords from previous
    installations. This is a fix for a bug that was brought to our
    attention on the MOTODEV Discussion Boards. If you want to set your
    own password for the key storage, you will want to allow this to
    happen.

**MOTODEV App Validator**

With the release of MOTODEV Studio 2.0 in October 2010, we added a new
feature called the MOTODEV App Validator. This feature performs a number
of static checks on your applications to help ensure that they are the
absolute best quality that they can be.

With MOTODEV Studio 2.1, we've updated the App Validator to test new
conditions, including some that may surprise you. To access this
feature, right click your project in the Package Manage and choose
"Validate Android Application". Or, you can validate existing Android
applications (.apk files) by choosing "App Validator" from the MOTODEV
Menu.

The new features of the App Validator are...

-   The missingDrawables checker tests for drawables for the xhdpi
    configuration used in the Motorola XOOM.
-   The ability to test against device specifications. This helps ensure
    that your application will work correctly on new Motorola devices,
    such as the CHARM™, CITRUS™, and ATRIX™.
-   Test your app against conditions that could prevent it from
    appearing in the Android Market for certain classes of devices, such
    as those with small screens (e.g. Motorola CHARM).
-   Output results in .CSV format for input into a spreadsheet.

We're making a lot of exciting new changes to MOTODEV Studio and the App
Validator in the coming months, so stay tuned to the MOTODEV Studio blog
for news.

Thank you for using MOTODEV Studio.

Eric
