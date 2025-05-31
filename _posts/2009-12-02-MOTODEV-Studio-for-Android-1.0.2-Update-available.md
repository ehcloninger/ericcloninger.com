---
category : blog
tags : [MOTODEV, Android]
title: MOTODEV Studio for Android 1.0.2 Update available
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello MOTODEV Studio users,

An update to MOTODEV Studio for Android is available today, 2 December
2009. This update provides support for the Android 2.0 (Eclair) SDK.
Version 1.0.2 is available as an online update as well as a full
installer. A description of the update and how to install it is
available on the MOTODEV discussion boards at
*http://community.developer.motorola.com/mtrl/board/message?board.id=Studio_Android&thread.id=339*

Thank you for using MOTODEV Studio for Android.

Eric Cloninger

Sr. Product Manager

------------------------------------------------------------------------

**&gt;&gt;&gt;&gt; Content of Discussion Board Post &lt;&lt;&lt;&lt;**

------------------------------------------------------------------------

Hello Everyone,

We've made an update to MOTODEV Studio for Android.This update provides
support for the Android 2.0 (Eclair) SDK as well as fixing some small
issues found since the last update. The new release is provided as an
Eclipse update as well as a full installer. Release notes, installation
instructions, and the installers are available on the MOTODEV download
site.

**Android 2.0 SDK**

The ADT 0.94 plugins have a new mechanism for installing the Android
SDK. After applying this update, you will need to use
"Window-&gt;Android SDK and AVD Manager" to find and install the 2.0 SDK
from Google's site. This is different from the previous version of
MOTODEV Studio. If you install the product (as opposed to running the
update), you will be taken to this screen automatically.

**Accessing the Update**

Before accessing the update, check a few things first. Go to
"Window-&gt;Preferences-&gt;Install/Update-&gt;Available Software Sites"
and ensure that there is a site named "MOTODEV Studio for Android
Updates" and that it is enabled. Also, if you are working behind a
proxy, you will need to enter the proxy information in the preference
panel named "General-&gt;Network Connections". Mac users, the
Preferences screen is on the "MOTODEV Studio" menu. While in this
preference screen, you should test your network connection by pressing
the "Test Connection" button.

To check for updates, choose "Help-&gt;Check for Updates". The MOTODEV
Studio update site uses the HTTPS protocol and requires a password, so
you will need to supply your MOTODEV user name and password.
Fortunately, if you have a secure machine, you need only enter this
information once and choose the "Save Password" check box. When the
update is ready, you will see it mentioned in the updates window. The
update is approximately 1MB in size and should not take more than a few
moments to install.

**P2 Updater Bug**

We've identified a problem in the Eclipse p2 updater and we submitted a
bug report to Eclipse covering it. If you get a message saying "Nothing
to update", then you will need to use the following work-around:

1.  Select "Window-&gt;Preferences-&gt;Install/Update-&gt;Available
    Software Sites"
2.  Choose the item named "MOTODEV Studio for Android Updates" and press
    the "Test Connection" button. This will clear the cache for the site
    and allow new updates to be downloaded.
3.  You should now be able to receive updates. If you are still
    unsuccessful in receiving the update, go back to the dialog and
    modify the update site by appending a forward slash to the site URL.

**NOTES:**

If you see a message about operations being blocked, do not be
alarmed--this is the Eclipse update manager doing its work and the
warning message is more severe than the condition. Give the update
manager a minute or two in order to do its work and the updates should
install properly.

Once you've installed the update, you will need to restart MOTODEV
Studio.

If you have any problems with the update or other questions or comments
about MOTODEV Studio for Android, please post your feedback in the
MOTODEV Studio Discussion board.

Thank you for using MOTODEV Studio for Android.

Eric Cloninger

Sr. Product Manager
