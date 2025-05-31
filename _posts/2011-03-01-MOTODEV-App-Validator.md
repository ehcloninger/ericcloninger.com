---
category : blog
tags : [MOTODEV, Android, Release, Static-Analysis, i18n]
title: MOTODEV App Validator
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello everyone!

I hope everyone is excited by all the new devices that are coming out.
We've been busy for months, it seems, getting ready for the Motorola
XOOM and the Honeycomb SDK. Now that they are here, I imagine everyone
is hard at work making tablet versions of their apps.

Before you press that *upload* button to send your new tablet-enabled
app to the Market, let's take a moment and examine it in more detail.
Are you sure it's really ready? You tested your app and probably had
your co-workers or family give it a go, but how extensive were your
tests? Do you have a lot of corner and edge cases in your tests? Have
you run your app on a carrier in another country or switched locales?
This month, I'm going to give an in-depth tour of our latest tool and
show how you can use it to help answer these questions.

### Presenting... MOTODEV App Validator

![](/assets/images{{ page.id }}/original(1).jpg)

The MOTODEV App Validator has been available since the MOTODEV Studio
2.0 release in October 2010. We've been rather quiet about it as we've
been steadily improving it. We're now ready to show it off. The App
Validator is a static testing tool. By *static*, I'm referring to the
fact that it doesn't run your app on a target OS. Rather, this tool
looks at the .APK file that is your app and extracts all the information
from it that it can. It uses this information in a series of checks.
These checks range from things like *your minSDKVersion isn't correct
for the screen sizes you are using* to *you have strings for this
country, but not that one*. There are also checks for settings or
conditions that could keep your app out of the market or filtered from
certain devices.

![](/assets/images{{ page.id }}/original(2).jpg)

From MOTODEV Studio, if you right-click on a project, you will get a
context menu. Near the bottom of this menu is a choice called *Validate
Android Application*. If you choose this menu item, the App Validator
will execute against the built version of your app (found in the 'bin'
folder).

The App Validator will take approximately 10-15 seconds to complete its
task. When it is finished, most of the time you will see the results in
the problems view. We chose to use this view because it has the best
integration experience for Eclipse users. Any problem that can be
identified as being in a specific file or with a specific line number is
tagged with a marker and you can double click the file in the problems
view to open it.

![](/assets/images{{ page.id }}/original(3).jpg)

### WARNINGS??? ERRORS!!!

The MOTODEV App Validator looks for conditions that could be a problem,
but it doesn't read minds. The operative word here is *could*. Most of
the conditions that the App Validator detects will result only in a
slightly diminished user experience.

For example, a graphic that exists in the drawable folder but not in the
drawable-ldpi folder isn't necessarily harmful. Android will find the
larger graphic and scale it to the smaller size. Perhaps it won't look
as nice, but it will still work. Also, a string that isn't translated
from English to Chinese will lead to a warning in the App Validator, but
it could be that string is never displayed and doesn't need to be
translated. The App Validator doesn't know your intent, so it's always
going to be cautious.

![](/assets/images{{ page.id }}/small.jpg)

If you don't understand what a message is telling you, we've created an
online document with more details of each message. This document
describes the messages in more detail and gives examples on how to
modify your application to deal with them. You can find this document
*here* .

When reading this document, there are two words that have very specific
meanings. One is '*checker*' and the other is '*condition*'. A checker
is an Eclipse plugin that handles a particular group of problems. One
such checker is called *localizationStrings*. The localizationStrings
checker goes through your strings.xml files and compares the contents of
those files. If a potential problem is found, it's called a condition.
Each condition that is raised results in a message that is reported back
to you.

The best advice I can give on this is not to overreact. There is no
requirement for you to pass the validator with zero warnings, so don't
feel pressured to do so. If you get an error, it's probably something
that needs to be addressed, but otherwise... (see the image)

### More than one way to skin a cat

Another way you can invoke the App Validator is to use the *MOTODEV*
menu. When clicked, you will see a sub-menu called *App Validator* which
has two items.

