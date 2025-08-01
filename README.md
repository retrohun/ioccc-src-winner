# The International Obfuscated C Code Contest


> **Obfuscate** | _verb_ [with object]
>
> render obscure, unclear, or unintelligible: the spelling changes will deform
some familiar words and obfuscate their etymological origins.
>
>    - bewilder (someone): it is more likely to obfuscate people than enlighten them.
>
>    **obfuscatory** | _adjective_
>
>   **ORIGIN**
>
>   late Middle English (as adjective): from late Latin **obfuscat-**
>   'darkened', from the verb **obfuscare**, based on Latin **fuscus** 'dark'.


> **Obfuscation** | _noun_
>
> the action of making something obscure, unclear, or unintelligible: when
confronted with sharp questions they resort to obfuscation | ministers put up
mealy-mouthed denials and obfuscations.
>
>   **ORIGIN**
>
>    late Middle English: from late Latin **obfuscatio(n-)**, from **obfuscare**
>    'to darken or obscure' (see *obfuscate*).

The official IOCCC website is [www.ioccc.org](https://www.ioccc.org).


## How it was started:

**It was a dark and stormy night...**

OK, let's go back to 1984: one day (1984 March 23 to be exact), Larry Bassel
and I (Landon Curt Noll) were working for National Semiconductor's Genix porting
group, and we were both in our offices trying to fix some very broken code.

Larry had been trying to fix a bug in
the old Bourne shell
[sh&lpar;1&rpar;](https://github.com/dank101/4.2BSD/tree/708b3890ac0c2f034f2840b5ee9125b3c83a05bc/bin/sh)
(C code `#define`d to death to sort of look like Algol),
and I had been working on the
[finger&lpar;1&rpar;](https://github.com/dank101/4.2BSD/blob/708b3890ac0c2f034f2840b5ee9125b3c83a05bc/ucb/finger.c) command
along with its associated
[fingerd&lpar;8&rpar;](https://github.com/dank101/4.3BSD-Reno/tree/master/libexec/fingerd) daemon
from early 4BSD as used by the GENIX operating system for the NS SYS16 system.

**BTW**: The above links to BSD code are only approximations of the BSD code that was being used in the GENIX operating system for the NS SYS16 system.  For example, the source code reference to `fingerd(8)` is well past the 1984 March 23 date.

If this is what could result from what some people claim is reasonable
programming practice, then to what depths might quality sink if people really
tried to write poor code?

I, Landon Curt Noll, put that question to the USENET news groups `net.lang.c`
and `net.unix-wizards` in the form of a contest.  I selected a form similar to the
[Bulwer-Lytton Contest](https://www.bulwer-lytton.com) that asks people to create the
worst opening line to a novel (that contest in turn was inspired by disgust over
[a novel](https://en.wikipedia.org/wiki/Paul_Clifford) by [Edward
Bulwer-Lytton](https://en.wikipedia.org/wiki/Edward_Bulwer-Lytton) that opened
with the line *"It was a dark and stormy night."*). The rules were simple: write,
in 512 bytes or less, the worst complete C program.

Thru the contest I have tried to instill two things in people.  First is a
disgust for poor coding style.  Second was the notion of just how much utility
is lost when a program is written in an unstructured fashion.  IOCCC winning entries
help do this by what I call *satirical programming*.  To see why, observe one of
the definitions of *satire*:

>       Keen or energetic activity of the mind used for the purpose
>       of exposing and discrediting vice or folly.

The authors of the winning entries placed a great deal of thought into their
programs.  These programs in turn exposed and discredited what I considered to
be the programmer's equivalent of "*vice or folly*".

There were two unexpected benefits that came from IOCCC winning entries.  First
was an educational value to the programs.  To understand these C programs is to
understand subtle points of the C programming language.  The second benefit is
the entertainment value, which should become evident as you read further!


## Suggestions on how to understand the winning entries:

You are strongly urged to try to determine what each program will produce by
visual inspection.  Often this is an impossible task, but the difficulty that
you encounter should give you more appreciation for the entry.

If you have the energy to type in the text, or if you have access to a machine
readable version of these programs, you should next consider some preprocessing
such as:

``` <!---sh-->
    sed -e '/^#.*include/d' prog.c | cc -E
```

This strips away comments and expands the program's macros without having things
such as includes and macros cluttering up the output.  If the entry requires or
suggests the use of compile line options (such as `-Dindex=strchr`) they should
be added after the `-E` flag. See the entry index.html and/or Makefile. For the
Makefiles look at the variable `CDEFINE`.

The next stage towards understanding is to use a C beautifier or C indenting
program on the source.  Be warned that a number of these entries are so twisted
that such tools may abort or become very confused, sometimes even preventing
them from compiling.  You may need to help out by doing some initial formatting
with an editor.  You might also try renaming variables and labels to give more
meaningful names. Be aware, however, that some entries will not work if you
rename variables, even if you manage to determine where exactly they are (to say
nothing of the fact that the name might be elsewhere in the code).

Now try linting the program.  You may be surprised at how little lint complains
about these programs.  Pay careful attention to messages about unused variables,
wrong types, pointer conversions, etc.  But be careful, some lints produce
incorrect error messages or even abort (or even crash)!  Your lint may detect
syntax errors in the source.  See the next paragraph for suggestions on how to
deal with this.

When you get to the stage where you are ready to compile the program, examine
the compilation comments above each entry.  A simple `#define` (or `-D` at the
compiler) or edit may be required due to differing semantics between operating
systems. If you are able to successfully compile the program, experiment with it
by giving it different arguments or input.  You may also use the Makefile
provided to compile the program. Look at the `To build` and `Try` sections at
the top of the index.html for each entry as well.  Keep in mind that C compilers
often have bugs, or features which result the program failing to compile.  You
may have to do some syntax changing as we did to get old programs to compile on
strict ANSI C compilers.

Last, read the judges' remarks (and sometimes spoilers) on the program.  Hints for `foo.c` are
given in `index.html`.  Often they will contain suggested arguments or
recommended data to use. Authors also sometimes provide obfuscation information
about their entries.

If you do gain some understanding of how a program works, go back to the source
and re-examine it using some of the techniques outlined above.  See if you can
convince yourself of why the program does what it does.


## Regarding the source archive:

Each subdirectory contains all the entries for a single year.  Often the file
names match one of the last names of the author.  Judges' hints and/or comments
are given in each entry's `index.html` file under the section `Judges' remarks`.

You may need to tweak the Makefile to get everything to compile correctly.  Read
the index.html files for suggestions.

The rules for a given year are given in the file named `rules.txt`.  Each
archive contains a copy of the rules for the upcoming contest.

The [years.html](years.html) web page the official archive of the winning entries.


## Regarding the distribution of sources:

All contest results are in the public domain.  We do ask that you observe the
following request:

You may share these files with others, but please do not prevent them of
doing the same.  If some of these files and/or contest entries are
published in printed form, or if you use them in a business or classroom
setting, please let us know.  We ask that you drop a line to the
judges' email box.  See [contact.html](contact.html) for instructions on
how to send us a message.


## Some final things to remember:

While the idea for the contests has remained the same through the years, the
contest rules and guidelines vary.  What was novel one year may be considered
common the next.  The categories for awards differ because they are determined
after the judges examine all of the entries.

The judges' hints assume that the program resides in a file with the same
username as the author or `prog.c` (depending on the year).  Where there is more
than one author, the first named author is used. If an author wins more than
one entry _per year_ there are different formats used but the first entry has 1
appended, the second has 2 etc. In some years it was `.1` and `.2` instead.

Some C compilers are unable to compile some of these programs.  The judges tried
to select programs that were widely portable and compilable, but did not always
succeed.  As of 1990, an entry may use both `K&R` and ANSI C compilers.
Makefiles for both types of C compilers are used.  See the contest rules for
details.

You are strongly encouraged to read the new contest rules before sending any
entries.  The rules, and sometimes the contest email address itself, change from
time to time.  A valid entry one year may be rejected in a later year due to
changes in the rules.  See [news.html](news.html) for up to
date information on how to enter.

Last, **PLEASE DO *NOT*** code in the style of these programs! It is hoped that you will
gain an understanding that poor style destroys an otherwise correct program.
Real programmers don't write obfuscated programs, unless they are submitting a
contest entry!  :-)

Happy pondering!


## About the IOCCC GitHub repository

The [official IOCCC entries GitHub repository](https://github.com/ioccc-src/winner) are the
official entries that won the **International Obfuscated C Code Contest (IOCCC)**, the
Internet's oldest ongoing contest.

**IMPORTANT NOTE**: See [contact.html](contact.html) for up to date contact details
as well as details on how to provide fixes to any of the entries.
See also the [IOCCC FAQ](faq.html) for additional information on the IOCCC.


<!--

    Copyright © 1984-2025 by Landon Curt Noll. All Rights Reserved.

    You are free to share and adapt this file under the terms of this license:

        Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)

    For more information, see:

        https://creativecommons.org/licenses/by-sa/4.0/

-->
