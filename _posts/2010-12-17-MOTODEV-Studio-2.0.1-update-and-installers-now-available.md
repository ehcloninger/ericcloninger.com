---
category : blog
tags : [MOTODEV, Android]
title: MOTODEV Studio 2.0.1 update and installers now available
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

Hello everyone,

We hope 2010 has been a very good year for you. 2011 certainly looks to
be very interesting for Android developers and we're excited to be
bringing you new tools.

Google released the Gingerbread SDK on Dec. 6 along with a new set of
Eclipse plugins (ADT). We've been working to release a MOTODEV Studio
update that will work with that combination. The testing took longer
than we thought because ADT must be updated at the same time as the SDK.
In previous releases, some combinations of ADT and SDK would work, but
not this time. You must use ADT 8.0 (or later) with the Gingerbread SDK.
That is the only allowed combination.

Normally, we would dedicate a full release cycle to getting this much
functionality out to users, but we know that everyone is excited to
start developing with Gingerbread. So, please read this post carefully
and let it help guide you to the new tools.

### Irrational Numbers

There are several version numbers that you need to be aware of.

**SDK Tools:** These are the compiler, post-linker, and debug support
tools that get released at [The Android Developer
site.](http://developer.android.com/sdk/index.html) These are usually
labeled r&lt;***X***, where ***X*** is an integer. The tools are often
released at the same time as a platform SDK, but not always. The current
release of tools, released with Gingerbread, is ***r8***.

**ADT:** Previously, the Google ADT plugins for Eclipse had version
numbers such as 0.94, 0.98, etc. With this release, Google is versioning
the plugins the same as the tools release. So the new versions of ADT
will have numbers of 8.0, 8.0.1, 9.0 etc. The current official version
of ADT is ***8.0.1***. This is what you should be using for your
development. There is an expirmental version of ADT 9.0 available, but
we have not yet tested it for use with MOTODEV Studio.

**Platform SDK:** This package contains the libraries used by the
version of the OS that your are targeting. The Gingerbread platform
version is ***2.3***, Froyo is 2.2, etc. You can have multiple versions
of the platform inside your SDK installation and you can have different
apps targeting different versions of the platform using the same version
of the tools. This is by design and that's a *Good Thing*.

**MOTODEV Studio:** MOTODEV Studio 2.0.0 uses ADT 0.99 and SDK tools r7
to build apps for Froyo (Platform 2.2) and earlier. MOTODEV Studio
***2.0.1*** is the subject of this release and it uses ADT 8.0.1 and SDK
tools r8 to build apps for Gingerbread (Platform 2.3) and earlier.
MOTODEV Studio 2.1 is in development and should be released in January
2011.

### Preliminary

For this update, I'm going to advise that you use the Google SDK
download tool. Install the SDK tools and platforms in a separate
location from your existing SDK and tools. Keep the Froyo SDK and tools
around until you are certain that the Gingerbread tools are working.
Especially if you have AVDs (emulator images) with a lot of effort in
them. You can get the SDK download tool from the [Android developer
site](http://developer.android.com/sdk/index.html) .

We didn't find any problems with workspaces. But, it's never a bad
practice to back up your code and workspace periodically.

If you aren't taking advantage of specific functionality in Gingerbread,
you can hold off with this release until MOTODEV Studio 2.1 is available
next month. There are a lot of changes taking place with the SDK and
platform tools and these will all get integrated into MOTODEV Studio 2.1
as a single package.

### The Big Easy

The easiest way to get MOTODEV Studio 2.0.1 is to just download the full
2.0.1 installer from the *MOTODEV Studio page* . Just uninstall version
2.0.0 and reinstall version 2.0.1. After the download is completed, this
takes about 5 minutes. After which you will be up to your elbows in
Gingerbread.

### The not-quite-so Big Easy

You can update MOTODEV Studio to version 2.0.1 using the online updater.
It's easy to invoke, just choose *Check for Updates* from the *Help*
menu and follow the instructions.

![](/assets/images{{ page.id }}/original(1).jpg)

This is where our testing problems occur--mostly due to network
timeouts. Because updates are coming from 3 different sources (Motorola,
Google, and Eclipse.org), the problems could be at any of these places
or any place between your computer and those sites. The network problems
have been sporadic and we don't have a conclusive thing to point at as
the source of all the problems.

Minimally, this process will take about 10 minutes. Even on a good
network connection.

If you get errors with the update mechanism, the workaround that we were
able to isolate was:

1.  Go into the preferences panel
2.  Choose *Install/Update*
3.  Choose *Available Software sites*
4.  From the list of sites, sort by location
5.  Delete or disable any locations that aren't using HTTPS as the
    protocol
6.  Leave the preferences
7.  Go back to *Check for Updates* from the *Help* menu and follow the
    instructions
8.  After the process is completed successfully, you will be instructed
    to restart MOTODEV Studio.

**OR**

Download and install the *full install package* mentioned above in
***The Big Easy***.

![](/assets/images{{ page.id }}/original(2).jpg)

If this still does not work for you, go back to the *Available Software
Sites* and choose each of the HTTPS connections and press the *Reload*
button.

### The other not-quite-so Big Easy

If you want to avoid possible network problems with the update site,
then you can choose to update MOTODEV Studio using a single archive
file. This archive is 125MB and can be found *on the download site*
(under *Eclipse Plug-in Version*; ignore the bit about *Requires an
existing Eclipse environment*). Locate the link for Eclipse Helios and
download the file to your computer.

To install the 2.0.1 update from this file:

1.  Go into the preferences panel
2.  Choose *Install/Update*
3.  Choose *Available Software sites*
4.  Make sure that none of the update sites have a checkbox next to
    their name
5.  Press the *Add* button
6.  Give the new update site a name, such as *MDS201*
7.  Press the *Archive* button
8.  Use the file navigation dialog to find the .zip file that you just
    downloaded
9.  Press *OK*
10. Exit the preferences panel
11. Go to *Check for updates* from the *Help* menu
12. There should be a total of 6 packages to update. 3 are labeled
    *MOTODEV Studio* and 3 are labeled *Sequoyah*. You want all of these
    updated.
13. Press *Next* several times to get through the installer. You will be
    prompted about installing unsigned content. These warnings are a
    result of the Google plugins, which we have no control over. If you
    trust these plugins, choose *Accept*.
14. After a few moments, the process is completed and you will be
    instructed to restart MOTODEV Studio.

![](/assets/images{{ page.id }}/original(3).jpg)

### Almost Home

At this point, you will need to switch the SDK from R7 (Froyo) to R8
(Gingerbread). Both MOTODEV Studio and the Google plugins recognize this
is a problem and will put up dialog boxes. Using the MOTODEV Studio
dialog, locate the new SDK and choose it. Dismiss both dialogs with
*OK*.

![](/assets/images{{ page.id }}/original(4).jpg)

After a few seconds, you should be in MOTODEV Studio 2.0.1. To verify
that everything worked, simply choose *Help-&gt;About MOTODEV Studio for
Android*. The about box should clearly show the version number.

![](/assets/images{{ page.id }}/original(5).jpg)

You're done! Now, get out there and write some awesome apps!

### &lt;audio src="sound-of-head-hitting-desk.mp3" /&gt;

If the updates still do not work, please choose *Help-&gt;Collect Log
Files*. Then, let us know on the *MOTODEV Studio Discussion Boards* .

Thank you for using MOTODEV Studio.

Eric
