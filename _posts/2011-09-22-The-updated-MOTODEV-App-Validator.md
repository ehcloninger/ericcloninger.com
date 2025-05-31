---
category : blog
tags : [MOTODEV, Android, Static-Analysis]
title: The updated MOTODEV App Validator
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello Everyone,

I hope you've had a chance to download and try out MOTODEV Studio 3.0.
We've made many improvements to the tools in this release, including the
App Validator tool that is integrated into the IDE. Today, I'm pleased
to announce that we've made those changes available to the *online
version of the App Validator* as well.

![Screenshot - 9_22_2011 , 11_14_46
AM.png](/assets/images/{{ page.id }}/original(1).jpg)

If you aren't aware of this feature of the MOTODEV web site, you should
be. The MOTODEV App Validator is a static analysis tool that inspects
your Android applications (.apk files) and reports on many different
potential problems. Even if you don't use MOTODEV Studio, you should be
taking advantage of this tool.

Here are just a few of the conditions the App Validator can detect:

-   Invalid signing certificate dates
-   Not enough permissions in the manifest for the APIs being called
-   Missing values in translated strings.xml files
-   Missing drawables for different densities that can lead to Force
    Close runtime errors
-   Missing or incorrect layouts for the various screen sizes
-   Failure to properly close database "cursors"
-   Compatibility issues for specific Motorola devices

A full list can be found in *this online document* .

Start by browsing to
*http://developer.motorola.com/testing/app-validator/?* . You will see a
screen similar to the one shown below. We've tested this feature with
many browsers on Windows, Mac, and Linux. Firefox, Chrome, Safari, and
Opera support the drag and drop feature in their most recent releases.
Internet Explorer, unfortunately, does not. IE users will need to press
the "Browse" button on the screen and locate the file with an Open File
dialog.?

![Screenshot - 9_22_2011 , 11_14_45
AM.png](/assets/images/{{ page.id }}/original(2).jpg)

The new online version now gives you the ability to configure settings
in a way that is similar to the version in MOTODEV Studio. Many of the
command line switches that make the stand-alone tool so powerful are now
available in the online tool. By clicking "edit" (next to "Settings"),
you are presented with the following screen that allows you to configure
the settings that best suit your needs.

![Screenshot - 9_22_2011 , 11_14_57
AM.png](/assets/images/{{ page.id }}/original(3).jpg)

From this screen, you can choose which checkers you wish to run. For
example, if you don't want to see messages about missing strings, you
can clear the checkbox for "Localization Strings Checker". If you only
want to test the permissions for your app, clear all but the
"Permissions Checker".

In a similar manner, you can choose which devices your app is compared
against. For these tests, the validator isn't executed on the devices,
but rather the values found in the manifest and layouts are compared
against values for specific Motorola devices or classes of devices, such
as those with Extra Large Screen Sizes.

When you're ready to perform the test, you can click "Close" to save the
settings for the current session. If you choose "Make My Defaults", the
choices are saved as a cookie on your computer. The "Restore Factory
Defaults" button returns every checkbox to their default state.

When you are ready to test, just drag and drop your .apk files onto the
web form. After a few seconds, you will see the results appear, similar
to those shown below.

![Screenshot - 9_22_2011 , 11_50_37
AM.png](/assets/images/{{ page.id }}/original(4).jpg)

The results show up, as before, in a list at the bottom of the page. We
limit the size of the output to 10,000 results to keep bandwidth down.
If you want to clear the results of the process and test new apps, the
"Clear Results" button will delete all the errors and allow you to
continue testing files without reloading the page.

We've tried to make the online App Validator as useful as possible for
developers while making it easy to understand and use. We hope you find
this a benefit of your MOTODEV membership and use the results you find
to make positive changes to your own offerings.

We have more changes in the works for both the stand-alone and online
App Validator. If you are attending the *O'Reilly Android Open
conference* in October, stop by Ballroom B on Tuesday at 2:00pm for more
insight into this powerful tool.

Thank you for being a MOTODEV member!

Eric
