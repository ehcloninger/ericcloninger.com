---
category : blog
tags : [CodeWarrior, PalmOS]
title: From The Factory Floor March 1998
hidden: true
---
From The Factory Floor March 1998
---------------------------------

**NOTE:** This blog post was originally hosted on the [MacTech web
site](http://www.mactech.com/). I've made every attempt to preserve this
post with their original content. Many web links are no longer valid, so
they have been removed and replaced with *emphasized* text.

---

    Volume Number: 14 (1998)
    Issue Number: 3
    Column Tag: From The Factory Floor

### CodeWarrior for PalmPilot

*by Dave Mark, Eric Cloninger, and the Metrowerks PalmPilot development
team, ©1997 by Metrowerks, Inc., all rights reserved.*

This month, we're going to talk with Eric Cloninger and the rest of the
CodeWarrior for PalmPilot development team. In case you haven't seen
one, the PalmPilot is a handheld organizer that offers contact
management and calendaring functions, along with the ability to run 3rd
party applications. Metrowerks CodeWarrior for PalmPilot lets you use
your Macintosh to develop your own PalmPilot applications.

Eric Cloninger is the Engineering Manager for "Scribbly Things" at
Metrowerks. He can be reached at ericc@metrowerks.com. When he isn't
investigating the mysteries of software development, Eric is hard at
work playing with his four month old son, Elijah. In the few hours a
week left to his own devices, he can be found concocting recipes for
homebrewed beer, playing softball, or watching the Colorado Rockies blow
it in the ninth inning. At night, he dreams of tall mountains, blue
skies, and deep powder.

Andrew Southwick is the Constructor for PalmPilot developer. Andrew and
his evil twin, Werdna, can be encountered on Quake servers worldwide.

Mark Corry works on the Mac- and Windows-hosted debuggers for PalmPilot.
When he isn't tinkering with the Metrowerks Debugger, he's tinkering
with a '29 Model A Ford.

Honggang Zhang works on the Windows-hosted debuggers for PalmPilot,
Windows CE, Windows NT, Windows 95, and Java. Away from work, she's
applying her knowledge of chemistry to build the perfect carrot cake
recipe.

Alex Harper provided Quality Assurance for CodeWarrior for PalmPilot.
Although he has recently moved on to other projects at Metrowerks, he
can still be found lurking about the PalmPilot news groups and harassing
the CodeWarrior engineers to make sure his pet features make it into the
next release.

**Dave: Tell me about the PalmPilot architecture?**

**Eric:** The PalmPilot uses a Motorola 68328 chip. It's so similar to
the 680x0 chip used in the Macintosh that most developers won't notice
the difference. The device itself is roughly the size of a deck of
playing cards and fits in your shirt pocket (or it would if that stylish
CodeWarrior shirt you are wearing had pockets). It has a
pressure-sensitive display area that responds to a stylus as well as a
data entry area where the user enters characters.

Unlike the Newton, the PalmPilot doesn't try to interpret the users'
handwriting. The PalmPilot uses Graffiti -- a system of strokes that are
roughly equivalent to the block alphabet. It takes about an hour to
figure out the letters and numbers and a few more hours to learn the
special characters. There is also a virtual keyboard that pops up on
request for those obscure characters (like how to get a grave accent
character over an 'e').

Currently, the device has a black and white screen. Older models, the
1000 and 5000, came with either 128K or 512K of RAM. The newer models,
the Personal and Professional, contain either 512K or 1MB of RAM. The
Personal and Professional offer a TCP/IP stack and the Professional has
a built-in email client.

As an organizer, the PalmPilot is useful. It has a date book, address
book, to-do list, and memo pad in ROM. If that was all it was, it
wouldn't be any more interesting than something Radio Shack sells for
$39.95. The great thing about the PalmPilot is that a developer can
write an application for the device and upload it to the device quickly
and easily.

When the user connects the device to their Mac (or PC) with a special
serial cable and presses the "HotSync" button on the front panel, the
device synchronizes itself with the data on the host computer. The
mechanism that performs the synchronization is called a "conduit". Other
developers can take advantage of conduits to synchronize their own data.

Macintosh programmers will recognize the way things are done on the
Pilot. The 68328 chip (also called the "Dragonball") is segmented the
same way the 680x0 chips were. This may seem like a limitation, but it
doesn't affect most people because a really large Pilot application is
50K. "A-Traps" are used to make ROM calls just like the 68K Macs did.
The PalmPilot runs what is essentially a single-threaded, single-tasking
operating system. Does this sound familiar?

Some people describe the Pilot as a Macintosh, circa 1984. I understand
the analogy because the device is very similar to the original Macs, but
it doesn't give the designers the credit they deserve. The Palm
engineers did a great job building an OS that met their design criteria,
and, with CodeWarrior for PalmPilot, developers have great tools for
writing quality applications for the platform.

**Dave: What is the PalmPilot runtime model?**

**Eric:** Before we discuss the specifics of writing code for the
PalmPilot, it's important for potential developers to understand how the
device operates. The PalmPilot runs PalmOS. The PalmOS is designed to
run on many devices, although to date, the PalmPilot and the IBM WorkPad
are the only ones that use it. It's conceivable that the PalmOS could be
scaled to operate other consumer devices, such as a car navigation
system.

The device contains a CPU, some memory, a serial port, and the user
controls. That's pretty much it. There is no disk drive, keyboard,
printer port, etc. Further, the device never truly shuts off. When left
idle or "shut off" from the power button, the device simply switches
into a low power consumption mode, similar to "sleep" on a PowerBook.
The application that is running at that time is still running, it's just
sitting idle in its event loop. The next time the user hits a button,
the processor switches back to active mode, and the application receives
its next event.

Another major difference from the desktop world is that everything about
a PalmPilot application (data, code, resources, etc.) is always stored
in RAM. And since the RAM on device is static, changes from one program
launch to another remain in place. Desktop application developers, who
are used to the operating system loading a fresh copy of their
application from disk every time the application is run, will find this
to be a new experience. A problem with new PalmOS developers is to
unintentionally modify memory where program code resides. In fact, we
take advantage of this feature to create breakpoints for the debugger.
The PalmOS includes a set of APIs which isolates program code from
direct memory accesses, which helps to prevent these kinds of problems.

The only PalmPilot application development languages that are currently
available from Metrowerks are 68K assembly and C. Pascal is not
available because a Pascal SDK hasn't been built and Java(tm) byte codes
are not feasible because the device doesn't have enough memory or CPU
power to run a full Java virtual machine (Java fans don't despair, see
the discussion of Jump at the end of this article). Berardino Baratta
has made C++ support available with the most recent release of our
compiler (version 4, due in January), but with no library support.
Developers can use the language, but there are no built-in classes for
strings, streams, etc.

**Dave: What is the Pilot development model?**

**Eric:** A PalmPilot application consists of many resources. Some of
these resources are code, some of them are data, and some of them are
visual elements. Is this beginning to sound familiar?

A PalmPilot developer will edit a form that contains user interface
elements like buttons, lists, check boxes, etc. They can edit the form
with ResEdit templates or visually with Constructor for PalmPilot.
Constructor generates a header file that has the symbolic names and
values of all the forms and user interface elements.

The user application begins at PilotMain(). PilotMain() receives a
command, some command-specific data, and some flags describing how the
application was launched. Inside PilotMain(), the application repeatedly
calls EvtGetEvent(). As events are retrieved, they are dispatched by the
application to the system event handler, the menu event handler, the
application event handler, and the form event handler. The system, menu,
and form event handlers are part of the operating system. The
application event handler is a large 'switch' statement that handles
application events.

The following code snippet, adapted from the 'Starter' example project,
shows some PalmPilot event handling code.

      DWord PilotMain( Word cmd, Ptr cmdPBP, Word launchFlags)
      {
        if (cmd == sysAppLaunchCmdNormalLaunch) {
          FrmGotoForm(MainForm);
          AppEventLoop();
        }

        return 0;
      }

      Boolean MainFormHandleEvent(EventPtr eventP)
      {
        if (eventP->eType == frmOpenEvent) {
          FormPtr frmP = FrmGetActiveForm();
          FrmDrawForm(frmP);
          return true;
        }

        return false;
      }

      Boolean AppHandleEvent( EventPtr eventP)
      {
        if (eventP->eType == frmLoadEvent) {
          Word formId = eventP->data.frmLoad.formID;
          FormPtr frmP = FrmInitForm(formId);
          FrmSetActiveForm(frmP);
          if (formId == MainForm) {
            FrmSetEventHandler(frmP, MainFormHandleEvent);
            return true;
          }
        }


        return false;
      }

      void AppEventLoop(void)
      {
        Word error;
        EventType event;
        do {
          EvtGetEvent(&event, evtWaitForever);
          if (!SysHandleEvent(&event))
            if (! MenuHandleEvent(0, &event, &error))
              if (! AppHandleEvent(&event))
                FrmDispatchEvent(&event);
        } while (event.eType != appStopEvent);
      }

PalmPilot code is pretty easy to read. The PalmOS SDK designers did
developers a favor when they decided to prefix every OS function call
with a three or four letter code for the manager that implements the
function call and the header file that defines the call. For example,
EvtGetEvent() is in the Event Manager and is defined in Event.h.
FrmDispatchEvent() is in the Form Manager and is defined in Form.h.

**Dave: What are the differences between the Pilot event model and the
Mac event model?**

**Eric:** The event models are very similar. The events that the
application processes are similar to Mac events: events telling the
application to launch, events describing data entry, events describing
menu selection, and events describing "taps" (the PalmOS equivalent to
mouse clicks). There are events that are specific to the PalmOS, such as
the event telling the application that the device was reset. A Mac
programmer who has written event loop code will feel comfortable with
the PalmOS event model after going through the tutorials that are
included on the CodeWarrior for PalmPilot CD.

**Dave: Andrew, can you tell me about Constructor for PalmPilot?**

**Andrew:** Constructor for PalmPilot lets the user edit PalmPilot
program resources. The application should look familiar to anyone who
has used Constructor for PowerPlant. **Figure 1** shows the form window,
which is where most editing takes place.

![](/assets/images/{{ page.id }}/fig01.gif)

***Figure 1.** Constructor for PalmPilot "form" editor.*

In this editor, the user edits a form by dragging user interface
elements from the catalog onto the Layout Appearance area which mimics
the PalmPilot screen. The user can modify the properties of the elements
by double clicking on the element. When the user saves the form,
Constructor for PalmPilot will generate a header file with symbol names
for each of the elements in the form. Constructor for PalmPilot also has
an icon editor for editing the application icon, a bitmap editor for
editing images, and editors for menus, menu bars, strings, string lists,
and alerts.

**Dave: What differences are there between CodeWarrior Pro and
CodeWarrior for Pilot?**

**Eric:** CodeWarrior for PalmPilot is a much less complicated package
than CodeWarrior Pro. For PalmPilot, we use the same IDE, Debugger,
compiler, and linker that ship with Pro. We add a post linker called
PilotRez that takes the output from the linker and modifies it to
conform to the PalmOS application format. We also ship Constructor for
PalmPilot, which Andrew described above. That's pretty much it. We throw
in some documentation covering the API, some example projects, a
20-chapter tutorial, and a PalmOS cookbook. A full install of the
current version (Release 3) takes about 45 MB of disk space and
CodeWarrior for PalmPilot can be installed on the same Macintosh as
CodeWarrior Pro.

**Dave:** What about debugging?

**Eric:** Developers can debug their applications in several ways: on
the device itself, with a simulator on the Mac, or with an emulator on
Windows and Macintosh.

Mac programmers have an advantage over their counterparts on Windows
because the Macintosh version of the PalmOS SDK includes a PalmPilot
simulator that runs on 68K and PowerPC Macintoshes. The simulator is a
library that is linked against the user code to create a Macintosh
application that behaves like a PalmPilot. The application can be
debugged with the standard Macintosh debugger. There are a number of
diagnostic tools that are included with the simulator, so by the time
the developer is ready to deploy to the device, most of the debugging is
done.

**Mark:** The simulator library simulates conditions on the PalmPilot as
close as it can, but there will be bugs that won't show up until they
are executed on the device. For that situation. developers can debug
directly to the PalmPilot. The Metrowerks debugger has been adapted to
debug the PalmPilot via the serial port. The process of debugging is
essentially the same as debugging the simulator -- the developer can set
breakpoints, evaluate variables, step through code, etc. The process is
slower because each action requires data be sent across the serial
connection to the device.

**Eric:** Debugging to the PalmPilot is not that difficult -- I've spent
the last eight months debugging this way. There are some tricks that
I've learned along the way to make it easier, but a developer who knows
how to debug the Macintosh won't have any problems debugging the
PalmPilot.

**Honggang:** Windows developers who want to debug away from the device
also have options. There is an application that runs on Windows called
CoPilot. CoPilot is a PalmPilot emulator, derived from the old Amiga
emulator. Since it is an emulator, the user must acquire a copy of the
PalmOS ROM image from their PalmPilot. Once the user has a working
CoPilot, they may debug their application with the Metrowerks Debugger
or they can use CoPilot itself to debug their application. Several
versions of CoPilot are also available for the PowerMac.

**Alex:** Retrieving the PalmPilot ROM currently requires that you
actually have a PalmPilot device. Some users have questioned the
legality of retrieving a ROM image. It is not our place to comment on
the legal issues, but we can say that Palm has never objected to
developers who own a PalmPilot using a ROM image to help debug. 3Com has
indicated they are looking for ways to supply PalmPilot developers with
a debug-only ROM image.

**Dave: So, what applications have you written?**

Eric: My first PalmPilot application, Dot Dot Two, was three lines long
and it put the device in a mode where it is ready to debug. Since then,
I've been redesigning and writing a program I wrote for my PowerBook
that lets me score baseball and softball games (actually, it lets my
wife, Jackie, score my softball games). The PalmPilot is a great
data-gatherer and it will be perfect for this.

**Dave: You've told me the good news, what's the bad news?**

**Eric:** The bad news isn't that bad...

The conduit SDK, which we ship with CodeWarrior, is only available for
Microsoft Windows. Also, as of the most recent version, the Conduit SDK
can only be developed with Microsoft Visual C++ 4.1. No other version
will work. 3Com is currently working to update the Conduit SDK to Visual
C 5.0. Support for other Windows compilers, including CodeWarrior for
Windows, is planned for future releases. Mac developers who want to
write conduits currently have no opportunity to do so, although Palm may
offer a Mac Conduit SDK at a later date.

CodeWarrior for PalmPilot, just like the PalmOS itself, is an
ever-changing product. The IDE, Debugger, compiler, and linker are
derived from our Mac 68k tools, which have a history of stability.
Constructor for PalmPilot is a new product and it hasn't been released
in its final form yet. It's very usable, but we recognize that there are
a few more features to add and a few more bugs to kill. Constructor for
PalmPilot requires a little more care than Constructor for PowerPlant
but it's progressing nicely.

**Dave: What kind of resources are available for the aspiring Pilot
programmer?**

**Eric:** The new PalmPilot programmer has a wealth of information at
their fingertips. We ship the OS documentation, tutorials, and FAQs on
CodeWarrior for PalmPilot. These are a good start, but there is so much
more. From the very start, there was an active PalmPilot developer
community on the InterNet and the people in that community are willing
to share their knowledge, experience, and pain.

These are the places I'd start:

-   *http://www.massena.com* - This is Darrin Massena's PalmPilot
    programming site. Darrin is one of the people that make the platform
    successful. Before CodeWarrior was available, developers who wanted
    to write for the platform could use Darrin's 'pila' assembler and
    'pilrc' resource compiler to write their applications.
-   *news.massena.com* - Darrin Massena also runs a private news server.
    There are (at last count) six news groups on his server covering
    PalmPilot development, including one for CodeWarrior. Al Rincon and
    I patrol this group and answer questions as they arise.
-   *comp.sys.palmtops.pilot* & *alt.comp.sys.palmtops.pilot* - These
    are UseNet newsgroups for the PalmPilot. The 'alt' group was created
    and used extensively before the official 'comp' group could be
    created. Both are still in use today although most of the
    discussions do not pertain to software development.
-   *http://www.roadcoders.com* - RoadCoders is a site for developers of
    all mobile platforms: PalmPilot, Newton, Windows CE, etc.
-   *http://www.wademan.com* - Wade Hatler writes a number of
    programming FAQs.
-   *ttp://www.pilotgear.com/* - Pilot Gear is a commercial site where
    visitors can download freeware and shareware as well as purchase
    commercial applications, aluminum cases, custom styluses, and other
    PalmPilot goods. The site can be searched or browsed. It's very
    similar to the MIT hyperarchive of Mac shareware, only with a lot
    more polish.
-   *http://www.hewgill.com/pilot/index.html* - Greg Hewgill is another
    person whose contributions to the PalmPilot development community
    are numerous. Greg wrote CoPilot, the Windows-hosted PalmPilot
    emulator. He also wrote Jump, which compiles Java classes to 68K
    assembly.
-   *http://w3.teaser.fr/~mpollet/Zilot* - Zilot is a PowerMac port of
    the CoPilot emulator.
-   *http://www.yahoo.com* - There's a ton of stuff on Yahoo about
    Pilot. Roll up your pant legs and jump on in!
-   *http://www.metrowerks.com/* - This is more than just a shameless
    plug for Metrowerks. Developers who want to try out PalmPilot
    development should pull down the PalmPilot Lite tools from our web
    site. With the PalmPilot Lite package, people who are curious about
    CodeWarrior for PalmPilot can go through a tutorial, edit visual
    resources, edit code, build applications, and debug.
