---
category : blog
tags : [MOTODEV, Android]
title: Get to the bottom of memory errors with MOTODEV Studio
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

### Episode IV - A New Hope

One of the promises of Java, circa 1996, was that memory problems would
be solved by a garbage collector that was smarter than we were. While
garbage collection does solve many of the problems that made our lives
more difficult, it's no silver bullet that slays the beast. Memory
problems still exist and the opaque nature of Java memory actually makes
the debugging process more difficult. MOTODEV Studio helps alleviate
some of this difficulty by including a common memory analysis tool
called MAT.

MAT, or the "Memory Analyzer Tool", is [an Eclipse
project](http://www.eclipse.org/projects/project_summary.php?projectid=tools.mat)
that was developed in open-source by committers from IBM and SAP. MAT
reads the output of the [Java standard HPROF
command](http://java.sun.com/developer/technicalArticles/Programming/HPROF.html)
and displays the results in a manner that can be interpreted by humans.

The MOTODEV Studio team made some adjustments to MAT to make it easier
for Android developers to use, but most of this functionality comes from
the existing MAT plugins or Google's ADT plugin. The [MAT
wiki](http://wiki.eclipse.org/index.php/MemoryAnalyzer) has some basic
FAQs, but the [best article I've
found](http://www.sdn.sap.com/irj/scn/weblogs?blog=/pub/wlg/6856) on how
to review and understand the results is available on SAP's web site.

### Start at the start

The following steps describe one way to perform this task. There are
many ways to get work done with Eclipse, so if this doesn't match your
own workflow, you should be able to adapt these instructions to meet
your needs. I suggest you run through these instructions once with your
app at a stable state such as in the main activity. Read the results and
try to gain an understanding of what objects are created by the OS and
base classes. HPROF generates so much information that you need to learn
the skill of spotting the *needle in the haystack*. Otherwise you could
waste time looking at results that aren't particularly interesting or
useful. Keep in mind that HPROF takes a snapshot of the current
conditions and doesn't monitor the execution over time, you may need to
run these steps multiple times to identify problems that arise.

1.  Launch an Android Virtual Device (AVD) using the MOTODEV Studio
    *Device Management* view, shown below. You can use MAT and MOTODEV
    Studio with an emulator or a real device, but if you are using your
    personal phone for debugging, you should always make sure you have
    your data backed up. From this point on, I will use the term
    "device", which could be either a physical handset or an emulator.

    ![1-msd_devmgmt.png](/assets/images/{{ page.id }}/original(1).jpg)

2.  Right click on the project containing your app in the Package
    Explorer and choose *Run As-&gt;Android Application using MOTODEV
    Studio*. This step copies your app to the device and launches it.
3.  Exercise the UI on your app to the point where you suspect the leaks
    are occurring. Remember the snapshot nature of HPROF, so you want to
    collect the data as close as you can to the problem spot.
4.  Switch to the *Device Management* view, right click on the device
    that you are testing, and choose *Analyze Memory with MAT* as shown
    below

    ![2-devmgmt_popup_mat.png](/assets/images/{{ page.id }}/original(2).jpg)

5.  Next, select the app that you wish to observe and press *Finish*

    ![3-analyze_dialog.png](/assets/images/{{ page.id }}/original(3).jpg)

6.  At this point, HPROF goes off and performs its analysis. This
    process will take about 15 seconds, depending on the speed of the
    computer and the load on the device. While you could put this dialog
    in the background, it will probably be finished before you have an
    opportunity to do so.

    ![4-mat_progress.png](/assets/images/{{ page.id }}/original(4).jpg)

7.  When HPROF is complete, it transfers a file to your computer. This
    file is stored in the directory referenced by the TEMP environment
    variable. After it is stored, you will be shown the dialog below,
    asking how to process the results. Since we're interested in finding
    leaks, choose the first option.

    ![5-MAT_getting_started.png](/assets/images/{{ page.id }}/original(5).jpg)

8.  The chart shown below is an example of what you will see from MAT.
    It has identifies several likely culprits as causing the leaks we
    see. The chart has links that allow you to drill deeper into each of
    the problem areas and allow you to explore individual data elements.

    ![6-mat_chart.png](/assets/images/{{ page.id }}/original(6).jpg)

    As I mentioned at the outset, there will be some false positives in
    the results and it helps to know which ones are caused by the
    framework and which come from your code. In my case, the problem was
    actually the second candidate, as shown below.

    ![7-leak_found.png](/assets/images/{{ page.id }}/original(7).jpg)

9.  There are ways to drill down into the results and actually see the
    contents of the memory that was allocated. From the toolbar at the
    top of the MAT results, click on the far-right icon, with a picture
    of an Android on it. This will filter the results for your
    application and allow you to inspect the memory in more detail.

    ![8-mat_chart_toolbar.png](/assets/images/{{ page.id }}/original(8).jpg)

10. If you find you want to go over the results of a previous execution,
    you can do so at any time. Just use *File-&gt;Open* to locate and
    open the file with the .hprof suffix in your TEMP folder. This will
    cause the analyzer to run again with the results that you previously
    captured.

### The Good, the Bad, the Ugly (and the Workaround)

As useful as HPROF and MAT are, this is an inexact method. HPROF only
works at a specific moment in time, not across the lifetime of your
application. You could run HPROF twice in quick succession and get
wildly different results. The randomness of the Garbage Collector makes
finding the moment where problems crop up to be non-deterministic. One
trick I've seen developers use is to put sleep() statements into the
code at specific points. Once they hit the sleep statement, they have
time to switch over to MOTODEV Studio long enough to trigger the data
capture.

### The right tool for the job

MAT is a useful tool and can be an essential part of a diagnostic
workflow. It takes the input from HPROF and displays it in a way that
you can track memory allocation issues. It doesn't track API usage and
it won't tell you which processes are using too much of the CPU. But,
used properly, it can help you create a faster and more efficient
application.

Good luck with your mobile applications and thank you for using MOTODEV
Studio.

Eric
