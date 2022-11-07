---
category : blog
tags : [MOTODEV, Android, Android-SDK]
title: Notice about Android SDK 2.3
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

Hello Everyone,

Today, Google released SDK 2.3 which provides support for Gingerbread.
Information about support for this SDK is described on the MOTODEV
Studio discussion boards. Stay tuned to the MOTODEV Studio Blog RSS feed
for more details.

*http://community.developer.motorola.com/t5/MOTODEV-Studio-for-Android/Notice-about-Android-SDK-2-3/t...*

Thank you for using MOTODEV Studio.

Eric

------------------------------------------------------------------------

**&gt;&gt;&gt;&gt; Content of Discussion Board Post &lt;&lt;&lt;&lt;**

------------------------------------------------------------------------

Today, Google released a new SDK for Android 2.3 (aka "Gingerbread").
Like most of you, we've been waiting for this release because it means
exciting new APIs and features for all of us to use. Like many of the
previous SDK releases, 2.3 has a new ADT plugins. We've been seeing
checkins to the open source repository and have been testing MOTODEV
Studio against that code.

The new SDK has a change to the layout of the platforms directory so
that some of the tools that used to be in the 'tools' directory are now
in level-specific folders within the platform folder. For developers,
this means that plugins and build tools prior to this release are not
compatible with this release and vice-versa. You MUST update the tools
and the SDK at the same time or your build processes will fail. That is
the case whether you are using MOTODEV Studio, Eclipse, or a command
line.

We've been building and testing MOTODEV Studio for several weeks with
the code that was checked into the AOSP repos, but we want to make
certain everything still works with the released SDK. I anticipate that
we will have an update site that has the new plugins and works with the
2.3 SDK early next week. Check back on the discussion boards often or
just check the RSS view inside MOTODEV Studio for updates via the
MOTODEV Studio Blog.

If you want to play with the features of the SDK, you can download it to
a separate location of your drive. If you want to use it in the IDE, you
can download Eclipse and manually install the ADT plugins and the SDK
until we activate our update site.

Thank you for using MOTODEV Studio and best wishes for success in 2011.
