# Stan

## Protocol Buffer Data Transport Layer

### Abstract

Add protocol buffer support in C++ for data input and sampling for the Stan probabilistic programming language.


| **Intensity** | **Priority | **Involves**  | **Mentors** |
| ------------- | -----------| ------------- | ----------- |
| Moderate      |  High      | C++, statistics | [@sakrejda](https://github.com/sakrejda) |


### Technical Details

[Stan](http://mc-stan.org/) is an open-source (BSD) probabilistic
programming language for fitting statistical models, making
predictions, and estimating event probabilities.  Its current
file-based input is based on an ASCII representation based on the R
language's dump format.  The format is ad hoc and it lacks library
support in languages other than R.  The current file-based output is
based on CSV files with metadata encoded as comments.

These both need to be replaced with a protocol buffer interface that
can be used across Stan's interfaces (command line, R, and Python).

The summer of code project is to provide an implementation of Stan's
I/O using [protocol
buffers](https://developers.google.com/protocol-buffers/), Google's
"language-neutral, platform-neutral extensible mechanism for
serializing structured data."  There is a [protocol buffer C++
tutorial](https://developers.google.com/protocol-buffers/docs/cpptutorial).

The implementation will be done in [Stan's C++
library](https://github.com/stan-dev/stan) so that it may be exposed
to the command line (C++), R, and Python interfaces.  Only the command
line version in C++ is within scope, though a student with appropriate
experience could add support for one or both of R and Python.

### Open Source Development Experience

We're going to be using the following tools:

* GitHub branching, pull requests, and continuous integration

* clang, gcc compilers for cross-platform C++

* make for builds

* google test for unit testing

* doxygen and GitHub wiki for documentation

* Google hangouts for meetings

* Python, R, and statistical modeling experience useful but not necessary


### First steps

* install the protocol buffer package and get hello world (I/O round
  trip) working

* get a Stan program working using CmdStan and the R dump format

* first real step will be implementing a `stan::io::var_context`
  object based on protocol buffers.
