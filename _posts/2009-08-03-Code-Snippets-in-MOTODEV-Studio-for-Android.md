---
category : blog
tags : [MOTODEV, Android]
title: Code Snippets in MOTODEV Studio for Android
hidden: true
---

**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

Application development, whether for desktop or mobile, is 10%
inspiration and 90% perspiration. MOTODEV Studio for Android has
features that shift the ratio in favor of inspiration.

Code Snippets is one such feature, and it's available in the lower left
corner of the MOTODEV Studio perspective. When you launch MOTODEV
Studio, you should see this perspective, but if not, it is accessible
from the Window-&gt;Open Perspective menu. Code Snippets are often-used
code, made available in template form, for you to drag and drop into
your source files. By using common and proven code for your work instead
of entering the same sequences each time, you will achieve more stable
and better performing applications.

One place where snippets can be useful is in the onCreate() method. In
onCreate(), you find yourself doing the same things over and over. For
example, you might wish to retrieve the shared preferences for your
application. Using the Snippets view, either drag the words Retrieve
shared preferences from Snippets into an opened source file, or double
click on the item in the Snippets view and the code is entered at the
cursor. Voilà, it's that easy!

![Code Snippets in MOTODEV Studio for
Android](/assets/images/{{ page.id }}/3787036964_b283ae92c1.jpg)

Code snippets are just pieces of code and not a full application, so you
will often need to replace default values or include other necessary
packages. In the case of the example shown in the screenshot, you would
need to import android.content.SharedPreferences before the code will
compile. Most of the errors you will encounter from using snippets can
be resolved by replacing default values (often in UPPERCASE) or using
the QuickFix tool (Ctrl+1) to let the IDE fix them.

**Express Yourself!**

Do you have a bit of code that you use over and over? Of course you do.
You can add your own bits of code to the Code Snippets View and share
with your friends, co-workers and those guys on the discussion boards.

![Code Snippets in MOTODEV Studio for
Android](/assets/images/{{ page.id }}/3787036994_4e90a11e67.jpg)

To create your own snippets, start by clicking in the middle of the
Snippets view and choose "Customize". This will bring up a dialog where
you can manage the existing snippets as well as adding your own
categories and new items to your categories.

You cannot modify the categories or snippets that ship with MOTODEV
Studio, but you can create your own categories and snippets within those
categories. As you can see in the screenshot below, I've created a new
category called "Eric's Snippets" by right-clicking in the snippet list
and choosing "New-&gt;New Category".

![Code Snippets in MOTODEV Studio for
Android](/assets/images/{{ page.id }}/3787037028_445030a17b_o.jpg)

Once you've created a category, you can add individual snippets by
right-clicking on the category name and choosing "New-&gt;New Item".
With the new item, you can create the code template in the editing area
at the bottom. After pressing "OK", you can access your new template
from the Code Snippets view.

![Code Snippets in MOTODEV Studio for
Android](/assets/images/{{ page.id }}/3787037052_33520f4113.jpg)

If you have values that need to be entered by the user of your template,
you can add them in the Variables editor on this screen. Simply create a
new variable and provide a default value for the variable. When the code
snippet is activated, the user is presented with a dialog asking for the
value they wish to use. If they wish to use the default, they need only
dismiss the dialog. If they wish to modify the values, they do so by
modifying the values in the table at the top of the dialog and view them
in the area below.

Code Snippets are only one of the cool new features that we've added to
MOTODEV Studio and more new features are coming in the future. I hope
that you will try our *product* and *give us feedback* on what works,
what doesn't, and what could work better.

Enjoy!

Eric Cloninger ( [@ericc on Twitter](http://twitter.com/ericc) )

Sr. Product Manager
MOTODEV Studio
