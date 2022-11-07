---
category : blog
tags : [MOTODEV, Android]
title: MOTODEV Studio 4.0 and the ADT 20 plugins
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

Hello everyone,

Two weeks ago at Google I/O, we saw the release of a new Android version
and new tools for the SDK. These tools had been anticipated for some
time and I wanted to update you on where we stand with MOTODEV Studio.

When we announced MOTODEV Studio 4.0 in May, I pointed out *in this blog
post* that we were splitting the concept of the MOTODEV tools into 2
pieces. The standalone product with the installer was tightly integrated
with the ADT plugins and requires work on our part whenever ADT is
updated. At the same time, we also announced a new "core plugins"
feature that can be installed into Eclipse that has no dependencies on
ADT whatsoever. While the look of the two versions is different, most of
the *functionality of the full product exists in the core plugins* . The
full product is meant to be stable, but would only be updated on our
quarterly schedule. When new versions of ADT come out, they may or may
not work with the existing full MOTODEV Studio but they should work with
Eclipse and the MOTODEV core plugins.

For this reason, we locked MOTODEV Studio 4.0 to ADT 18 in order to
prevent updates that would result in a breakage due to API changes
beyond our control. Unfortunately, the SDK Manager allowed developers to
update their SDK tools and SDK 20 has problems with ADT 18. We're
working on a new version of MOTODEV Studio that has ADT 20 (or 20.0.1)
already built-in that works with SDK tools 20. I anticipate you will
have access to it in a few weeks. Until that time, **you should not
update the SDK tools or attempt to manually install ADT 20 into MOTODEV
Studio**.

If you are using the MOTODEV core plugins with Eclipse, you can use ADT
20 and SDK Tools 20.

### Roll back SDK tools

If you've already updated to SDK Tools 20 and you're using the installed
MOTODEV Studio, there's a good chance that things will not work
correctly. Especially when debugging with the emulator. If you want to
continue using the MOTODEV Studio tools to develop for Ice Cream
Sandwich and earlier, you can download the older SDK tools version 19 at
http://dl-ssl.google.com/android/repository/tools_r19-windows.zip, http://dl-ssl.google.com/android/repository/tools_r19-macosx.zip, and http://dl-ssl.google.com/android/repository/tools_r19-linux.zip.

Using the links above as a template, you can download older versions of
the SDK by providing a different release number. The same formula works
for the ADT plugins, which can be found at
http://dl.google.com/android/ADT-18.0.0.zip.

### Using Eclipse

For developers who need to target Jelly Bean before MOTODEV Studio 4.1
is released, the best solution is to use a clean installation of
Eclipse, such as the new *Eclipse for Mobile Developers* package and update your
tools to the latest release (version 20 at this time). Unzip the Eclipse
package into a convenient location on your development machine.

![](/assets/images{{ page.id }}/original(1).jpg)

Once you have Eclipse downloaded and installed, launch the IDE and use
the Marketplace client (Help&gt;Eclipse Marketplace) to locate either
the Android Development Tools or the MOTODEV Core Plugins. You can also install from the web link by dragging the button labeled "Install" onto your running version of Eclipse. The MOTODEV core plugins
from the Eclipse Marketplace are linked to depend on ADT, so if you want
to install both, just use the MOTODEV link.

![](/assets/images{{ page.id }}/original(2).jpg)

If you're more comfortable with the Eclipse Update Manager rather than
the Marketplace Client, you can install the MOTODEV core plugins from
our site as well. Use Help&gt;Install New Software and create a new
update site whose link is
*https://studio-android.motodevupdate.com/android/4.0/basic* .

If you find any problems using MOTODEV Studio with ADT 18 or the core
plugins with ADT 20, please let us know on the *discussion boards* .

Thank you for using MOTODEV Studio

Eric
