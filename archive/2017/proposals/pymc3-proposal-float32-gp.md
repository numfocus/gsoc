# Single Precision Support, Gaussian Processes

## Abstract

PyMC3 contains a rich suite of building blocks for probabilistic modelling and
inference.  This proposal contains two parts, the first part is finishing
support for single precision, and the second involves finishing Gaussian
Process functionality.  I have already submitted a proposal about exploring
alternative computation engines, and am open to working with the mentors on
mixing and matching projects from each, for example, evaluating PyTorch in
addition to the projects in this proposal.

## Technical Details

Inference on some classes of probabilistic models would be greatly accelerated
by the parallelism offered by modern GPUs.  However, most GPUs only support
32-bit precision for floating point computations.  Theano supports 32-bit
precision computation, and includes many Ops with GPU implementations.  The
computation graph Theano produces is constructed according to a given PyMC3
model.  In order for PyMC3 to take advantage of these Ops, it must cast numeric
values that become inputs to the computation graph to 32-bit precision.  The
goal of this work is to finish single precision support and testing in PyMC3.

When `theano.config.floatX` is set to `'float32'`, all PyMC3 inputs should be
automatically cast to that type.  Routines in PyMC3's internals need to be
checked for places where values that start as `float32` are accidentally
converted to `float64`.  This will involve writing an extensive test suite, or
configuring the current test suite to run with 32-bit precision.

There are several open and closed issues that relate to this issue,
some of which include:
  - [Using PyMC3 on the GPU](https://github.com/pymc-devs/pymc3/issues/1246)
  - [Cast input data and testvalues to theano.config.floatX](https://github.com/pymc-devs/pymc3/issues/1640)
  - [Model raises exception with floatX=float32](https://github.com/pymc-devs/pymc3/issues/1146)
  - [GPU Failure in minimal model](https://github.com/pymc-devs/pymc3/issues/1939)
  - [Sampling failure using initial values with Metropolis](https://github.com/pymc-devs/pymc3/issues/1681)
  - [ValueError: can't copy from un-initialized CudaNdarray during Metropolis using GPU for complex model](https://github.com/pymc-devs/pymc3/issues/1649)

Researching and working on closing these issues will provide a good starting
point on this effort.  When this issue is resolved, this work can transition to
testing, benchmarking, and documenting models that see significant speedups
from GPU computation.

A secondary project will be to round out Gaussian Process functionality and
documentation.  A large chunk of GP functionality has already been included.  I
propose to add the following additional features:

- Extending the `GP` class to accommodate non-Gaussian likelihoods by
  allowing the `GP` class to be specified in a fashion more similar to
  mathematical notation (in pseudocode):

```python
f = GP("latent", m(X), K(X, X))
y = Poisson("likelihood", mu=exp(f))
```
	
- Allowing the `GP` class to be additive, allowing predictions to be made from one of the GPs:

```python
f1 = GP("latent1", 0, K1(X, X))
f2 = GP("latent2", 0, K2(X, X))
y = MvNormal("likelihood", mu=f1 + f2, cov=noise)

gp_samples = sample_gp(trace, samples=50, gp=[f1], evaluate_at=Z)
```

- Add more documentation and notebook examples

- I would also like to add spline functionality to PyMC3.  Splines are useful
in their own right for fitting non-linear functions, but would enhance
Gaussian Process functionality since they would be useful for lengthscale
functions for the `Gibbs` covariance function, and for warping inputs in the
`WarpedInput` covariance function.

## Why this project?

I am interested in working on `float32` support in order to gain a deeper
understanding of the internals of PyMC3, frameworks like Theano, and GPU
computing.  Single precision support looks like a long-standing and high-impact
issue, and addressing it looks like a great way to gain experience in these
subjects.

PyMC3 provides a great foundation for experimenting with Gaussian Processes and
non-linear regression, due to the symbolic differentiation provided by Theano,
its handling of variables and variable transformations via Distribution
objects, and its suite of advanced inference methods, such as NUTS and ADVI.




