---
category : blog
tags : [MOTODEV, Android]
title: Public Service Announcement--Back up your keystore
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

It snowed Sunday. Where we live that's unusual enough to cause us to
stand outside and watch it. My kids built a snowman with the neighbors
and threw snowballs until their fingers were frozen. The meteorologists
told us to expect 12" (30cm) of snow. These predictions seldom live up
to expectations, but we prepare for the worst and hope for the best. We
have plenty of supplies, but I made sure my generator was working,
bought some extra fuel, and filled a few jerrycans of fresh water.

![IMGP7595.jpg](/assets/images{{ page.id }}/small.jpg)

As it turned out, we got about half the snow we expected and we have
clear skies today. I was out the next morning, clearing sidewalks and
one of my neighbors was driving around with his snow blade on the front
of his tractor, clearing the streets. I like to think that part of what
made the storm so benign was the fact that our community was prepared
for it. If we weren't so prepared, it probably would've been much worse.

Be prepared. That's what the Boy Scouts say. So I'm going to tell you to
do something and you should do it. Right now. Before it slips your mind.
Because if you do this and bad things happen, you won't regret it.

***I want you to back up the keystore you use to sign your Android
apps.***

If you publish apps, whether it be to Android Market, another app store,
or just distributing them on your own website, you have a certificate
and that certificate is held in a keystore. If this file is lost, you
lose the ability to [update your
apps](http://developer.android.com/guide/publishing/publishing.html#marketupgrade)
on the Android Market. If you submit an app update signed with a new
certificate, your users will have to reinstall your app signed with the
new certificate. It's quite simple to avoid this: all you need to do is
back up the keystore containing your certificates and you're safe.

If you use the MOTODEV Studio export workflow, the keystore is in a file
named *motodev.keystore*. You can usually find the file at these
locations, although they may be slightly different depending on how your
machine is configured.

    ------------------------------------------------------------------------------
    Platform     File Path
    ------------ -----------------------------------------------------------------
    Windows 7    c:/users/&lt;user name&gt;/motodevstudio/tools
    Windows XP   c:/documents and settings/&lt;user name&gt;/motodevstudio/tools
    Mac OS       /Users/&lt;user name&gt;/motodevstudio/tools
                 **or**
                 ~/motodevstudio/tools/
    Linux        /home/&lt;user name&gt;/motodevstudio/tools
                 **or**
                 ~/motodevstudio/tools/
    ------------------------------------------------------------------------------

![](/assets/images{{ page.id }}/medium.jpg "IMGP7593.jpg")

If you use the Android tools or Eclipse plus ADT plugins, your keystore is wherever you told the tool to create it. But, a good place to start would be in the .android directory or in your Android SDK folder. There's no specific name for the keystore if you use keytool or the ADT workflow. You don't need to worry about .android/debug.keystore, which is used to sign apps for debugging. If this file is lost, the tools will recreate it.

Once you've located your keystore file, copy it somewhere
safe--preferably in another physical location from your computer.
Whether it's a flash drive, a network hard drive, your Dropbox account,
or a github repo, it doesn't matter. So long as it is safe from being
lost.

It would also be a good idea to store the password for the keystore. For
this item, I'm not giving any specific advice other than it shouldn't go
in the same place as the keystore itself. Your keystore password should
be difficult enough that it can't be reversed by brute force and used,
in case someone manages to get a copy of your keystore file. Every
person's situation is unique, but you need to have a strategy for
storing and recovering the keystore password other than "I'll remember
it".

### Disaster Recovery

If disaster does strike, you're covered. If you use the ADT signing
workflow, just copy your keystore back to your development computer
somewhere and begin using it. If you use the MOTODEV Studio signing
workflow, copy the file named motodev.keystore back to the folders named
above, depending on which host OS you use.

### Import Existing Keys

If you've started using a new keystore with the MOTODEV Studio workflow
and you want to use a certificate from an existing MOTODEV keystore,
it's easy to import the contents of one keystore into another. Go to the
view named "Application Signing Tool" and click on the button whose icon
represents "Import Keystore" (it's the second to the last button on the
right side).

![](/assets/images/{{ page.id }}/original(1).jpg)

Now you need to find the existing file that you saved somewhere. You
will need to remember the password for the import operation and choose
"JCEKS" (Java Cryptography Extension Key Store) for the store type. Then
press the "Load" button. If you have the password correct, the contents
of the keystore will appear in the list below. You can choose the ones
you wish to have in your existing keystore, which will be protected by
the existing keystore password.

![](/assets/images/{{ page.id }}/original(2).jpg)

Today my kids are back in school. Later, I'll retrieve the scarf, hat
and goggles when the snowman melts. I'm happy that our grass have some
moisture and the kids had fun. I'm also relieved that my preparations
weren't necessary. Besides, the gas in the generator will power my mower
this summer. So, I'm good.

Are you good? Have you followed my advice yet? Good. Now, let it snow...

Eric
