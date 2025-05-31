---
category: blog
tags: [Klocwork, Android, Linux, Static-Analysis]
title: Bringing up your Android Dev Environment from Scratch
hidden: true
---

**NOTE:** This blog post was originally hosted on [Klocwork's Android
blog](http://blog.klocwork.com/android-development/). At some point
those posts may not continue to exist. I've made every attempt to
preserve the original content with only formatting changes to fit this
site.

**by Eric Cloninger**

As (bad) luck would have it, the solid state drive with my Linux
partition died the week of Thanksgiving. I have backups of the data, so
I haven't lost anything other than time. I used that partition for
testing and building the Android platform sources and that's about it.
It's aggravating to lose the SSD, but not devastating. All my important
files are stored elsewhere.

After the hard drive died, I needed to get the latest code for Android
4.2.1 that came out in late November. Instead of going through the
hassles of installing a new hard drive, I just installed
[VirtualBox](https://www.virtualbox.org/) and pulled down one of the
ready-made installations of Ubuntu 10.04 (aka Lucid Lynx) from
[VirtualBoxes.org](https://www.virtualbox.org/). If you’re starting from
scratch, you should start with version 10.04 because that is what Google
is using at the moment and all their scripts are set to use it. When I
started a few months ago, I used Ubuntu 12.04 because it was what was
new. I kept running into issues with the newer compilers and libraries
that are in the later installations. While there are guides to building
with 12.04, if you want to save yourself the hassle, I suggest you use
10.04.

Within 2 hours of pulling down the 10.04 virtual machine image, I had
all the patches I needed installed into the virtual machine and I was
building the Android sources. The performance was good enough for the
time that I was able to get some work done.

When I started with this fresh VM, I ran into a few problems that
weren't outlined in the [official Google
documentation](http://source.android.com/source/initializing.html). Over
the Christmas break I stuck a new drive in my PC and went through this
exercise again. I thought I would point these out for anyone else who
may also be starting out from scratch. There's nothing earth-shattering
in here—thousands of developers have gone down this road before, but
it's good to get this information in one place.

Java 6
------

If you're targeting an Android platform after release 2.2 (Froyo), you
will need Java 1.6. Ubuntu 10.04 has OpenJDK by default but you will
want Sun Java 6. OpenJDK can be made to build Android, but the path of
less resistance is to use Sun Java. I say *less resistance* because it’s
certainly not *no resistance*. The Canonical online software archive no
longer has the Sun binaries, so you will need to download it from
Oracle. Getting the official distribution of Java 1.6 from the Oracle
site is proving to be trickier as we get closer to the [Java 6
end-of-life](http://www.oracle.com/technetwork/java/eol-135779.html).

I've done this twice in the last 4 months and there are two relatively
painless ways I've found to install Java 6 on Linux.

### Download from Oracle

The first way is by downloading the Java SE 6 JDK from the [Oracle
downloads
page](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
Make sure to get the .bin version and not the rpm version that’s
intended for Fedora. From there, follow the steps that are explained at
[this AskUbuntu
thread](http://askubuntu.com/questions/67909/how-do-i-install-oracle-jdk-6)
on StackExchange, courtesy of
[fossfreedom](http://askubuntu.com/users/14356/fossfreedom).

**NOTE:** For the sake of this example, I’m using JDK 1.6b38, the
version that was released at the time.

    chmod a+x jdk-6u38-linux-i586.bin
    ./jdk-6u38-linux-i586.bin
    mv jdk1.6.0_38 java-6-oracle
    sudo mkdir /usr/lib/jvm
    sudo mv java-6-oracle /usr/lib/jvm
    # switch to Oracle JDK 6 - webupd8.googlecode.com hosts a nice script to help with this.
    # 0.5b refers to the script version<
    wget http://webupd8.googlecode.com/files/update-java-0.5b
    chmod +x update-java-0.5b
    sudo ./update-java-0.5b

### Build from Source

Another way to install Java is by building the binaries from the
sources. There are many valid reasons for doing this and your use case
may be among them. Fortunately, there is a [very helpful bash
script](https://github.com/flexiondotorg/oab-java6) that handles nearly
all the hard work for you. Credit for this goes to [Martin
Wimpress](https://github.com/flexiondotorg) and [Janusz
Dziemidowicz](https://github.com/rraptorr) for maintaining the scripts
on GitHub.

    sudo apt-get purge sun-java*
    mkdir ~/src
    cd ~/src
    git clone https://github.com/flexiondotorg/oab-java6.git
    cd ~/src/oab-java6
    sudo ./oab-java.sh

You may get some error reports from the script that will require you to
install libraries. Take the names from the error log and feed them into
your favorite package manager for installation. For example,

    sudo apt-get install libc6-i386 gnupg ia32-libs lib32ncurses5

After several iterations, you should see the script succeed. You will
now have a local repository for installation packages in /var/local. You
will use this repository for your installation of Java 6.

    sudo apt-get update
    sudo apt-get install sun-java6-plugin sun-java6-jre sun-java6-bin
    java -version

If you don't uninstall OpenJDK or if you need Java 7, you can force
Ubuntu to switch between them by [setting the **$JAVA_HOME**
variable](http://stackoverflow.com/questions/6477415/how-to-set-java-home-in-ubuntu).
You may also need to force the system settings to choose your newly
built Java instead of OpenJDK or Java 7 with *update-alternatives.* The
commands to do this, courtesy of [Tyler
Wagner](http://www.tolaris.com/2010/06/10/installing-sun-java-on-ubuntu-lucid/),
are

    sudo update-alternatives --set java /usr/lib/jvm/java-6-sun/jre/bin/java
    sudo update-alternatives --set javaws /usr/lib/jvm/java-6-sun/jre/bin/javaws

### Memory, Like the Corners of my Mind

Chances are, you will want to increase the default size of the stack
allocation for the Java compiler. This is done with the **-Xmx** and
**-Xms** parameters. There are a number of ways you can set this
depending on your needs. If you find builds failing because the Java
compiler runs out of memory, see what the allocation is in the log file
and bump it up. I set these with the JAVA_OPTS environment variable in
my .bashrc script using the following statement.

    export JAVA_OPTS='-Xmx4G -Xms512m'

Python 2.7.3
------------

The Android team uses a homegrown tool called *repo* to interact with
their source servers. Repo requires a version of python between 2.5 and
2.7.x. Further, it needs several modules enabled that are not enabled by
default in Ubuntu 10.04. Namely *readline*, *SSL*, and *sockets*.

### Readline

Readline is enabled in some versions, but usually the supporting library
may not be installed, so it doesn't load in python. You can force
readline to be installed by simply installing the supporting library
with

    sudo apt-get install libreadline5-dev

### SSL and Sockets

SSL and sockets are disabled from the build, so you have to install the
correct libraries **[and]{style="text-decoration: underline;"}** rebuild
python in order to correct this. Fortunately people on the web have
excellent instructions and deserve all the credit for publishing them.
In this case, the responsible parties are JC Brand and Grig Gheorghiu.

    http://www.opkode.net/media/blog/compiling-python-with-readline-support-on-ubuntu-linux

    http://agiletesting.blogspot.ca/2008/05/compiling-python-25-with-ssl-support.html

1.  Download the python 2.7.3 sources from python.org (find the source
    tarball link and extract it someone on your drive)
2.  Install the dependent libraries you need with
    **`sudo apt-get install libssl-dev libreadline5-dev`**
3.  Go into modules/setup.dist and find the lines defining the build
    commands for socket and ssl. Remove the hash marks that comment
    those build commands out.
4.  From a bash prompt at the root of the python folder, type
    **`./configure`**
5.  Build python with **`make –j`**
6.  Install python with **`make install`**
7.  I found that I had to reboot to get python 2.7.3 to displace python
    2.6.5 that is installed with Ubuntu 10.04.

If you need to have more than one installation of python, there is a
useful script called
*[pythonbrew](https://github.com/utahta/pythonbrew)* that allows you to
switch between them.

Update != Upgrade
-----------------

One morning I came to my office before the coffee had kicked in and my
Linux partition was telling me it needed to update some software. When I
had a dedicated Linux hard drive, I always let the software update and
didn't suffer any ill effects. Well, at least until my SSD cratered.

This particular morning, I didn't notice the difference between the
words *update* and *upgrade*. I accepted all the “Are you sure” dialogs
while sipping my Sumatra and, before long my carefully crafted Ubuntu
10.04 VM was running Ubuntu 12.04. Exactly what I didn't want.

If you've gone to all the trouble to create a VM or dedicated partition
just for building Android, I suggest you back it up occasionally.
Fortunately, I had created a ZIP archive of this VM disk the day before,
so after about 30 minutes of reinstalling updates, I was running again.
Let my mistake be a lesson in the perils of system administration before
the caffeine has had its full effect.

SDK, repo, etc.
---------------

At this point, you're ready to start getting code from the Android Open
Source Project. While you don't need the Android SDK or NDK to build the
platform, there are parts of the tools that are useful. In particular,
the ***adb*** daemon is used quite often to communicate with your debug
target. You can build adb from the AOSP sources with ‘make sdk' or you
can download the SDK from the [Android developer
site](/Users/ecloninger/Desktop/developer.android.com).

Now you are ready to start following the instructions on the AOSP
[source access
site](http://source.android.com/source/initializing.html). Follow the
instructions carefully as there are things that will trip you up. For
example, you may want to sync and work with a specific label of the
Android sources during the course of your development. If you don't
specify this on the ‘repo sync' line, you will be on the master branch.
When you first configure your development environment, provide the –b
switch to repo to ensure that you are working in the branch you want.

Also, you will certainly want to turn on the CCACHE feature. In fact, if
you have a spare solid state drive lying around, you can speed things up
by pointing the CCACHE onto a SSD.

And now for something completely different...
---------------------------------------------

We all have our quirks. My dog, for example, loves kids but he hates
joggers. Personally, I loathe the sound scheme in Ubuntu. I know the
nice people at Canonical don't mean for the sounds to be annoying, but
they just are.

Sounds like the ethereal tribal startup can be disabled through the
preferences. But there is one sound that I cannot avoid. That sound is
the drumbeat that greets me when I login. Perhaps you have an
auto-login, but I do not. The first thing I hear each morning is that
drumbeat coming out of my vintage Bose 501's. Needless to say, I'm not
prepared for it. At least not until the coffee kicks in.

I searched the answer for this for some time and then realized that I
was searching for the wrong thing. There is the startup sound and there
is the login sound. This is the login sound. There are a number of ways
to get rid of this abominable sound, but the best documentation appears
to be described in [this AskUbuntu
post](http://askubuntu.com/questions/24946/how-do-i-disable-the-drum-beat-sound-on-the-login-screen)
on StackExchange. The Login Screen preference panel did the trick for
me.

On to other things
------------------

At this point, presumably you want to build the Android platform for
creating an emulator image or flashable ROM. In a future article, I'll
go into tips on that with various reference devices. We'll also start
going into ways that you can improve the quality, reliability, and
performance of your system builds with static analysis.

Did you have any challenges getting your build environment set up? Share
them in the comments and let others learn from your experiences. If you
are the twittering type, follow me at
~~[@kloceric](http://twitter.com/kloceric)~~
[@ericc](http://twitter.com/ericc) and give me a shout.

------------------------------------------------------------------------

**[About the Author:]{}**

Eric got his start in electronics by disassembling radios at age 10 and
putting them back together. Before he could legally drive, he was
installing stereos in his friends' pickups. From there came obsessions
with kites, basketball, girls, computers, tools, guitars, baseball,
woodworking, photography, golf, and kayaking with varying degrees of
success. Eric lives where there are few trees, little water, and no
mountains, but he dreams...
