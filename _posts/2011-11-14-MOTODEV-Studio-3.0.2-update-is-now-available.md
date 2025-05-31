---
category : blog
tags : [MOTODEV, Android]
title: MOTODEV Studio 3.0.2 update is now available
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello Everyone!

This is just a quick post to let you know that we've released MOTODEV
Studio 3.0.2 as an update. This update is available by going to the
MOTODEV menu and selecting "Update MOTODEV Studio".

![](/assets/images{{ page.id }}/original(1).jpg)

This update brings in a several bug fixes to the App Validator that we
found as a result of testing with Android SDK version 15. It also brings
in the official ADT 15 plugins from Google. If you've already manually
integrated ADT 15 yourself, then you are fine with the tools you have,
unless you want the bug fixes to the App Validator.

If you are updating from MOTODEV Studio 3.0.0 or earlier, you will want
to simultaneously update your Android SDK to version 15. Because the
version of ADT that is included with this update is intended to work
with SDK version 15, you will start to get compile errors in your
projects if you update them out of step with each other.

You might've seen some of *my posts on our discussion boards* or links
to [articles at the Android developer
site](http://www.google.com/url?q=http%3A%2F%2Ftools.android.com%2Ftips%2Fnon-constant-fields)
on changes to the tools for Ice Cream Sandwich. If you've been using
R.constant values in your projects in switch statements, you will need
to spend some time refactoring those into if-else blocks before moving
to SDK 15.

Other than the R.constant issue, there should be no major changes to
MOTODEV Studio 3.0.0 users with this release. In fact, with the ADT 15
release, it appears that Google has fixed quite a few usability problems
that were reported in ADT 14.

Thank you for using MOTODEV Studio!

Eric
