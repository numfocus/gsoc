# Installation-test scripts: aggregation server, script enhancements

## Abstract

While Software Carpentry workshops gather a lot of feedback regarding
instructors' teaching, not much attention was drawn to students' computers and
their issues with installation of open source software used during workshops.

This project will focus on bringing a working server for aggregating of
Software Carpentry installation testing scripts with additional goal of
providing easier and better experience for students using the script itself.

## Features

The server should be able to aggregate incoming (via REST API) data from
installation testing script.

The server should allow Software Carpentry admins to view statistics from
uploaded data.

Installation script should upload diagnostic data consisting of:

* operating system information
* installed packages and their versions
* failed packages checks and error messanges

Additionally, the data may be matched against workshop
[reference package list](https://github.com/wking/swc-setup-installation-test/issues/2).

## Technical Details

The server will be implemented using [Django](https://www.djangoproject.com/)
and [SQLite](https://docs.djangoproject.com/en/1.7/ref/databases/) database
for lower footprint and easier maintainance (and because I feel comfortable
using them).

API, in a RESTful fashion, will be implemented using
[django-tastypie](http://tastypieapi.org/).

Rough database structure:

* system information table
    * operating system family (Windows / MacOSX / Linux / Other)
    * OS version (8.1 / 10.9 / Ubuntu 14.04 / ?)
    * Hardware architecture (x86_64 / x86 / Other)
* package checks table
    * package name
    * requested package version
    * found package version
    * failed check?
    * fail reason

Every entry in both tables will be additionally assigned a
[universally unique identifier](http://en.wikipedia.org/wiki/Universally_unique_identifier).
Every single UUID will correspond to one diagnostic data upload.

Additionally we might want to store workshop-specific data, ie.:

* track submissions from specific workshops
* track packages requested for specific workshop.

Enhancements to the installation testing script will use standard tools
available in Python Standard Library and on students' systems (like, for
example, `uname`).

I want to use Mozilla's
[Metrics-Graphics](https://github.com/mozilla/metrics-graphics) for charts and
graphs, because this JavaScript graphing seems actively developed.

## Schedule of Deliverables

A word on my availability:

In the month of June I have to prepare for exams. I'll make sure to be
spending at least 20hrs per week on this project.

My exams are not scheduled yet, but the timeslot for them is
**June 23rd - July 7th**. In this time I won't be able to work on the project.
However, most likely I'll be over with the exams by the end of June - in that
case I'll resume working on the project as soon as I pass everything.

First I want to start by editing the installation testing script. I want to
enhance its capabilities in collecting diagnostic data from the system.

Currently, diagnostic output from the script on my system looks like this:

```
==================
System information
==================
os.name            : posix
os.uname           : ('Linux', 'zenbook', '3.13.0-46-generic', '#79-Ubuntu SMP Tue Mar 10 20:06:50 UTC 2015', 'x86_64', 'x86_64')
platform           : linux2
platform+          : Linux-3.13.0-46-generic-x86_64-with-debian-jessie-sid
linux_distribution : ('debian', 'jessie/sid', '')
prefix             : /home/piotr/workspace/anaconda
exec_prefix        : /home/piotr/workspace/anaconda
executable         : /home/piotr/workspace/anaconda/bin/python
version_info       : sys.version_info(major=2, minor=7, micro=9, releaselevel='final', serial=0)
version            : 2.7.9 |Anaconda 2.1.0 (64-bit)| (default, Dec 15 2014, 10:33:51)
```

It's very close to what I suggest in the database layout, but it's not entirely
the same. To cover differences:

* for operating system family I'd use `platform.system()` instead of `os.name`
  or `platform.name()` (to avoid matching, for example, "posix" to "Linux")
* to discover exact system version I'd use `platform.linux_distribution()` or
  `platform.release()` unless a better way exists)
* to discover CPU architecture: `platform.processor()` instead of
  `platform.uname()`.

### May 25th - June 7th

Implement gathering of operating system diagnostic data. Start testing that
script on at least one MacOSX machine, couple Windows boxes, and as many Linux
boxes as possible.

### June 8th - June 21st

Continue testing. Implement sending diagnostic data in the installation testing
script. Change the database schema if required.

### June 22nd - July 5th

I'm unavailable due to exams.

### July 6th - July 19th

Implement the REST API. Provide good (100%) test coverage. Start working on
a front end for Software Carpentry admins. Most likely graphs, charts, and so
on will take the biggest amount of work at this point.

### July 20th - August 2nd

Finish up UI, probably have a round of UX testing with Software Carpentry admins.

### August 3rd - August 16th

Finish up automated testing and UX-testing. Write documentation.

### August 17th - August 21th 19:00 UTC

In case the project finishes up earlier, I want to spend my time working on
installation script (see
https://github.com/wking/swc-setup-installation-test/issues/2).

## Future works

I've been involved with Software Carpentry for almost a year now. I'm
a Software Carpentry instructor, Software Carpentry Foundation member and
I don't plan to leave.

## Open Source Development Experience

2010-2012: cooperation with
[Oregon State University Open Source Lab](http://osuosl.org/):
[Ganeti Web Manager](http://ganeti-webmgr.readthedocs.org/en/latest/) project
(during two GSoCs and one Google Code-In).

GSoC 2014: [Peer instruction](https://github.com/pbanaszkiewicz/pitt) project
for Mozilla Science Lab (and Software Carpentry).

Since January 2015: [Amy](https://github.com/swcarpentry/amy) for Software
Carpentry.

## Academic Experience

I'm studying Automatics Control and Robotics at
[AGH-UST](http://www.agh.edu.pl/en) in Krakow, Poland. I know understand quite
a bit of Mathematics, including optimization theory, control theory,
probability, and others. Additionally I've got to know many industrial
automatics systems (PLCs, robots, etc.), I'm also good at Matlab. I posses a
LabView certificate (CLAD).

## Why this project?

Because I liked it. :) And I know the resulting server will be quite useful
for Software Carpentry.
