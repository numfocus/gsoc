# Explore Alternative Computation Engines for PyMC3

## Abstract

Theano provides symbolic differentiation of mathematical expressions, enabling
the application of powerful, gradient-based inference algorithms to arbitrary
probabilistic models in PyMC3.  Vectorization, parallelism, GPU computation,
and dynamic C code generation are added benefits.  Underlying the deep learning
boom, other computational frameworks have also become popular, such as
TensorFlow, MxNet, CNTK, Chainer, PyTorch, and others.  Perhaps the most
relevant of these to PyMC3 are TensorFlow and PyTorch.  Like Theano, these
frameworks include extensive libraries of low-level mathematical and linear
algebra operations.  

The goal of this work will be to examine how alternative computation engines
effect the performance and the ease of use of PyMC3.  I will make two forks of
PyMC3, one with TensorFlow and the other with PyTorch in place of Theano.  I
will analyze the performance of common models and tests in each of the two
forks and the master.  Via blog posts, I will document performance results with
an eye towards how the backend impacts ease-of-use from the perspective of a
user.  This work will also result in a detailed programming guide and PyMC3
architecture overview (see [issue #1359][issue]) that can be added to the
documentation.  Finally, my mentors and I will design an abstraction layer
between PyMC3 core code and Theano.  If deemed successful, I hope to merge this
into master by the end of Phase 3.

This is certainly a large and challenging project.  I plan to carefully
document progress via blog posts, some of which will be able to be turned into
a developer guide or architecture overview for the documentation.  I hope this
work will provide value by helping understand the computational demands of
probabilistic programming and will benefit the PyMC3 project going into the
future.  

## Technical Details

### Motivation

PyMC3 relies on Theano to execute the computation graph and its gradient.
However the vast majority of PyMC3's codebase is devoted to facilitating
probabilistic modeling.  It includes libraries of inference methods, random
variable types, methods to handle and analyze traces, and plotting
functionality --- routines that are weakly bound at most to the computational
backend.  Implementing an abstraction layer between PyMC3 core code and the
computational backend will make the codebase more maintainable.  It would be
easier to categorize issues as being related to either Theano or PyMC3 core
code.  PyMC3 already has `theanof.py` and `math.py`.  Instead of making calls
throughout the codebase directly to Theano, PyMC3 would interact with Theano
through calls to the interface.

A different backend may increase PyMC3's computational efficiency.  Results from
benchmarking different frameworks must be taken with a big grain of salt in the
context of PyMC3, since they naturally use deep neural networks and not
probabilistic models as test cases.  The only way to find out would be to test!

### Computational Backends

The two most relevant deep learning frameworks to PyMC3 are PyTorch and
TensorFlow (not counting Theano), based on their more extensive libraries of
mathematical and linear algebra operations.

The operations currently available are:

- [Theano][theano-ops]
- [TensorFlow][tf-ops]
- [PyTorch][torch-ops]

1. [Theano][theano-technote] represents a series of computations expressed by a
user as a directed acyclic graph (DAG).  The DAG representation allows for
speed increases via symbolic simplifications (such as $\frac{cx}{x} \rightarrow
c$) and dynamic C code generation.  Some operations have methods defined on
both the CPU and GPU.

2. [TensorFlow][tensorflow-technote] generates a DAG in a similar fashion as
Theano.  TensorFlow is a much broader framework than Theano.  It includes
minimization routines, facilities for reinforcement learning, and it is
designed to allow computation to scale on distributed systems, multiple GPUs,
and specialized devices like Google's new TPUs.  The recent release of
[XLA][xla] aims to optimize lower-level mathematical and linear algebra
operations to improve execution speed.  TensorFlow appears to currently have
the largest userbase of the three, based on a superficial look at GitHub
activity.

3. [PyTorch][pytorch-technote] is the new Python API to Torch.  Unlike Theano and
TensorFlow, the PyTorch execution graph is dynamically defined.  Reverse-mode
auto-differentiation is used to compute gradients on the fly.  Without the
definition of a static computation graph before execution, its not possible for
PyTorch to do symbolic optimizations like Theano and TensorFlow.  However, its
dynamic computation graphs allows variable-length inputs and outputs to be
processed.  This may make the implementation of prediction methods in PyMC3
more straightforward.  PyTorch code is easier to debug than TensorFlow or
Theano (due to its dynamism) and advertises its extensibility.  Since PyTorch's
dynamic computational model is different from Theano and TensorFlow's, it may
be more difficult to include in a common interface with TensorFlow and Theano.

### Similar Work

- [Keras][keras] is a higher-level deep learning library that allows one to choose between
Theano or TensorFlow.  A configuration file is used to specify backend
settings.  
- [TensorFuse][tensorfuse] is a library which provides a common
interface to Theano and Tensorflow (although it appears to not be actively
developed)
- [Edward][edward] is an inference engine similar to PyMC3.  It is built on top
of TensorFlow instead of Theano.
- The documentation of [MxNet][mxnet-docs] contains an interesting overview of
deep learning framework architectures.

### Summary of Deliverables

This list of deliverables is purposefully optimistic.  Will work with mentors
to refine or narrow scope if they deem necessary.  For instance, the scope
could be narrowed to test one of the computation engines instead of both.

- A series of blog posts comparing evaluations of PyMC3 on Theano, TensorFlow,
and PyTorch 

- Developer and PyMC3 architecture guide

- Develop interface between PyMC3 and Theano

## Schedule of Deliverables

**Schedule is tentative**

### May 1th - May 28th, **Community Bonding Period**

As I get to know the mentors and other members of the PyMC3 community, I will
study how to disentangle PyMC3 from Theano and research TensorFlow and PyTorch
internals and functionality.

- Experiment with and research architecture of Theano, TensorFlow, PyTorch, and PyMC3

- Decide with mentors on the first alternate computation engine to try

Prior to the community bonding period, I plan to continue submitting
contributions to extend PyMC3's Gaussian Process functionality.

### May 29th - June 3rd

- Work through PyMC3 identifying and categorizing Theano calls as being either
mathematical operations (`tensor.dot`, `tensor.transpose`, etc.) or
Theano-specific (`theano.function`, `scan`, `tensor.as_tensor_variable`, etc.)

- Check for glaring differences across backends in common operations such
as array slicing behavior and the signatures of mathematical operations

### June 5th - June 9th

- Identify example model or existing unittests that would make good test cases

- Begin porting the subset of PyMC3 needed to run the decided upon tests

- Write a blog post comparing and contrasting TensorFlow, Theano, and PyTorch
with a focus on their usage as general purpose computional engines, not as
deep learning frameworks

### June 12th - June 16th

- Continue porting

- Begin setting up the testing suite

- Start draft of PyMC3 architecture and developer guide

### June 19th - June 23th, **End of Phase 1**

- Finish the first fork

- Finish testing suite

- Make sure the tests run

### June 26 - June 30th, **Begin of Phase 2**

- Document the comparison of the performance of Theano-PyMC3 and the
first fork on the tests as a blog post

### July 3rd - July 7th

- Begin the second fork

- Iterate on PyMC3 developer guide 

### July 10th - July 14th

- Continue work on the second fork

### July 17th - July 21th, **End of Phase 2**

- Finish the second fork

- Make sure tests run

### July 24th - July 28th, **Begin of Phase 3**

- Compare performances

- Understand why some models may work better than others in different forks

- Document the comparison of the performance of Theano-PyMC3 and the
second fork on the tests as a blog post

### July 31st - August 4th

- Test forks with GPU enabled

- Write blog post comparing results on GPU

### August 7th - August 11th

- Finish up testing

- Document overall results in a blog post

- Design abstraction layer, or interface, between PyMC3 and Theano

### August 14th - August 18th

- Write tests for the interface

### August 21st - August 25th, **Final Week**

- Write the interface

### August 28th - August 29th, **Submit final work**

- Write documentation for interface

- Finish the PyMC3 developer guide and add to documentation

- Merge! 

## Future Works

One of the most important things about developing open source code is being
available into the future to fix bugs, answer questions, and help maintain the
contributed code.  I hope to gain the expertise to help maintain  

I think avenues for future work depend on the results of this study.  The use
of an abstraction layer between Theano and PyMC3 would make PyMC3 easier to
maintain.  It will also allow easier testing of PyMC3 on alternative backends
in the future.  This would allow PyMC3 to benefit from the rapid development
and advances happening not just within Theano, but throughout the area of deep
learning frameworks.   

## Development Experience

- Experience developing software in Python and Scala

- Helped develop functionality for [Hadrian](https://github.com/opendatagroup/hadrian), which was later open-sourced

- Matplotlib plotting methods for [Histogrammar](https://github.com/histogrammar/histogrammar-python)

- PyMC3 contributions listed [here](https://github.com/pymc-devs/pymc3/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Abwengals)

## Other Experiences

- See [resume](https://github.com/bwengals/resume) 

## Why this project?

I am interested in undertaking this project because it is an opportunity to
learn in-depth about both computational frameworks and the architecture of
probabilistic programming software at a deep level.  PyMC3 uniquely resides at
the exciting intersection of these two paradigms.

Software like BUGS, JAGS, STAN, and PyMC3 have helped make Bayesian inference
accessible, allowing it to be employed in everyday modelling tasks.  Becoming
an expert in using and developing PyMC3 under the supervison of mentors would
be a huge benefit my academic and professional developemt.  

## Appendix

- Starting MS in Statistics in Fall 2007
- Eligible for GSoC
- Issue filed on NumFOCUS: [link](https://github.com/numfocus/gsoc/issues/197)

[theano-technote]: https://arxiv.org/abs/1605.02688
[tensorflow-technote]: https://www.tensorflow.org/about/bib
[pytorch-technote]: http://pytorch.org/about/

[theano-ops]: http://deeplearning.net/software/theano/library/tensor/index.html 
[tf-ops]: https://www.tensorflow.org/versions/r0.11/api_docs/python/math_ops/
[torch-ops]: http://pytorch.org/docs/torch.html#math-operations

[xla]: https://developers.googleblog.com/2017/03/xla-tensorflow-compiled.html
[keras]: https://github.com/fchollet/keras
[tensorfuse]: https://github.com/dementrock/tensorfuse
[edward]: https://github.com/blei-lab/edward
[mxnet-docs]: http://mxnet.io/architecture/index.html

[issue]: https://github.com/pymc-devs/pymc3/issues/1359
