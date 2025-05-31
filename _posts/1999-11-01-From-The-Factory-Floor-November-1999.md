---
category : blog
tags : [CodeWarrior, PalmOS, i18n]
title: From The Factory Floor November 1999
hidden: true
---

**NOTE:** This blog post was originally hosted on the [MacTech web
site](http://www.mactech.com/). I've made every attempt to preserve this
post with their original content. Many web links are no longer valid, so
they have been removed and replaced with *emphasized* text.

From The Factory Floor November 1999
------------------------------------

    Volume Number: 15 (1999)
    Issue Number: 10
    Column Tag: From the Factory Floor

A Palm Update, Part 2
=====================

*by Eric Cloninger and Dave Mark*

*Web apps with Lasso and FileMaker Pro*
---------------------------------------

Last month's Factory Floor got us back in sync with the Palm development
universe. We checked out some of the new features in the upcoming Palm
OS SDK and CodeWarrior for Palm OS releases. This month, Eric Cloninger
will take us through the process of developing an internationalized Palm
OS application.

Eric Cloninger is the product manager and technical lead for CodeWarrior
for Palm OS. When he isn't working on CodeWarrior, he spends way too
much time on his research project - how to rid the world of the
affliction known as the 'designated hitter rule'. He can be reached at
ericc@metrowerks.com.

Dave: Tell me about the application you'll be taking us through.

Eric: This month, I'm going to go through an application I wrote using
CodeWarrior for Palm OS Release 6 (available in October 1999). The
application, called Base X, takes advantage of several new features that
Palm has added to the OS, the SDK and the tools.

Since our goal is to make an international Palm OS application, I chose
an example that is simple to describe but is also useful in a real-world
sense. This example, called Base X, provides four edit fields that
display the same 4 byte value as decimal, hexadecimal, octal, and
binary. In addition to the user interface elements on the main form,
Base X has a menu bar, an alert and an info string, all of which have
been localized from English to German, French, and Japanese.

I started by creating a new project from the "Palm OS C App" stationery
project. CodeWarrior for Palm OS provides stationery projects for Palm
OS 3.1 (for the original Pilot, PalmPilot, Palm III, and Palm V), Palm
OS 3.2 (for the wireless Palm VII), as well as Japanese examples.

I renamed the resource file to BaseX_english.rsrc and opened it with
Constructor for Palm OS. Constructor for Palm OS is very similar to
Constructor for PowerPlant - you can create menus, menu bars, pictures,
icons, etc. In fact, all of these elements use standard MacOS resource
types, so you can edit them with ResEdit or Resourcerer. The thing that
is unique about Constructor for Palm OS is that it lets you create the
Palm-specific resource types that define how forms look. Figure 1 is
from Constructor for Palm OS - it shows the main form for Base X as it
appears in the English resource file.

{{ page.id }}

![](/assets/images/{{ page.id }}/fig01.gif)

***Figure 1.** Constructor for Palm OS editing a Palm OS form.*

After I created the user interface, I moved the resources that are
language neutral into a separate resource file called
BaseX_Common.rsrc. Then, I moved the English resource file into a
directory named "English" and duplicated it for German, French, and
Japanese and renamed the files appropriately.

In my CodeWarrior project, I created four targets, one each for English,
German, French, and Japanese. Next, I added the common resources to all
four targets and the language-specific resources to each of the language
targets. At this point, for each target I have a source file, a common
resource file, and a language-specific resource file. I have not yet
written any code to operate my user interface, but the starter
application has enough code in it to display my form.

Next came the localization part. First, I browsed to
babelfish.altavista.com and tried their web interface. The engine that
provides the translation on the web page is available as a commercial
product called SYSTRAN. I used the web interface to convert my strings,
but I found that the translations weren't quite right and it doesn't
translate to Japanese. Since Metrowerks has offices in many parts of the
world, I asked employees to do the translations instead. These employees
aren't in our Austin office and they didn't have immediate access to the
Palm tools, so I had to find a way to get the strings to them easily.

Using the CodeWarrior IDE, I created an empty target in my project
called "DeRez". Then I added the English resource file to that target
and changed the settings for the target to use the 68K linker (to
activate preference panels - no linking occurred in this target). Next,
I changed the "File Mappings" panel so that files of type "PLob" (Palm
OS Constructor files) are compiled with the "Rez" compiler. Finally, I
entered a file called PalmTypes.r for the Prefix file in the "Rez"
panel. I saved these settings and returned to the "Files" view of the
project window.

From the files view, I clicked on BaseX_english.rsrc and held the mouse
button for a second until the pop-up menu appeared with an item called
"Disassemble". The Rez compiler will disassemble resource files into .r
files if it has type definitions for the resources, which is what
PalmTypes.r provides. The result of the disassembly is a new text file
that I sent to my colleagues. PalmTypes.r is available from Palms'
developer web site, by the way.

I knew it would be several thousand Swatch beats before I got my replies
back, so I decided to jump into the programming task.

Base X has four edit fields into which the user can enter text for
decimal, hexadecimal, octal and binary numbers. When the user enters a
character, Base X sees if the character is valid for the field with the
edit focus and converts all the fields if it is. The code for all of
BaseX is too long to include in this article, so I've put it on
Metrowerks web site at the address shown at the end of this article. The
code segment below is the part that converts strings from one base to
another and is where I had to use the Palm OS internationalization
manager.

``` {width="50"}
  // This struct describes how to convert strings
  // into different bases. It’s OK for this to be
  // single-byte chars because it’s never shown.
  struct {
   char *digits;
   char multiplier;
  } cvtTable[4] = {
   {“0123456789”, 10},
   {“0123456789abcdef”, 16},
   {“01234567”, 8},
   {“01”, 2}
  };

  // Convert a string to a number using base ‘x’
  ULong ToLong(WChar *str, short table_index) {
   unsigned long accum = 0;
   WChar ch = 0;
   short str_index = 0;
   short accum_index = 0;

   // Go through the input string, pull each
   // character off and find the index of that
   // character in the table. That index is
   // the amount to add to the accumulator.
   while (str_index < EDIT_SIZE) {
    str_index += TxtGlueGetNextChar(
       (Char *) str, str_index, &ch);
    if (ch)
    {
     accum_index = 0;
     while ((accum_index <
       StrLen(cvtTable[table_index].digits)) &&
       (WChar)
       cvtTable[table_index].digits[accum_index]
       != ch)
      accum_index++;

     accum *= cvtTable[table_index].multiplier;
     accum += accum_index;
    }
   }

   return accum;
  }

  // Creates a string of base ‘x’ from a long value
  void ToString(ULong value, WChar *str, short table_index)
  {
   short masked_value;
   char temp_str[EDIT_SIZE];
   char *p = temp_str;
   short str_index = 0;

   while (value > 0) {
    masked_value = value %
        cvtTable[table_index].multiplier;
    *p++ = cvtTable[table_index].digits[masked_value];
    value /= cvtTable[table_index].multiplier;
   }

   // At this point, temp_str is in reverse order.
   // Create output by walking temp_str in reverse.
   str_index = 0;
   while ( — p >= temp_str)
    str_index += TxtGlueSetNextChar(
      (Char *) str, str_index, (WChar) *p);
  }



    
```

The functions prefixed by 'TxtGlue' are notable because they are
implemented through a library called PalmOSGlue.lib instead of the
A-trap mechanism used by most of the OS calls. These functions are safe
to use on devices running version 2.0 or later of the Palm OS,
regardless of whether it is a single-byte or multi-byte OS.

By the time I had the code working for English, my translations were
done. Thanks to Andreas Hommel, Christophe Escobar, and Shoji Ueda for
providing this service. Next came the task of getting the converted
resources into the project without re-entering the text myself.

Developers who work with localized applications are faced with the same
situation I found myself in - whether to keep localized resources in a
text file where the text can be modified easily, or to keep them as
resource files where their properties can be modified with a visual
editor. Either method is fine and CodeWarrior allows me to use resource
files or Rez files, so I chose to set up my project so that I could use
either method.

I created a new target called 'Rez French' cloned from the 'French'
target. I modified the settings so that the output file created by the
MacOS linker is different from the output file generated by the resource
file target. I also modified the 'Rez' panel to include the
'PalmTypes.r' file as the prefix file. Into this target, I added the
translated BaseX_french.r file. Then, I built the project.

The output from the linker and post linker was the translated Palm OS
application. An artifact of the build process is a file that also
contains all my application's resources before they were modified by the
post linker. I opened this intermediate file, called BaseXRsrc.tmp, with
ResEdit and copied all the resources except 'CODE' and 'DATA' into my
French resource file named BaseX_french.rsrc. At this point, I can
create a French Palm OS application using either the .r file that is
compiled by the Rez compiler or I can build the same application using
resources included from the Constructor file. Next, I duplicated this
work for the English, German, and Japanese targets.

After building my applications, I want to run them and see how they
work. I could download them individually to my Palm OS device over the
serial connection or I can use an extremely useful application called
the Palm OS Emulator, or POSE for short. POSE is an application that
contains a 68K emulator and it runs the Palm OS image that is in ROM.
You must own a Palm device to get the ROM image, which is downloaded
from your Palm device using an application called ROM Transfer.

Figure 2 shows the Palm OS Emulator running the Japanese version of
BaseX. Any Palm device is capable of displaying English, French, and
German applications. Japanese applications require a Japanese-enabled
ROM to display the text correctly.

![](/assets/images/{{ page.id }}/fig02.gif)

***Figure 2.** Palm OS Emulator and CodeWarrior debugger.*

Let's suppose that I decide I don't like the way one of the Japanese
screens looks - perhaps I want to change the text of the info string
shown in the about box. Instead of sending the text file to Tokyo where
it's early in the morning, I use Constructor for Palm OS to modify the
strings myself. Palm has modified Constructor to work with the Japanese
Language Kit. With the JLK installed on my Mac, I need only change the
selection for the "Palm OS Target" in Constructor to "Palm OS for Japan"
and then I can begin editing in Japanese.

Now, I open the editor for the string that I want to modify. I select
Japanese mode input by clicking on the blue triangle on the menu bar in
the upper right corner. As I type, the MacOS pops up a text entry window
that converts the 'sounds' I am typing into the correct characters, as
shown in Figure 3.

![](/assets/images/{{ page.id }}/fig03.gif)

![](/assets/images/{{ page.id }}/fig03.5.gif)

***Figure 3.** Constructors' string editor with Japanese text input.*

At this point, BaseX is completed. I hope that I've been able to show
that CodeWarrior for Palm OS and the Palm OS SDK provide a rich toolkit
for developers who want to or need to write international applications.
CodeWarrior Professional users who want to try out the Palm OS tools can
do so after October by visiting the Metrowerks web site and downloading
the tools.

Users who want to play with the BaseX project file or the application
can download the archive from the Metrowerks Palm OS web site listed at
the end of the article.

### Resources

-   *http://www.metrowerks.com/pdas/palm/extras*
    Metrowerks support site for Palm OS. Contains samples, FAQs, and
    utilities that aren’t on the CodeWarrior for Palm OS CD.
-   *http://babelfish.altavista.com*
    Provides web-based translation between various languages.
-   *http://www.palm.com/devzone*
    Palms' developer zone.
-   *http://ls.palm.com*
    A list server, maintained by Palm, where developers can ask
    questions about CodeWarrior, conduits, Palm OS programming, and
    other relevant topics.
