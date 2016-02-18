# matplotlib ideas page

These are just starting points.  Your own ideas are most welcome.

Any questions or comments should be asked on
[matplotlib-devel list](https://mail.python.org/mailman/listinfo/matplotlib-devel).

## Table of contents

* [Layout manager](#layout)
* [Documentation improvements](#documentation)
* [International text handling](#text)
* [Categorical axes](#categorical)
* [New Mac OSX backent](#macosx)
* [Compound artists](#compound)
* [GUI Improvements](#gui)

---------

<a name="layout"></a>
## Layout manager

### Abstract

matplotlib currently has a very simple method to layout the components
of a figure (axes, titles, tick labels).  This unfortunately makes it
very common for text to run together or outside of the image.  A
proper constraint-based layout engine should be able to automatically
prevent such artists from running together, as well as solve other
layout difficulties such as twinned axes and colorbars.

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Advanced | Python, Constraint Solving | [@mdboom][] [@tacaswell][] |

### Technical details

This work will likely be based on an existing constraint-solving
library, such as
[Cassowary](http://constraints.cs.washington.edu/cassowary/).  Likely
this would be implemented in two phases: (1) the implementation of
existing layout abilities in terms of constraints, accessible through
a new API, and (2) exposing this new implementation through the
existing APIs.

### Open Source Development Experience

This project involves experience with Python and and understanding of
linear constraint solvers.  Computer graphics concepts, particularly
text layout, would also be an asset.

<a name="documentation"></a>
## Documentation improvements

### Abstract

matplotlib's documentation has grown organically over time and is now
disorganized and out-of-date in many areas.  This task would involve
making the documentation more user-accessible through some combination
of:

1. Reorganizing examples by keywords

1. Redoing our Sphinx usage to put a single docstring per page and
   linking to frequently repeated content

1. Modernizing narrative documentation to follow best practice

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Light | Python, Sphinx | [@mdboom][] [@tacaswell][] |

### Technical details

Beside just writing, this task will involve working closely with our
Sphinx-based documentation production system, possibly writing and
adapting extensions.  Writing tools and scripts in Python to automate
repetitive tasks will also be required.

### Open Source Development Experience

A contributor to this project will have to have broad experience using
matplotlib and Python and have good technical writing skills.  A deep
understanding of Sphinx, including writing extensions, would also be a
major asset.

<a name="text"></a>
## International text handling

### Abstract

matplotlib's existing text layout engine is appropriate only for
standard English-like left-to-right languages.  In order to open
matplotlib usage up to a broader international audience, it would be
nice to wrap an existing internationalized text layout engine, such as
[Pango](http://www.pango.org/)/[Harfbuzz](https://github.com/behdad/harfbuzz).

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Advanced | Python, C, low-level font details | [@mdboom][] |

### Technical details

In order to make this work possible, we first need to abstract out the
existing text layout engine so it can be interchanged with other
algorithms.

More details of this project are described
[here](https://github.com/matplotlib/matplotlib/blob/0037595096e3be3ec863f3e3008e73c469e2f2b8/doc/devel/MEP/MEP14.rst).
Some of the initial tasks in that MEP are already implemented in
[PR 5414](https://github.com/matplotlib/matplotlib/pull/5414).

### Open Source Development Experience

This task may involve writing Python wrappers for C libraries in the
Python/C API if the existing wrappers are found to be insufficient.

Deep understanding of or willingness to learn font technologies, the
Unicode Standard and text layout algorithms is required.

<a name="categorical"></a>
## Categorical axes

### Abstract

matplotlib 1.5 added direct support for plotting data frames.
However, there are still a few related tasks yet to be done.  An
important one is detecting when plotting categorical data
(i.e. enumerations) and updating the tick labels accordingly.

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Intermediate | Python, Pandas, Databases, Data science | [@tacaswell][] |

### Technical details

More broadly, this task will involve designed new user-friendly APIs
to more automatically deal with certain types of data.

### Open Source Development Experience

This work will be done in Python.  Pandas experience would be very helpful.

<a name="macosx"></a>
## New Mac OSX backend

### Abstract

The existing `macosx` backend performs rendering of all drawing
primitives.  Unfortunately, this means it often lags behind in terms
of the quality and speed of the rendering vs. the more commonly used
Agg backend.  We would like to implement a new `macosx` backend
(largely based on the existing one) where the rendering is performed
by Agg and this image buffer is simply copied to the screen.  This
would retain all of the advantages of the existing `macosx` backend --
that it has no external dependencies and the widgets appear "native"
-- but would render with the same quality and performance as the Agg
backends.

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Advanced | Python, Objective-C, Python/C API, Cocoa | [@mdboom][] |

### Open Source Development Experience

This will involve adapting an existing Python extension written in
Objective-C, and will require access to a Macintosh.

<a name="compound"></a>
## Compound Artists

### Abstract

Many of matplotlib's higher-level plotting methods, such as `hist`
(histogram), return lists of artists that it draws.  Unfortunately,
this loses the connection to the original data, or any relationships
that those artists may have.  We would like to refactor these methods
to return "compound artist" classes that would retain the original
data and have methods to further modify the artists as a whole in
meaningful ways.  In addition to improving the API for matplotlib
users, this opens up the possibility of better data exchange with
other tools.

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Intermediate | Python | [@tacaswell][] |

### Technical Details

Some of this work has already been begun in
[#3944](https://github.com/matplotlib/matplotlib/pull/3944).

### Open Source Development Experience

This work should be 100% in Python.  Understanding of duck-typing and
operator overloading in Python is a must.

<a name="gui"></a>
## GUI improvements

### Abstract

Many of matplotlib's GUI backends have existed for 10 years at this
point.  For this reason, they don't always handle modern technologies
very well.  This task would involve some combination of the following:

1. Improve support for HiDPI (Retina) displays

1. Add touch support (e.g. two-finger zooming etc.) through the web
   browser for mobile devices (e.g. iPad), and natively
   (e.g. Microsoft Surface).

1. Add support for scrollable figures (i.e. where the GUI window and
   the figure size are not the same)

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Intermediate | Python, Javascript, GUI frameworks | [@mdboom][] |

### Technical Details

One of the challenges of this task is that matplotlib supports so many
GUI frameworks, including Tk, Gtk, Qt, Wx and Cocoa.  This task will
need to keep features in parity across these frameworks wherever
possible.

### Open Source Development Experience

This work should be mostly in Python with some Javascript and
understanding of browser technologies.  Understanding of multiple GUI
frameworks a must.  Access to multiple operating systems (as many as
possible of Linux, Mac, Windows, iOS, and Android), is very helpful.

# Contact

[matplotlib-devel list](https://mail.python.org/mailman/listinfo/matplotlib-devel)
