# GSoC 2016 Ideas Pages - DyND

* https://github.com/numfocus/gsoc
* http://libdynd.org/gsoc

## Table of Contents

* [String Type and Library](#string)
* [Serialization and Data Hashing](#serialization)
* [Hash Map Type](#hashmap)
* [Missing Data](#missing)
* [Robust String/Float Conversions](#stringfloat)

<a name="string"><a/>
## String Type and Library

### Abstract

Complete DyND's string type implementation and develop the supporting string library. Explore how string processing and NumPy-style array programming interact.

### Technical Details

DyND's string type and supporting library are incomplete. A project could involve completing the implementation of the basic string type, fleshing out the supporting string function API, connecting DyND strings to the Numba JIT, and exploring how string API choices interact with the array programming ideas and typical data ingest and analysis workflows.

* [DyND String Design Document](https://github.com/libdynd/libdynd/blob/master/docs/string-design.md)
* [DyND's Incomplete String Class Implementation](https://github.com/libdynd/libdynd/blob/master/include/dynd/types/string_type.hpp#L17)
* [Example String Functionality Pull Request](https://github.com/libdynd/libdynd/pull/910)
* [C\++Now Talk: Unicode In C\++](https://www.youtube.com/watch?v=MW884pluTw8)

### Open Source Development Experience

C\++ development experience, particularly with C\++11 or C\++14, is necessary. Familiarity with how strings work in a variety of other systems, Unicode, and implementation details of string algorithms will help as well. Additional useful experience is with array programming ideas from NumPy, Matlab, R, etc.

### First Steps

* [Get to know DyND](https://github.com/libdynd/libdynd/blob/master/docs/developer-guide.md)
* Get more deeply familiar with the string libraries of a variety of systems.
* Complete the DyND nd::string class according to the design, tweaking the design document with any additional improvements.

<a name="serialization"></a>
## Serialization and Data Hashing

### Abstract

Implement binary serialization for DyND array data, and corresponding data hashing that's useful for building Merkle DAG-based systems.

### Technical Details

DyND's nd::array object can store many forms of data in memory. Being able to generically serialize these objects into blobs of bytes for saving on disk or transporting across a network is needed for many use cases. DyND's design aims to use an obvious raw-style format for the serialized data, to be usable as a building block for more sophisticated systems.

Complementary to serialization is data hashing. In hash maps, fast hash functions are needed, while for data integrity and Merkle DAG applications, cryptographic hash functions are preferred. A functional difference between these systems is that for hash maps, you want elements that compare equal to hash equal even if they have different underlying representations (e.g. -0.0 and 0.0), whereas for a Merkle DAG the the hash of data should be that of its serialized bytes.

* [DyND Serialization Design Document](https://github.com/libdynd/libdynd/blob/master/docs/serialization-design.md)
* [DyND Merkle DAG Design Document (Use Case)](https://github.com/libdynd/libdynd/blob/master/docs/merkledag-design.md)
* [Issue About Hashing DyND Types](https://github.com/libdynd/libdynd/issues/117)

### Open Source Development Experience

C\++ development experience, particularly with C\++11 or C\++14, is necessary. Familiarity with serialization libraries in various languages, the design choices that systems like CBOR, protobuf, parquet, pickle, and other systems make, and data hashing are all valuable. Experience with file format design, distributed systems, or other related applications of serialization would be useful.

### First Steps

* [Get to know DyND](https://github.com/libdynd/libdynd/blob/master/docs/developer-guide.md)
* [Become familar with DyND's data model](https://nbviewer.jupyter.org/github/libdynd/libdynd/blob/master/docs/intro-01/HowDyNDViewsMemory.ipynb)
* Get to know [DyND callables](https://github.com/libdynd/libdynd/tree/master/include/dynd/callables)

A project tackling this functionality could begin with implementing the serialization function, getting familiar with DyND's data model and callable system in the process. Further work would be to explore how data hashing in DyND should be structured to satisfy the various use cases, and implementing as much of that as possible.


<a name="hashmap"></a>
## Hash Map Type

### Abstract

Implement an associative array type for DyND as a hash map.

### Technical Details

This idea overlaps slightly with [Serialization and Data Hashing](#serialization), but focuses on the implementation of a hash map type instead of the hashing its built on. The idea would be for the type to be named like `map[S, T]`, and be a hash table with keys of type `S` and values of type `T`.

Building the type requires exploration of potential interactions with the rest of DyND, likely requiring writing a design proposal and brainstorming with the DyND team to shake them all out. Hash map performance requires many specific design choices, getting this right will involve shaking out the tradeoffs between dynamic generality and code performance.

* [KLib's KHash](https://github.com/attractivechaos/klib/blob/master/khash.h)
* [Google SparseHash](https://github.com/sparsehash/sparsehash)
* [A Description of Python's dict](http://www.laurentluce.com/posts/python-dictionary-implementation/)
* [Blog Post about Hash Tables](http://www.reedbeta.com/blog/2015/01/12/data-oriented-hash-table/)

### Open Source Development Experience

C\++ development experience, particularly with C\++11 or C\++14, is necessary. An understanding of how high performance data structures work, particularly how they are tuned for cache locality, is important. Ideally have familiarity with all the possible choices in creating a hash table, and how they play out in practical implementations like std::unordered_map.

### First Steps

* [Get to know DyND](https://github.com/libdynd/libdynd/blob/master/docs/developer-guide.md)
* [Become familar with DyND's data model](https://nbviewer.jupyter.org/github/libdynd/libdynd/blob/master/docs/intro-01/HowDyNDViewsMemory.ipynb)
* Review literature on high performance hash tables.

<a name="missing"></a>
## Missing Data

### Abstract

Improve DyND's support for missing data via its `option[T]` type, targetting DyND's ability to be used by projects like [pandas](http://pandas.pydata.org/).

### Technical Details

Having a representation for missing data and ways to meaningfully manipulate them are crucial for data analytics applications. DyND has preliminary support for this, but it is far from complete.

One goal is to provide a way to represent a token across DyND's supported data types for interoperability with other systems that have such an NA/Null/None value. Another goal is to provide reasonable default calculation semantics for these missing values, using the needs of Pandas as the primary driver for design choices.

For computation with missing data, the likely default behaviors are to propagate when doing element-wise operations and ignore when doing reductions. When defining callables, these should work automatically or be easy to enable, while it should also be possible to override them for operations like boolean `and` and `or` which do not strictly propagate.

* [Pandas Missing Data Docs](http://pandas.pydata.org/pandas-docs/stable/missing_data.html)
* [Pandas-Dev Thread with some Pandas/DyND Discussion](https://mail.python.org/pipermail/pandas-dev/2015-December/000392.html)
* [R Missing Data FAQ](http://www.ats.ucla.edu/stat/r/faq/missing.htm)
* [NumPy Missing Data NEP](http://docs.scipy.org/doc/numpy-1.10.1/neps/missing-data.html)
* [NumPy Missing Data Debate Summary](http://www.numpy.org/NA-overview.html)
* NULL Bitmap [in MySQL](https://dev.mysql.com/doc/internals/en/null-bitmap.html) and [in PostGreSQL](http://www.postgresql.org/docs/8.0/static/storage-page-layout.html)

### Open Source Development Experience

C\++ development experience, particularly with C\++11 or C\++14, is necessary. Experience with applications making use of missing values, like with pandas or matplotlib is highly recommended.

### First Steps

* [Get to know DyND](https://github.com/libdynd/libdynd/blob/master/docs/developer-guide.md)
* [Become familar with DyND's data model](https://nbviewer.jupyter.org/github/libdynd/libdynd/blob/master/docs/intro-01/HowDyNDViewsMemory.ipynb)
* Dig into DyND's [option type](https://github.com/libdynd/libdynd/blob/master/include/dynd/types/option_type.hpp) and callable kernels like [is_na](https://github.com/libdynd/libdynd/blob/master/include/dynd/kernels/is_na_kernel.hpp), [assign_na](https://github.com/libdynd/libdynd/blob/master/include/dynd/kernels/assign_na_kernel.hpp), and [forward_na](https://github.com/libdynd/libdynd/blob/master/include/dynd/kernels/forward_na_kernel.hpp).

<a name="stringfloat"></a>
## Robust String/Float Conversions

### Abstract

Integrate robust conversions of float point values to/from strings.

### Technical Details

As part of being a library to facilitate interoperability of data, DyND needs to include good quality routines for conversion of floating point values to and from strings. This basic functionality is used by I/O such as for JSON and CSV, and in displaying output for the user or interpreting user input.

A robust approach for converting floating point values to strings is to find the shortest decimal representation that rounds to the value. The netlib library dtoa, and variations like the double-conversion library used by V8 accomplish this.

Converting decimal strings to floating point values is similarly fraught with difficulty. It may be nice to get this right too, and validate by testing all float32 and randomly testing float64. Another way to allow robust specification of floating point values is to add support for hexadecimal float notation, as supported by dtoa.

* [The netlib dtoa library](http://www.netlib.org/fp/dtoa.c)
* [Python's adaptation of dtoa](https://github.com/python/cpython/blob/c7688b44387d116522ff53c0927169db45969f0e/Python/dtoa.c)
* [The V8 double-conversion library](https://github.com/google/double-conversion/tree/master/double-conversion)
* [Blog post about testing algorithms on floats](https://randomascii.wordpress.com/2014/01/27/theres-only-four-billion-floatsso-test-them-all/)
* [Blog post about portability of string/float conversion](https://randomascii.wordpress.com/2013/02/07/float-precision-revisited-nine-digit-float-portability/)
* [Blog post about floating point determinism](https://randomascii.wordpress.com/2013/07/16/floating-point-determinism/)
* [Blog post about string to double conversion bug](http://www.exploringbinary.com/incorrect-round-trip-conversions-in-visual-c-plus-plus/)
* [Blog with many relevant posts](http://www.exploringbinary.com/)

### Open Source Development Experience

C\++ development experience, particularly with C\++11 or C\++14, is necessary. Experience with numerical analysis and the nuances of binary floating point is also needed to be able to correctly implement and validate the correctness of these operations.

### First Steps

* [Get to know DyND](https://github.com/libdynd/libdynd/blob/master/docs/developer-guide.md)
* Immerse oneself in the code and writings about floating point to/from string conversions, and floating point in general.
* Read [DyND's current code for string/number conversion](https://github.com/libdynd/libdynd/blob/master/include/dynd/parse.hpp#L1186)