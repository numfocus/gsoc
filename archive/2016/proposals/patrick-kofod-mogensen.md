# Improving the state of Optim.jl
Suggested mentor: Miles Lubin (@mlubin)

## Abstract
The purpose of this GSoC project is to work on a variety of loose ends in, and general improvements to, the optimization package [Optim.jl](https://github.com/JuliaOpt/Optim.jl). Optim.jl is written entirely in the [Julia Language](https://github.com/JuliaLang/julia), and organized under the [JuliaOpt](https://github.com/JuliaOpt/) organization. Improvements are in the form of a unified documentation effort, performance tests, improved algorithms, unit tests, and general bug fixing. At the end of GSoC Optim.jl should be a relatively well-polished, documented, and tested package for optimization in Julia.

## Technical Details
The current state of Optim.jl is actually quite good. A variety of algorithms are implemented for univariate optimization, unconstrained multivariate optimization, and simple constrained optimization. Recently, there has been some effort to improve the optimization API, and a better API for accessing the results is under development. However, the package has a relatively weak developer and user documentation, the tests leave much to be wished for, and there is no systematic performance testing. Certain algorithms, such as the Simulated Annealing, are also much weaker than necessary. Some basic code is already provided to improve some of these algorithms, but they are either in a non-compatible format, or unfinished pull-requests, see [#110](https://github.com/JuliaOpt/Optim.jl/pull/110) for an improved Nelder Mead, and the [pkm/samin](https://github.com/JuliaOpt/Optim.jl/tree/pkm/samin) branch for an improved Simulated Annealing algorithm.

### Documentation
To improve documentation, is it beneficial to introduce two branches of documentation: a developer oriented documentation, and a user oriented documentation. The former explains how the package is organized, how to add new algorithms, and so on. The latter is more of a general tutorial, with additional individual sections showcasing each algorithm.

Proper documentation clarifies exactly where Optim.jl is now. It helps new contributors finding their way around the package, and it eases adaptation on the user side.

### Performance testing
Performance testing is [currently](https://github.com/JuliaOpt/Optim.jl/tree/master/benchmarks) very rudimentary. Primarily, a larger test set is needed. Only nine functions are currently evaluated in a very ad hoc manner, and results are presented in a basic table with no sorting mechanism, or key performance indicators. There are many available sources for test functions, for example from the CUTEst collection, Himmelblau (1972), and more. To the extent possible, I will re-use existing repositories containing these test functions. A starting point is [CUTEst.jl](https://github.com/JuliaOptimizers/CUTEst.jl) for the problems in [CUTEst](http://ccpforge.cse.rl.ac.uk/gf/project/cutest/wiki).

The performance tests will be performed with regards to different measures such as: function evaluations, gradient evaluations, CPU time, memory allocation (for large problems), and more. Standard measures such as means, percentiles, and typical ranks will be presented, but also [performance profiles](http://link.springer.com/article/10.1007%2Fs101070100263). The purpose of this information is to help users reason about the strengths and weaknesses of the different algorithms. This can help guide users to the appropriate algorithm for their task. At the same time, it disciplines contributors to Optim.jl to carefully consider the impact of their contributions.

### Improved algorithms
I propose to improve at least two of the current algorithms as part of this GSoC project. The two I have in mind (Simulated Annealing and Nelder Mead) both have code available, and I will make sure that they are adapted, benchmarked, and merged if they provide improvements over the current algorithms. Obviously, the test set needs to be implemented to make this decision.

Potentially, the improvements are not Pareto improvements. The current implementation of the Nelder Mead algorithm might work well for small problems, while another version might be better suited for large-scale problems. A solution could be to include both in Optim.jl. This comes with the risk of setting a precedent of including all algorithms that improve a corner of the universe of all functions the user might optimize. Another solution is to simply go with one of the two. Ultimately, given the open source nature of the package, it is a community decision. If the complication arises, it is part of this project to engage the community to find an acceptable solution.  

### Unit testing
The last area which needs better testing is testing each individual method in the package. A more detailed testing of the package itself guards against future bugs, and might reveal current bugs.

### JuliaCon application
I have submitted a workshop proposal for JuliaCon 2016. The workshop is meant to be a tutorial in using Optim.jl for optimization in Julia. The recently merged optimization API changes, and the soon to be merged results access API changes, are going to be presented, examples are going to be shown, and the attendees work on provided exercises. Part of my GSoC time is spent preparing this material.

## Schedule of Deliverables

General considerations. This schedule is intentionally very rough. Bullets from above coincide in different ways: the documentation process might reveal bugs, that affects performance, and give rise to new unit tests. It is meant as a guide to which parts of the project receive the most time before and after the midterm.

### May 23th - June 27th

The first work on this project is to improve the documentation. The rest of the project proposal either depend on it, or benefit from it. Personally, it is also be a great opportunity get to know the current code even better.

The user documentation work coincides somewhat with the JuliaCon workshop preparation. As such, it is the second item on my list, although the work on the documentation won't really be finished before the project stops. The workshop is going to provide good feedback with regards to the improved documentation. It can also be a source of examples to include, inspired by the work of the attendees.

### June 28nd - August 14th

With a solid documentation in place, and with the input from JuliaCon, a thorough performance system is going to be put in place. It is important spend a sufficient amount of time to get this right, such that the system can be used in the future.

While algorithmic improvements to the mentioned algorithms are tempting to choose next, unit tests are going to receive attention first. At this point, some work on this has already been done, but this period is a good time to take stock.

Algorithm improvements are last on the list. Not because they are not important, but because they are in some sense only of secondary importance as long as the rest of the work is not in place.

### August 15th - August 23rd 19:00 UTC

Make sure everything is merged, and take care of remaining issues.

## Future works
After the project ends, I hope to continue working on Optim.jl. With a solid framework for documentation and testing, the package should be ready for general use, but also further improvements. This could be things like adaptive algorithms balancing performance and stability, multi-start methods using clustering, and more.

## Open Source Development Experience
I have contributed to a few Julia packages including QuantileRegression.jl (with an interior point based estimation procedure), Optim.jl (general work), Base Julia (testing of the linear algebra methods), and Plots.jl + PGFPlots.jl (adding PGFPlots as a backend to Plots). I am quite comfortable with the technologies used to contribute to Optim.jl.

## Academic Experience
I am a Ph.D. student in Economics at the University of Copenhagen. I work on theoretical and applied topics in econometrics and dynamic programming.

### Experience with Optimization
My official training in optimization is limited to a few courses on numerical methods at the University of Copenhagen, and the optimization part of  [ZICE2015](http://www.zccfe.uzh.ch/pastevents/zice15/announcement.html) taught by [Todd S. Munson](http://www.mcs.anl.gov/~tmunson/) and [Stefan Wild](http://www.mcs.anl.gov/~wild/).

### Experience with the Julia Language
I use Julia in most of my research. Exceptions are collaborations that extend back in time to a point before I started using Julia. These are done in Matlab and C. I have also made some contributions to various Julia projects as listed above, but nothing as systematic as the project described in this proposal.

## Why this project?
Optimization has interested me since I had to minimize my first negative log(likelihod) function. Since I started using the Julia Language, I've had an urge to contribute to the community I derive so much utility from. Combining the two, I hope to improve the state of optimization in Julia.

As stated above, I have previously been using a Matlab+C-combination to do my numerical work. This works and is quite popular. However, it can be quite unproductive, and requires learning two languages quite well. I have found that Julia lives up to its promise of solving the two-language problem, and for this reason I find it more useful to improve Optim.jl than improve a Matlab Optimization toolbox.