![](/assets/images{{ page.id }}/original(4).jpg)

The first item allows you to navigate to a folder and choose one or more
.APK files to validate. The results of this operation are put in the
console view since there are no projects that can be identified as
belonging to those apps. The second item allows you to choose multiple
open projects to validate.

Even though the App Validator is integrated nicely into MOTODEV Studio,
underneath it is really a command line application. You can use it from
your favorite scripting environment. There are a lot of options for the
tools and you can find a complete list of parameters in the online help
within MOTODEV Studio (***Help-&gt;Contents***). For example, you can
modify the severity of specific messages to be more or less strict than
we've prioritized them by default.

![](/assets/images{{ page.id }}/original(5).jpg)

You can also direct the output into different formats for viewing with
other tools, such as a spreadsheet. You might find this useful if you
have an app with hundreds of errors. When used with the *Auto-sort*
feature or in creating a pivot-table, it could be very helpful in
working through a large list of reported issues.

![](/assets/images{{ page.id }}/original(6).jpg)

You can also modify the way certain checkers behave. Since each checker
has an ID associated with it, you can increase or decrease the severity
of the messages it generates. For example, this command line:

    appvalidator MyApp.apk -xw localizationStrings
        

Tells the App Validator to treat localizationString conditions less
severely.

### MOTODEV App Validator online

This month, we're pleased to announce the availability of the MOTODEV
App Validator as an online service. MOTODEV members can access the tool
at *http://developer.motorola.com/testing/app-validator* . After you've
logged in to MOTODEV and accepted the terms of use, you can drag your
.APK files onto the page.

![](/assets/images{{ page.id }}/original(7).jpg)

When you drop your apps on the page, the files will upload to the
MOTODEV server. From there, a daemon version of the App Validator runs
on the MOTODEV server. The results are displayed back in the browser. If
you have a question about a message and want more details, there is a
link with the message that will take you to an online document for more
information about that specific message.

![](/assets/images{{ page.id }}/original(8).jpg)

### Six ways from Sunday

The MOTODEV App Validator has many configuration options. We designed
this tool to give developers choices in how to run and how to generate
the results. The documentation for the App Validator can be found in the
online help within MOTODEV Studio (***Help-&gt;Contents***). Most of the
command line parameters can also be entered into the version of the App
Validator that is part of MOTODEV Studio. If you open the preferences
dialog and click on *MOTODEV Studio-&gt; Android App Validator*, you can
enter the parameters that are given when the tool is executed.

![](/assets/images{{ page.id }}/original(9).jpg)

We're launching the online App Validator this month and we're going to
make more changes to it in upcoming months. Because it's a different use
case from the stand-alone and integrated tools, it probably will not
have feature parity with the other versions. At least not for some time.
We want to give you the ability to filter the results and download them
in different formats. We'll also put some practical limits in place to
keep submitted apps from dropping the online version to its' knees.
Please check back often to see the progress we make.

### The ugly part

The App Validator uses the same underlying technology that Eclipse uses
for dynamic plugin loading. While it provides us a great deal of
flexibility, nobody has ever accused Eclipse of being speedy.
Fortunately, modern desktop operating systems cache things, so after the
first time, the App Validator should load faster. The longest I've seen
the App Validator run was about 45 seconds and that was on an app that
returned 142,311 warnings (I'll never tell which ;-) ). So, once the
plugin loader is operating, it's reasonably fast.

### We get letters

We're really excited about the App Validator and I hope you get a chance
try it out. The goal with this tool is to help you create the best app
you possibly can. If this tool catches a problem before it occurs, then
we all benefit. The customer is happy with your great app, you're happy
you have users, and we're happy to be selling way-cool devices.

If you have a suggestion for a new *checker* or maybe you feel a
condition should be changed, let us know about it. We're more than happy
to talk about ways to make the tool even better. If the App Validator
found a particularly difficult problem for you, let us know about it. If
you're having problems decoding the results, we're here to help. Just
pop over to the *MOTODEV Studio discussion board* and let us know.

Thank you for using MOTODEV Studio.

Eric
