# Matplotlib ideas page

These are just starting points.  Your own ideas are most welcome.

Any questions or comments should be asked on
[matplotlib-devel list](https://mail.python.org/mailman/listinfo/matplotlib-devel).

---------

<a name="layout"></a>
## Layout manager

### Abstract

Matplotlib currently has a very simple method to layout the components
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
[Cassowary](http://constraints.cs.washington.edu/cassowary/) as implemented by
[kiwi](https://github.com/nucleic/kiwi).  Likely
this would be implemented in two phases: (1) the implementation of
existing layout abilities in terms of constraints, accessible through
a new API, and (2) exposing this new implementation through the
existing APIs.

### Open Source Development Experience

This project involves experience with Python and understanding of
linear constraint solvers.  Computer graphics concepts, particularly
text layout, would also be an asset.

<a name="documentation"></a>
## imshow performance
### Abstract


When images are rendered to the canvas, we use >4x the input size in
internal temporary arrays.  In some cases, with masked images or with
values out of the normalization range, this is un-avoidable, but in
many cases we could make do with only one full size intermediate copy. See
[#6952](https://github.com/matplotlib/matplotlib/issues/6952) for more details.

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Intermediate  | Python, | [@tacaswell][] |


### Technical details

One approach at this would be add some fast-paths through the existing code.

Extending the current interpolation system to include dedicated
(external) resampling code, for example datashader.
See
[bokeh/datashader#200](https://github.com/bokeh/datashader/pull/200)
for a proof of concept.

Folding something
like
[mpl-modest-image](https://github.com/ChrisBeaumont/mpl-modest-image)
directly into Matplotlib would also be in scope for this project.

### Open Source Development Experince

This task will be majority in Python and requires a working knowledge
of NumPy.  Domain experience with large images is a plus.

This work may require the ability to read and understand templated C++
and Matplotlib's binding of Agg to Python.

This work may require API design (for which domain expertise will be beneficial) and
interaction with external libraries.


## 2D color maps
### Abstract
All of the color mapping in Matplotlib is currently derived from
`ScalerMappable` which as the name suggests maps scalers from `R^1 ->
R^4` RGBA color space.  It is common to want to map a vector to
colors, for example to control the alpha based on a second value in a
scatter plot or to show the orientation of a field.

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Intermediate  | Python, | [@tacaswell][] |


### Technical details
This will require developing a significant new set of classes for
Matplotlib, possibly parallel to possibly extending the existing 1D
normalization and color mapping classes.  These  classes will need to be exposed to
users either as new API or extending the existing `ScalerMappable` API.

In addition, a 2D color bar would need to be implemented and suitable
color maps will need to be developed.

### Open Source Development Experience

Competence with Python and OO design is required.  Experience with the
current normalization and color mapping tools in Matplotlib would be
beneficial.


## International text handling

### Abstract

Matplotlib's existing text layout engine is appropriate only for
standard English-like left-to-right languages.  In order to open
Matplotlib usage up to a broader international audience, it would be
nice to wrap an existing internationalized text layout engine, such
as
[Pango](http://www.pango.org/)/[Harfbuzz](https://github.com/behdad/harfbuzz).

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Advanced | Python, C, low-level font details | [][] |

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
## Categorical Color

### Abstract

Categorical color support in Matplotlib consists of the user manually
mapping their categorical data to numbers and creating some hacks to plot
their data accordingly. To more broadly support categorical color, the tasks
may include:
1. Modifying the units and plots interface so that heatmaps and other `scalerMappables` support units.
2. Creating a CategoricalNorm and CategoricalCmap
3. Modifying legends to work with `scalermappbles`
4. Adding support for natively plotting arrays of colornames


| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Intermediate | Python, Data science | [@tacaswell][@story645] |

### Technical details

More broadly, this task will involve designing new user-friendly APIs
to more automatically deal with certain types of data.

### Open Source Development Experience

This work will be done in Python.



## Compound Artists

### Abstract

Many of Matplotlib's higher-level plotting methods, such as `hist`
(histogram), return lists of artists that it draws.  Unfortunately,
this loses the connection to the original data, or any relationships
that those artists may have.  We would like to refactor these methods
to return "compound artist" classes that would retain the original
data and have methods to further modify the artists as a whole in
meaningful ways.  In addition to improving the API for Matplotlib
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
## Sphinx extension for handling large galleries

### Abstract

Matplotlib's documentation has grown organically over time and is now
extreme large, disorganized, and still does not cover the full
functionality of the library.  Current best practices favor an
overview->filter->details on demand approach to UI, but unfortunately
this tool does not yet exist in the sphinx ecosystem that Matplotlib
uses to build the documentation.  This task would involve making
the documentation more user-accessible through some combination of:

1. Understanding how [sphinx-gallery](sphinx-gallery/sphinx-gallery) generates documentation

1. Building album support in [sphinx-gallery](sphinx-gallery/sphinx-gallery)

1. Sorting out what meta-data to capture and cross link, develop tools
   to do this automatically.

1. Begin reorganizing the
   Matplotlib [gallery](http://matplotlib.org/gallery.html)
   and [examples](http://matplotlib.org/examples/index.html) albums,
   develop documentation and guide lines on how to sort examples into albums



| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Medium | Python, Sphinx, javascript, html | [ @story645 @tacaswell |

### Technical details

This task will involve working closely with our sphinx-based
documentation production system and writing and adapting extensions.

Writing tools and scripts in Python to automate repetitive tasks will
also be required.

### Open Source Development Experience

A contributor to this project should have broad experience using
Matplotlib and Python.  A deep understanding of Sphinx, including
writing extensions, would also be a major asset.

<a name="text"></a>
# Contact

[matplotlib-devel list](https://mail.python.org/mailman/listinfo/matplotlib-devel)
