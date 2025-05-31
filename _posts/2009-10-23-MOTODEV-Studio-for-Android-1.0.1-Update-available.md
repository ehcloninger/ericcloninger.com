---
category : blog
tags : [MOTODEV, Android]
title: MOTODEV Studio for Android 1.0.1 Update available
hidden: true
---

**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello MOTODEV Studio users,

An update to MOTODEV Studio for Android is available today, 23 October
2009. A description of the update and how to install it is available on
the MOTODEV discussion boards at
*http://community.developer.motorola.com/mtrl/board/message?board.id=Studio_Android&thread.id=223*

Thank you for using MOTODEV Studio for Android.

Eric Cloninger

Sr. Product Manager

------------------------------------------------------------------------

**&gt;&gt;&gt;&gt; Content of Discussion Board Post &lt;&lt;&lt;&lt;**

------------------------------------------------------------------------

Announcement: MOTODEV Studio for Android 1.0.1 Update available

10-23-2009 01:49 PM

Hello Everyone,

We've made an update to MOTODEV Studio for Android. We found a few bugs
after we pushed the 1.0 installer live that we wish to fix. Also, with
this being our 1.0 release, we wanted to test the update mechanism to
ensure that it works to our satisfaction.

**Bugs Addressed**

The bugs we've addressed are:

Application Name entered by the user was not being properly saved in the
AndroidManifest.xml. This occurred for new projects not based on samples
or existing source.

When an update is performed that would cause an alert saying that
Eclipse has to be restarted for the update to take effect was not
displayed if the SDK installer had been executed on that same session.
On next updates of Studio, the Eclipse alert should be displayed on any
scenario.

Fixed DataTools component version matching rule. This will prevent
updates to the Eclipse DataTools component rendering the MOTODEV
Database perspective inoperable.

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

We've identified a problem in the Eclipse p2 updater and we are
submitting a bug report covering it. If you get a message saying
"Nothing to update, then you will need to use the following work-around:

1.  Select "Window-&gt;Preferences-&gt;Install/Update-&gt;Available
    Software Sites"
2.  Choose the item named "MOTODEV Studio for Android Updates" and press
    the "Test Connection" button. This will clear the cache for the site
    and allow new updates to be downloaded.
3.  You should now be able to receive updates. If you are still
    unsuccessful in receiving the update, go back to the dialog and
    modify the update site by appending a foward slash to the site URL.

**NOTES:**

1.  If you see a message about operations being blocked, do not be
    alarmed--this is the Eclipse update manager doing its work and the
    warning message is more severe than the condition. Give the update
    manager a minute or two in order to do its work and the updates
    should install properly.
2.  The dependency level between the MOTODEV Studio components and
    several Eclipse components (DTP, PDE) has changed with this update.
    If you've updated these Eclipse components in MOTODEV Studio
    separately, the 1.0.1 update may not install. If this occurs, you
    must uninstall MOTODEV Studio, reinstall, run the MOTODEV Studio
    update, and then update your Eclipse components. We apologize for
    the confusion, but we had the dependencies incorrect for these
    components in Studio 1.0.

Once you've installed the update, you will need to restart MOTODEV
Studio. Voila!

If you have any problems with the update or other questions or comments
about MOTODEV Studio for Android, please post your feedback in the
MOTODEV Discussion boards.

Thank you for using MOTODEV Studio for Android.

Eric Cloninger

Sr. Product Manager
