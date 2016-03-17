# Making Optim.jl great again
Suggested mentor: Miles Lubin (@mlubin)

## Abstract
The purpose of this GSoC project will be to work on a variety of loose ends in, and general improvements to, the optimization package [Optim.jl](https://github.com/JuliaOpt/Optim.jl). Optim.jl is written entirely in the [Julia Language](https://github.com/JuliaLang/julia), and organized under the [JuliaOpt](https://github.com/JuliaOpt/) organization. Improvements will be in the form of a unified documentation effort, performance tests, algorithm tests and general bug fixing. Certain algorithms will also receive improvements. At the end of GSoC Optim.jl should be a relatively well-polished, documented, and tested package for optimization in Julia.

## Technical Details
The current state of Optim.jl is actually quite good. A variety of algorithms are implemented for univariate optimization, unconstrained multivariate optimization, and simple constrained optimization. Recently, there has been some effort to improve the optimization API, and a better API for accessing the results is under development. However, the package has a relatively weak developer and user documentation, the tests leave much to be wished for, and there is no systematic performance testing. Certain algorithms, such as the Simulated Annealing, are also much weaker than necessary. Some basic code is already provided to improve some of these algorithms, but they are either in a non-compatible format, or unfinished pull-requests, see [#110](https://github.com/JuliaOpt/Optim.jl/pull/110) for an improved Nelder Mead, and the [pkm/samin](https://github.com/JuliaOpt/Optim.jl/tree/pkm/samin) branch for an improved Simulated Annealing algorithm.

### Documentation
To improve documentation, it will be beneficial to introduce two branches of documentation: a developer oriented documentation, and a user oriented documentation. The former will explain how the package is organized, how to add new algorithms, and so on. The latter will be more of a general tutorial, with additional individual sections showcasing each algorithm.

### Performance testing
Performance testing is [currently](https://github.com/JuliaOpt/Optim.jl/tree/master/benchmarks) very rudimentary. It will be improved in several directions. A larger test set is needed. 9 functions are currently evaluated in a very ad hoc manner. Results are presented in a table with no sorting mechanism, or key performance indicators.

To improve what you might call coverage, I will extend the test set significantly. There are many sources available, for example from the CUTE(r) collection, Himmelblau (1972), and more. The will be tested at least at their canonical starting values, possibly also elsewhere.

The performance tests will be performed with regards to different measures such as: function evaluations, gradient evaluations, cpu time, memory allocation (for large problems), and more. Standard measures such as means, percentiles, and typical ranks will be presented, but also [performance profiles](http://link.springer.com/article/10.1007%2Fs101070100263).

### Improved algorithms
I propose to improve at least two of the current algorithms as part of this GSoC project. The two I have in mind (Simulated Annealing and Nelder Mead) both have code available, and I will make sure that they are adapted, benchmarked, and merged if they provide improvements over the current situation. Obviously, the test set needs to be implemented to make this decision. Other possibilities might present themselves along the way.

### Unit testing
The last area which needs better testing is testing each individual method in the package. A more detailed testing of the package itself will guard against future bugs, and might reveal current bugs.

### JuliaCon application
I have submitted a workshop proposal for JuliaCon 2016. The workshop is meant to be a tutorial in using Optim.jl for optimization in Julia. The recently merged optimization API, and the soon to be merged results access API changes will be presented, and the attendees will see examples, and work on provided exercises. Part of my GSoC time will be spent preparing this material.

## Schedule of Deliverables

### May 23th - June 27th

- Documentation
- Performance
- JuliaCon workshop


### June 28nd - August 14th

 - Unit tests
 - Algorithm improvements

### August 15th - August 23rd 19:00 UTC

- Make sure everything is merged, and take care of issues

## Future works
After the project ends, I hope to continue working on Optim.jl. With a solid framework for documentation and testing, the package should be ready for general use, but also further improvements. This could be things like adaptive algorithms balancing performance and stability, multi-start methods using clustering, and more.

## Open Source Development Experience
I have contributed to a few Julia packages including QuantileRegression.jl (with an interior point based estimation procedure), Optim.jl (general work), Base Julia (testing of the linear algebra methods), Plots.jl (adding PGFPlots as a backend). I am quite comfortable with the technologies used to contribute to Optim.jl.

## Academic Experience
I am a Ph.D. student in Economics at the University of Copenhagen. I work on theoretical and applied topics in econometrics and dynamic programming. As such, my official training in optimization is limited to a few courses on numerical methods at the University of Copenhagen, and the optimization part of  [ZICE2015](http://www.zccfe.uzh.ch/pastevents/zice15/announcement.html) taught by [Todd S. Munson](http://www.mcs.anl.gov/~tmunson/) and [Stefan Wild](http://www.mcs.anl.gov/~wild/). In relation to Julia, I use it for most of my research. Exceptions are collaborations that extend back in time to a point before I started using Julia. These are done in Matlab and C.

## Why this project?
Optimization has interested me since I had to minimize my first negative log(likelihod) function, and since I learned about the Julia Language, I've had an urge to contribute to a community I derive much utility from. Combining the two, I hope to improve the state of optimization in Julia.

## Appendix
