# Title
Cython API For DyND

## Abstract
DyND is an array math library written in C++ and designed to improve on the functionality of NumPy.
Its implementation as a C++ library allows it to be used for vector math at the C++ level without any interaction with Python objects.
It is proposed that the Python wrapper for DyND be modified to provide an API for Cython extension modules so that array arithmetic operations can be used in Cython without the overhead of interacting with Python objects.

## Technical Details
Cython is language used to automatically generate C and C++ extension modules for Python.
It augments Python's syntax with optional explicit type declarations that can be used to make code more efficient on a localized basis.
Cython is commonly used for both accelerating computations that are difficult to perform efficiently in Python and for interfacing with existing libraries written in other languages via their C and C++ APIs.
Currently, Cython has good support for NumPy arrays and other array-like objects supporting Python's buffer protocol.
In Cython these kinds of objects can be declared as memory views.
They are required to have a fixed data type and a fixed number of dimensions.
They can be sliced and passed between functions in Cython without any calls to the Python C API, but any array arithmetic must be performed by explicitly looping through these arrays element-by-element.
NumPy arrays are usable within Cython, but the NumPy array object is inexorably tied to its Python API and operations on NumPy arrays cannot be performed easily without the Python API.

DyND, on the other hand, allows these sorts of arithmetic operations within C++ and without any dependency on operations involving python objects.
In addition, its design allows for better expression analysis and optimization at compile time.
Adding a Cython API for DyND will address the limitations that are currently a part of array arithmetic in Cython and make things like static expression analysis for arrays much easier to access when writing Python C extension modules.
Currently there are limited Cython wrappers in DyND, but they are not a part of the public API and only cover a portion of the features present.

## Schedule of Deliverables

### May 25 -  June 7
Modify the existing wrappers in DyND's python wrappers to make the C++ objects in DyND accessible to external Cython modules.
Make overloaded arithmetic and indexing operators properly handle exceptions.
**You need to accomplish this to mid-term.**

### June 8 - June 21
Add support for the overloaded assignment operator to the Cython API.
Overloading the assignment operator isn't currently supported in Cython, so this can be added by either adding the feature to Cython or by using Cython's support for user-specified C-names for functions.

**You need to accomplish this to mid-term.**

### June 22 - July 5
Provide externally available wrappers for types, arrfunc manipulation, math functions, and array iterators.

### July 6 - July 19
Make Python wrapper classes publicly available for Cython modules as well.
Make conversion routines to and from PEP 3118 compliant objects publically available as well.

### July 20 - August 2
Make and test wrappers for take and groupby operations.

### August 3 - August 16
Make conversions to and from Python functions and numpy gufuncs work properly for external modules.

### August 17 - August 21 19:00 UTC
Simplify API as much as possible.
Improve documentation.
Clean up code further.

## Future work
Future work will be focused on expanding the number of matrix and linear algebra operations that are available in DyND.
Once finished, the Cython API should also be kept up-to-date with the features that are finished in C++.

## Open Source Development Experience
I worked for over two years as a primary contributor to BYU's lab manuals for their new applied math emphasis.
These labs are available on GitHub at byuimpact/numerical_computing.
I have submitted relatively small patches to several projects (including NumPy, SciPy, and PyFFTW) and have recently added a Cython API for BLAS and LAPACK to scipy.
The Cython API for BLAS and LAPACK makes it so that extension modules generated using Cython can now use the BLAS and LAPACK routines included in SciPy without having to link against the original libraries.

## Academic Experience
I graduated from BYU in 2013 with an undergraduate degree in mathematics after attending for the 2009 and 2012 academic years.
I have been in the masters program here since August of 2013 and expect to be defending my thesis shortly before the GSOC program begins.
I am planning to begin the PhD program at BYU in the fall.
My research involves finite element analysis on spline curves.
My contribution there involves creating better refinement and evaluation techniques for certain classes of generalized spline curves.

## Why this project?
I'm interested in DyND primarily because of the problems it solves.
When I was first learning to interface with other programming languages from Python, I was amazed by Fortran's ability to perform static expression analysis and optimization of array operations.
Though it is cumbersome as a language, its support for array operations is incredible.
I was troubled by the fact that, as good as it is, NumPy can't do all that Fortran does.
NumPy's array object is inseparably connected to the Python C API.
This makes it hard to perform fast vector operations on small arrays inside loops and it makes it so that programmers have to do things like expression analysis and common subexpression elimination by hand when working with large arrays.
I searched around to see if C++ had any sort of numpy-like options and found that most of the main vector math libraries in C++ (Eigen, Armadillo, Blaze-lib) only support operations on arrays with, at most, 3 dimensions.
What would be ideal is a library that supports a variety of memory layouts and can still statically optimize code evaluated using those arrays.
Other C++ vector libraries that do that sort of thing (like blitz++) are no longer maintained. 

It was after reading about many of these other array libraries that I found DyND.
Its design is ideal since it allows for static expression analysis on n-dimensional arrays at compile time and fast operations with small arrays.
I am very happy with the idea that everything is a view, since the view vs. copy dichotomy tends to lead to trouble when working with numpy.
NumPy's gufunc machinery is a remarkable work in its own right and I'm impressed by the fact that DyND is developing its own ArrFuncs.
I have little experience with writing a library that does static analysis like this since most of my experience has been in numerical methods, but I want to contribute because I see the massive kinds of benefits it will have.
As I learn more about the mechanics of how to use DyND, it makes me all the more anxious to help.

Adding a Cython API for DyND will make it possible to write extension modules for Python without having to go through the trouble of manually looping through each portion of an array.
Using DyND for this also makes it so that operations on arrays in Cython can be optimized at compile time at the library level rather than being confined to one particular way of looping through an array.
Much of the Python scientific stack is written in Cython, and providing a Cython API for DyND will make it much easier for developers to use the features of DyND in extension modules for Python.
With a Cython API in place, developers will no longer have to implement array operations in C++ and then wrap them separately.
The Cython API will make it so that little or no new C++ code is required to perform array operations within Python extension modules.

## Appendix
Cython: https://github.com/cython/cython
DyND: https://github.com/libdynd/libdynd
DyND Python Wrappers: https://github.com/libdynd/dynd-python
Cython API for BLAS and LAPACK (Previous work providing related functionality): https://github.com/scipy/scipy/pull/4021
