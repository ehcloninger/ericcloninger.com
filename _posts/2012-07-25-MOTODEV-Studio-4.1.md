---
category : blog
tags : [MOTODEV, Android, Release]
title: MOTODEV Studio 4.1
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

Hello Everyone!

One of the drawbacks of being a manager is I long for the days when I
wasn't a manager. This was when things were simple and I just had to
worry about the code I wrote. At least that's the way I remember life
before Product Management. The MOTODEV tools team won't let me submit
code into production, so I have to keep my code skills up to date on my
own.

I started writing an Android app late last year and I've been improving
it every now and then. I started the project because I wanted to better
understand how developers use our tools and what the pain points are.
What came out of that experiment is a new laundry list of requirements
for the dev team. I think they hope I give up development and stick to
comedy in the future.

I'm here today to announce that the latest release of MOTODEV Studio is
now available from the *download site* . I'm hoping that some of the
requirements I came up with will make your jobs easier.

### One Step Forward, One Step Back

I'm going to back up a step before going into the new features of
MOTODEV Studio. I want to remind everyone that there are now 2 distinct
ways to install MOTODEV Studio. How you work with the ADT plugins and
the Android SDK will govern how you install the product. I'm going to
paraphrase from my *earlier MOTODEV Studio 4.0 announcement* ...

*Do you always want the latest plugins and SDK tools the moment the
Android tools team releases them?*

If the answer to that question is "No", then you should continue to
install MOTODEV Studio the way you always have. Just download the
install package from our web site or the Eclipse plugin archive and work
the way you always have. Or choose "Update MOTODEV Studio" from the
MOTODEV menu. The installer version of MOTODEV Studio is locked to a
specific version of ADT (and by extension the Android SDK). When a new
version of the plugins or SDK are released, we won't support it until
the next release of MOTODEV Studio.

If you answered "Yes" then you will need to install the "MOTODEV Core
Plugins". This package has no dependencies on ADT, yet it delivers much
of the functionality of the full product. Using this package, you can
update Eclipse and/or ADT independent of the MOTODEV plugins and
everything should continue to work. You will notice that the locations
of things will have moved to accommodate working inside the Java
Perspective, but *most of the functionality* is still there. There is no
installer for the core plugins--you download and install Eclipse
separately and then install the MOTODEV plugins using ***Help&gt;Eclipse
Marketplace***. For developers who are interested primarily in
developing for Android, the new [Eclipse for Mobile Developers
package](http://www.eclipse.org/downloads/) is configured to work with
ADT and the MOTODEV core plugins.

### Keys to the Kingdom

One of the things I felt was confusing when I was writing my app this
spring was the way apps are signed before going into the Google Play
store. The Android development process allows developers to create their
keys and sign apps with those keys. This was a big change from the
previous world we were used to with Java ME where a manufacturer or
carrier would bless an app with a certificate from their central
authority. We had some signing capabilities in MOTODEV Studio that we
carried over from the Java ME days, but they really weren't in tune with
what Android developers needed.

We tore out the old signing view and created something new that works
with multiple keystore formats and gives you a lot of options for
managing keys and signing apps. This view and the way it works is
compatible with the jarsigner process that is used by the ADT export
wizard. In most cases, it is calling the same tools underneath, but we
use context and serialization to make many of the workflows simpler.

![](/assets/images{{ page.id }}/original(1).jpg)

After you've installed MOTODEV Studio 4.1, this view should be visible
along the bottom of the workspace with the problems view, device
manager, etc. Eclipse has difficulty dealing with new or changed views
in existing workspaces, so if you do not see the view, you can manually
add it with **Window&gt;Show View&gt;Other**. Another way to make it
appear is to reset the perspective (if you use the MOTODEV Studio
perspective) with **Window&gt;Reset Perspective**.

This view provides the ability to back up your keystores and gives you a
subtle reminder of the last time you backed up. I'll bet many of you
don't do this often enough. If you were to lose your keystore, you
wouldn't be able to update your app on the Google Play store. Even more
problematic is you would have to use a different signature in the new
app's manifest. Would your users be happy if they had to install a
completely new version of the app, just because you changed the signing
key? What if you had a paid app or one that dozens of other apps relied
on? Even if you don't use this feature, *please back up your keystore
today* .

The Signing and Keys view is available in the full product of MOTODEV
Studio, but it is not in the core plugins because it requires
information from ADT.

### Ring in the New

Every year, the Eclipse Foundation creates a [coordinated
release](http://www.eclipse.org/downloads/) in late June. Last year, the
code name for the release was called "Indigo" and this year it is
"Juno". In years past, the MOTODEV tools team has participated in the
train by including the Sequoyah open source components. This year, we
opted out, but we haven't been quiet. Myself and several of the MOTODEV
tools team created a new "[Eclipse for Mobile
Developers](http://www.eclipse.org/downloads/packages/eclipse-mobile-developers/junor)" package that anyone can use to create their mobile IDE. In fact, we used this package as the basis for MOTODEV Studio 4.1.

Google released new ADT plugins at the Google IO conference in late
June. These plugins support the latest SDK tools and platforms for Jelly
Bean and earlier. There were a few bugs reported in the plugins, so
Google has released an update to version 20.0.1. MOTODEV Studio 4.1
contains these updated plugins.

**NOTE:** As we were pushing this release out, Google made another
update of the ADT plugins to fix a critical bug. Our testing shows that
this release works with MOTODEV Studio 4.1. If you need to update to get
this patch, you are encouraged to do so using ***MOTODEV&gt;Update
MOTODEV Studio***.

### Ring out the Old

As a product manager or developer, it's important to look at your
product with a critical eye and see if it's still relevant to your
customers. This is true, no matter what the product and whether it's
free or costs money. Based on the download reports I get, more people
are downloading MOTODEV Studio than ever before. This leads me to hope
that we're doing the right things, but without a critical eye, we might
get complacent.

There have been quite a few features that MOTODEV Studio had in earlier
times. For example the ability to download the SDK and platforms from
the IDE. At some point, the Google plugins gained this ability, so our
feature was no longer necessary. In a similar vein, the ability to
download SDK addons for the emulator has been in our tools for a long
time, but now the Android SDK Manager provides the same function. We've
spent some time this release removing our overlapping features. Several
of these features were grouped under the Download Components window,
which is now gone from the MOTODEV menu.

One feature that overlaps in a way, but isn't going away, is our App
Validator. This static analysis tool does overlap in its function with
the Android '*lint*' tool, but lint does not do everything that App
Validator does. At least not yet. We don't want to compete with our
friends at Google on tools, especially since all our tools are free, but
we also see that this tool still has some usefulness. Use both tools to
improve your apps and when the time comes, we will look to put App
Validator out to pasture if it makes sense to do so.

### The Fork in the Road

Where do we go from here? Now that the Google acquisition of Motorola is
complete, we hope to have some intense discussions about tools. There's
nothing to announce at this time because we're all still digging out
from Google IO, summer vacations, and things like that. When we have
some news for you on that front, I'll let you know.

Until then, keep developing great apps. If you have problems with the
tools or getting them installed, let us know on our *discussion boards*.

Thank you for using MOTODEV Studio

Eric
