---
category : blog
tags : [MOTODEV, Android, i18n]
title: MOTODEV Studio, Your Way
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

MOTODEV Studio is capable of working with you if English is not your
first language. In this article, I'll show how you can use MOTODEV
Studio in Chinese, Portuguese or Spanish to develop great Android apps.

### Installing a Localized MOTODEV Studio

Since version 2.0 was released in October 2010, MOTODEV Studio can be
installed with language packs for Simplified Chinese, Brazilian
Portuguese, and Spanish. Simply choose the language you want from the
installer.

![Screenshot - 22_12_2010 ,
12_33_39.png](/assets/images/{{ page.id }}/original(1).jpg)

The legal notices were translated and can be viewed in the installer
before you install the product. We've made every attempt to ensure that
the translations are accurate, but you should refer to the English copy
if there are any doubts.

The *installation guide* and *release notes* for Studio 2.0 are
available online as well. To view these documents in your choice of
language, press the ***Change*** link in the upper left corner.

MOTODEV Studio is built from the Eclipse IDE and Eclipse is capable of
determining how it should display text, currency, and numbers correctly.
If your operating system works correctly for your language, then Eclipse
(and MOTODEV Studio) should work correctly also. From the moment the
splash screen appears, you should start seeing messages, menus and
content in the language you chose as shown in the following screenshots.

![Screenshot - 22_12_2010 ,
12_43_31.png](/assets/images/{{ page.id }}/original(2).jpg)

![Screenshot - 22_12_2010 ,
12_43_58.png](/assets/images/{{ page.id }}/original(3).jpg)

If you continue to see English strings, you will need to force MOTODEV
Studio to launch in the language you chose in the installer. Quit
MOTODEV Studio and locate the shortcut you use to launch. You will need
to add a parameter to force language selection. This parameter is "-nl "
followed by an ISO language and country code. For Brazilian Portuguese,
the selector is "pt_BR". Spanish is "es" and Chinese is "zh".

### Updating MOTODEV Studio after Installation

If you've already installed MOTODEV Studio you can add the language
packs at a later time. There are two ways you can do this. One method
uses the built-in Eclipse update mechanism and the other uses the
MOTODEV Studio Download Components screen.

Either method will get you to the same eventual goal, but they do so in
different ways. The only advantage to using the first method over the
second is if you are experiencing network delays when installing the
language packs.

### Installing Additional Language Packs via Eclipse Update

The first way is to go to the MOTODEV site and download the language
pack archive. This file is approximately 10MB in size.

1.  Once the archive file is successfully downloaded, launch MOTODEV
    Studio and choose Help-&gt;Install New Software.
    ![Screenshot - 22_12_2010 ,
    12_39_06.png](/assets/images/{{ page.id }}/original(4).jpg)
2.  From the installation screen, press the Add button.
3.  In the next screen, enter a name for the item, such as "MOTODEV
    Language Pack".
4.  Next, press the Archive button to locate the .zip file that you
    downloaded. Press OK once you've filled out the form.
5.  Back in the Install Software screen, clear the check box labeled
    Contact all update sites during install.
6.  Press the Next button and follow the instructions to finish the
    installation.

You will be notified that the language packs are unsigned and you will
have to decide if you accept this fact. When the installation is
completed, you will be asked to restart MOTODEV Studio.

Similar to the installation workflow above, if your operating system
works for your language choice, then MOTODEV Studio should also. If not,
you will need to modify the MOTODEV Studio launch shortcut to include
the "-nl" parameter described earlier.

### Installing Additional Language Packs via Download Components

Another way to install language packs to MOTODEV Studio is to use the
Download Components screen.

1.  From the MOTODEV menu, choose Download Components. ![Screenshot -
    22_12_2010 ,
    12_36_20.png](/assets/images/{{ page.id }}/original(5).jpg)
2.  From the Download Components screen, choose Language Packs.
    ![Screenshot - 22_12_2010 ,
    12_37_57.png](/assets/images/{{ page.id }}/original(6).jpg)
    In this list, you will see many choices, not just Chinese,
    Portuguese and Spanish. That's because these language packs come
    from the Eclipse Foundation servers and Eclipse has many language
    choices. If you choose one of these other languages, only the core
    Eclipse features are localized. The parts of MOTODEV Studio that are
    unique to Motorola Mobility will continue to be shown in English.
3.  Once you've chosen a language, you will be asked to accept a license
    agreement and then MOTODEV Studio will begin downloading the
    language packs from different servers. The Eclipse strings will come
    from eclipse.org and the MOTODEV Studio strings will come from
    Motorola Mobility's server.

When the installation is completed, you will be asked to restart MOTODEV
Studio. Similar to the installation workflow above, if your operating
system works for your language choice, then MOTODEV Studio should also.
If not, you will need to modify the MOTODEV Studio launch shortcut to
include the "-nl" parameter described earlier.

### Final Words

We hope developers find this feature useful. Even though parts of the
product will continue to be in English, we hope the parts that are
translated help make developers lives easier.

We will continue toward the goal of a 100% translated product, but that
will take time to complete. MOTODEV Studio contains many components from
different sources. Some of those sources are open projects where we can
add localized content easily. Other sources do not accept translations,
so we have to be content with what we have.

If you have questions or encounter problems, just sign in to the
*developer discussion boards* and ask a question. We'll be happy to help
solve your problems.

Thank you for using MOTODEV Studio.

Eric
