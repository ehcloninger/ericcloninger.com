---
category : blog
tags : [MOTODEV, Android, Release]
title: MOTODEV Studio 4.0
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

Hello everyone,

It's time again for for a release of MOTODEV Studio. I'd like to invite
you to get a cup of coffee or a cold beverage before starting to read
this article. There's stuff in here you'll want to read and it will
affect how you use MOTODEV Studio now and in the future. I'll try not to
bore you too much, but there are some changes and they will affect some
of you.

### Dependencies

I said I'll try not to bore you, but I think some background is helpful.
If you want the [TL;DR](http://en.wiktionary.org/wiki/TL;DR) version,
skip to the next major heading titled "*If You Read Nothing Else, Read
This*".

MOTODEV Studio consists of several things:

-   An installer that works on Windows, Linux, and Mac OS
-   The most recent version of Eclipse
-   The most recent Google ADT plugins
-   Tools and plugins that we've written, such as code snippets, App
    Validator, localization editor, etc.

In the early days of MOTODEV Studio, we built ADT ourselves because we
wanted to make some changes to the package. ADT needed some improvements
and we felt we could provide them. We realized early on that this was a
bad idea with the pace of ADT releases. Eventually we got where we were
just bundling ADT as it came from Google.

Some of the MOTODEV Studio features that we find useful call APIs inside
ADT. Some of the conveniences, such as our debugger launch
configuration, use information gleaned from ADT to populate the launch
dialog. In other words, we came to depend on ADT and that dependency
comes at a price.

If you've been using MOTODEV Studio for a while and have been reading
our discussion boards, you've no doubt seen some posts addressing new
ADT releases. When ADT releases come out, we stop development for a day
or two, install the plugins on our test machines, and determine if the
plugins are safe for everyone. Some times they are, and some times we
have to plan an interim release. The typical causes of interim releases
are an API or functionality change inside ADT, but some times it's a
command line change for one of the SDK tools, such as the emulator. We
can (and do) watch the external repo for the SDK tools and this helps us
plan our activities, but until the branch is tagged we really don't know
what the final results will be.

As the pace of tools releases have accelerated over the last six months,
we came to realize that we could not sustain the pace of testing and
interim releases to MOTODEV Studio. This is where the change comes in...

### If You Read Nothing Else, Read This

If you've been using MOTODEV Studio in previous releases, I hope you've
seen that we try to make the transition between updates as painless as
possible. We'll try to do the same with this release, but you have a
decision to make and it will affect how you install MOTODEV Studio now
and in the future.

Regardless of which choice you make, we will continue to provide support
for you on the *MOTODEV Studio Discussion Board* . Also, your projects
and workspaces will work no matter which version you use, so if you
decide to use one and change your mind later, your code and settings are
safe.

### Riddle Me This

Answer this question and use it to guide your actions: ***Do I always
want the latest plugins and SDK tools the moment the Android tools team
releases them?***

If the answer to that question is "*No*", then you should continue to
install MOTODEV Studio the way you always have. Just *download the
install package* from our web site or the Eclipse plugin archive and
work the way you always have. Or choose "*Update MOTODEV Studio*" from
the *MOTODEV* menu.

Why would you do this? Why wouldn't you always take the latest SDK? In
most cases, it's for stability. If you're in the middle of a project,
you don't want to introduce changes from underneath unless you need
them. Maybe there's a bug fix in the tools you need, in which case it's
perfectly valid to update, but most of the time you may want to wait for
the end of the project cycle before updating your tools.

By choosing this route, you accept that we (the MOTODEV team) may go the
full release cycle without an interim update. During which time the
Android tools team might've released 2 ADT updates and a newer set of
SDK tools. We'll have those integrated in the next release of MOTODEV
Studio, but it may be as long as 3 or 4 months until that time.

### MOTODEV Core Plugins

![](/assets/images{{ page.id }}/original(1).jpg)

If you answered "Yes" to my question above, then you will need to take a
different route. If you're updating the SDK and plugins frequently, we
assume you're a *bleeding edge* type of developer. For you, we've
created a slimmer set of tools that we call the "MOTODEV Core Plugins".
This package has no dependencies on ADT yet it delivers much of the
functionality of the full product. Using this package, you can update
Eclipse and/or ADT independent of the MOTODEV plugins and everything
should continue to work. You will notice that the locations of things
will have moved to accommodate working inside the Java Perspective, but
the functionality should still be there. There is a complete list of
differences in the *release notes* under the *New Packaging Option*
section.

The tradeoff is that the MOTODEV Core Plugins are truly a set of plugins
and there is no installer. The burden of creating and maintaining your
development environment falls on you. You will need to download and
install Eclipse on your own prior to installing the MOTODEV Core
Plugins. Creating an update site for the MOTODEV Core Plugins is no
different than installing the ADT plugins--it's just another update
site. It's not a difficult task and we've documented the steps you need
to go through in our *installation guide* .

![](/assets/images{{ page.id }}/original(2).jpg)

We are looking forward to people using this new package. We've wanted to
get to this point for quite some time and now that we're there, we hope
it makes your development easier. If you have problems or suggestions,
let us know on our *discussion boards* .

We're planning some documentation for users of the MOTODEV Core Plugins.
Where MOTODEV Studio users had a perspective to work in, these plugins
access the MOTODEV features inside other perspectives. This will require
some time to create, but watch the discussion boards and the @motodev twitter account for
announcements.

### In Other News

Now that we've gotten the big one out of the way, let's talk about the
other changes we've made to MOTODEV Studio.

### Code Generation

I released my first app on Google Play a few weeks ago. I'm biased, but
I have to say that I love our feature that takes the contents of a
layout and generates the Java code for it. It makes the tedious actions
much less of a burden. If you haven't noticed this in the past,
definitely give it a look.

We've added to the existing tools to include code generation for menus.
Start by creating res/menu/menu.xml, then choose "Generate Java Code
Based on Menu XML Files" from the MOTODEV menu. You'll get a wizard
dialog that will show you all your projects and your Java source files
that can handle the menu code.

![](/assets/images{{ page.id }}/original(3).jpg)

For example, consider this menu definition:

    <?xml version="1.0" encoding="utf-8"?>
    <menu xmlns:android="
                http://schemas.android.com/apk/res/android
                " >
     <item android:id="@+id/about" android:title="@string/about"></item>
     <item android:id="@+id/settings" android:title="@string/settings"></item>
     <item android:id="@+id/new_game" android:title="@string/new_game"></item>
     <item android:id="@+id/undo" android:title="@string/undo"></item>
    </menu>
        

The result is the Java code below. It's not groundbreaking, but it does
save you a lot of typing. If you modify the menu.xml later in your
project, you can run the wizard again to regenerate the
*onOptionItemsSelected()* method.

    public boolean onCreateOptionsMenu(Menu menu) {
                MenuInflater inflater = getMenuInflater();
                inflater.inflate(R.menu.main, menu);
                return true;
                }

                public boolean onOptionsItemSelected(MenuItem item) {
                if (item.getItemId() == R.id.about) {
                } else if (item.getItemId() == R.id.settings) {
                } else if (item.getItemId() == R.id.new_game) {
                } else {
                return super.onOptionsItemSelected(item);
                }
                return true;
                }
        

If you're using the MOTODEV Core Plugins, you'll have to use the
existing Eclipse menu hierarchy. For example, right click on a Java file
in the Project Explorer, choose the Source sub-menu and you'll notice
the MOTODEV code generation items at the bottom of the list.

### Database Management

We've been shipping a database tool since the first releases of MOTODEV
Studio. This is one of the pieces that needed a lot of cleanup when we
refactored the code. While we were in there, we spent some time cleaning
up the user interface and removing pieces that didn't make sense for
SQLite databases. The result is a much cleaner and easier to use tool.
If you ship a database with your app or if you need to inspect databases
as part of your debugging, you'll appreciate this feature.

![](/assets/images{{ page.id }}/original(4).jpg)

The database feature is in its own perspective because there are quite a
few different views. Go to "*Window-&gt;Open Perspective-&gt;Other*" and
find the *MOTODEV Database* perspective. Once you've opened it the first
time, you can select it from the perspective chooser in the upper right
corner of the IDE.

You can use the database perspective with any running emulator. You can
also use it with developer phones that have root permissions enabled for
the /data partition.

If your phone doesn't have root access, don't despair. Write your
databases to the SD card by modifying the
[SQLiteDatabase.openOrCreateDatabase()](http://developer.android.com/reference/android/database/sqlite/SQLiteDatabase.html#openOrCreateDatabase(java.lang.String,%20android.database.sqlite.SQLiteDatabase.CursorFactory,%20android.database.DatabaseErrorHandler))
call with the full path to the card. Once you've created the SD card
database, mount the phone as removable storage and use the "Map
Database" feature to access the contents of the database on your
production phone. Once you've solved your problems, reset the database
API back to the default partition so your data remains secure.

### App Validator

You should be using the App Validator to pre-flight your apps before you
distribute them. It provides many tests that can keep you from having to
do quick turnaround releases.

Since the App Validator was released nearly two years ago, we've spent
most of our efforts on providing new types of tests. For this release,
we've added some new tests, but we've also focused on usability. In
addition to working from the Problems view, the App Validator now
supports *Quick Fixes*. If the tool can determine the best course of
action for you to fix your app, it will put a light bulb next to the
icon in the problems view or in the left margin of the editor window. To
fix the problem, right click and choose *Quick Fix*. Follow the wizard
instructions after that.

![](/assets/images{{ page.id }}/original(5).jpg)

App Validator is available as a *stand-alone tool* , which will benefit
larger development teams and companies that utilize continuous build
systems. By putting App Validator into your build scripts, you can watch
for new problems as they arise.

Developers who have unique conditions that need to be tested can
customize the App Validator with the *App Validator SDK* . There are
samples and documentation to guide you to extending this powerful tool
to meet your own needs. We know you don't want to create a new tool from
scratch, which is why we've distilled the SDK down to just the pieces
you need to test and report on your unique conditions.

### Version numbering

We bumped the version number to 4.0. Normally we bump the number when we
go to the next release of Eclipse. This time we bumped the version
number because we made some significant changes to the architecture and
release mechanisms of MOTODEV Studio. We felt like this was a
significant change and we wanted to cement in users minds that something
big was up.

### The Road Ahead

We're starting to look at what we're going to do for Studio 4.1. We're
doing some investigations and we're starting to determine what features
we will support for the next release.

We're planning to work on a new Eclipse package that
developers can use as the basis for their Android work. This is a
community project that will help all developers, regardless of whether
they use MOTODEV Studio or just want to use the basic features of
Eclipse + ADT. It will also be what we use for the foundation of the
next version of MOTODEV Studio and what we will use to test our plugins
against.

As we were finishing up the release efforts for Studio 4.0, the Android
tools team released a preview of ADT 20. We suspect we will get requests
to support the release in the full version of MOTODEV Studio, but we
have a busy schedule the next few months. Our first look at the package
showed many changes in ADT 20. When Google releases this package to the
world, we'll evaluate and report on what we can do.

Support for C code with the Native Development Kit (NDK) is coming in
ADT20. This is similar to a feature that we integrated into MOTODEV
Studio from a contribution to the Eclipse Sequoyah project. The
contributor who submitted to Sequoyah also submitted to the AOSP, where
the code is a better fit. When ADT 20 comes out with NDK support, we
will begin to retire the MOTODEV Studio feature.

Thanks again for using MOTODEV Studio and App Validator. I hope this
release is something you will enjoy and use to create great apps. Let us
know if you have ideas for things we can do to make your lives easier.

Eric Cloninger, Sr. Product Manager, MOTODEV Tools

Greg Wilson, Developer Evangelist, Motorola Mobility

Julia Perdigueiro, Technical Lead, MOTODEV Tools
