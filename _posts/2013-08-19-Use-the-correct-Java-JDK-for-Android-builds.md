---
category : blog
tags : [Klocwork, Android, Java, Linux]
title: Use the correct Java JDK for Android builds
hidden: true
---
**NOTE:** This blog post was originally hosted on [Klocwork's Android
blog](http://blog.klocwork.com/android-development/). At some point
those posts may not continue to exist. I've made every attempt to
preserve the original content with only formatting changes to fit this
site.

Google released Android 4.3 (Jellybean) source to the AOSP repository a
few weeks ago. Whenever this happens, I sync my build box against the
AOSP sources. Then I spend some time going through the diffs and run a
full Klocwork scan against the code, just to make sure no new problems
surface.

I build several Android projects each week and I scan most with our
tools. Within certain ranges, I know about how long the builds take and
what the distribution of errors looks like. When I started the build for
4.3, I knew I could get on my bike, ride to the deli and grab a sandwich
and still have enough time for a peaceful lunch on the patio outside my
office.

Needless to say, I didn't get to enjoy my sandwich as the log files
showed the build process lasted less than 2 minutes. The culprit was the
build scripts are too smart—they check that I have the correct version
of Java installed.

Wait. I have the wrong version of Java installed? There's only one
version of Java that's allowed with AOSP and that is Sun (Oracle) Java
1.6. There's no way I would've replaced that. I checked java -version
and, sure enough, it was Java 1.7.0.21-b11. How did that get in there?

Truth is, there's no telling. I install and uninstall a lot of software
on my build machine and I'm also impatient when it comes to looking at
updates. Java 1.6 was still on the machine, it just wasn't on the path.
So, I used the [wonderful update-java
script](http://www.webupd8.org/2010/04/java-update-script-for-ubuntu-version.html)
that I mentioned in a previous post to set everything back to 1.6.

    wget http://webupd8.googlecode.com/files/update-java-0.5b
    chmod +x update-java-0.5b
    sudo ./update-java-0.5b
        

I restarted the script and devoted my attention to the
[muffuletta](http://en.wikipedia.org/wiki/Muffuletta) on my table. I
wasn't two bites into the sandwich when the console window scrolled up,
indicating it was finished. I went to the logs and discovered that the
$JAVA_HOME value in my .bashrc had changed. [I honestly have no idea
how this happened.]{style="font-size: 13px;"}

    # export JAVA_HOME=/usr/lib/jvm/java-6-oracle
    export JAVA_HOME=/usr/lib/jvm/java-7-oracle
        

Maybe I sleep-walk, sleep-copy, sleep-paste, and sleep-edit.

I did what every developer does: I looked for a [solution on
StackOverflow](http://stackoverflow.com/a/7335524/296758). From this
post, I whipped up a bash script that alerts me before anything else
happens. I put this in my .bashrc with a note, but I also have it in the
script I use to kick off an Android build.

    #!/bin/bash
      # Usage: checkjava.sh [java_version]
      #    if no parameter, script assumes 1.6 (for Android JDK)
      # Returns: 0 for successful match
      #          1 for no Java or wrong Java
      if [ $# -eq 0 ] ; then
        assume="1.6"
      else
        assume=$1
      fi
      typeval=`type -p java`
      if [[ -n $typeval ]] ; then
        _java=java
      elif [[ -n "$JAVA_HOME" ]] &amp;&amp; [[ -x "$JAVA_HOME/bin/java" ]];  then
        _java="$JAVA_HOME/bin/java"
      else
        echo "Warning: Java not on PATH"
        exit 1
      fi
      if [[ "$_java" ]]; then
        version=$("$_java" -version 2>&amp;1 | awk -F '"' '/version/ {print $2}')
        jver=${version:0:3}
        if [[ "$jver" != "$assume" ]]; then
        echo "Java version on PATH is $version"
        exit 1
        fi
      fi
      exit 0
        

Granted, the Android build system will detect this early on, but it
takes a couple minutes of figuring out the dependencies before it does
the work. I would rather know the moment I open a bash prompt that I may
need to give my JDK settings some attention.

------------------------------------------------------------------------

**About the Author:**

Eric got his start in electronics by disassembling radios at age 10 and
putting them back together. Before he could legally drive, he was
installing stereos in his friends' pickups. From there came obsessions
with kites, basketball, girls, computers, tools, guitars, baseball,
woodworking, photography, golf, and kayaking with varying degrees of
success. Eric lives where there are few trees, little water, and no
mountains, but he dreams...
