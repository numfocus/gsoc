# JuliaOpt

## Develop a Julia interface to the [SCIP](http://scip.zib.de/) solver


### Abstract

SCIP is a powerful noncommerical solver for [mixed-integer programming](https://en.wikipedia.org/wiki/Integer_programming) and
[constraint programming](https://en.wikipedia.org/wiki/Constraint_programming).
It provides advanced features like [callbacks](http://jump.readthedocs.org/en/latest/callbacks.html) which are used to attack challenging combinatorial problems
like the [TSP](https://en.wikipedia.org/wiki/Travelling_salesman_problem).
The goal of this project is to develop a fully featured and documented
interface to SCIP through its C API, which will enable
users to access SCIP and all of its advanced features from Julia,
and in particular from the modeling interfaces
[JuMP](https://github.com/JuliaOpt/JuMP.jl)
and [Convex.jl](https://github.com/JuliaOpt/Convex.jl).

### Motivation

Julia already has wrappers for many optimization solvers (listed [here](http://www.juliaopt.org/)). Compared with the open-source mixed-integer programming solvers currently supported (GLPK, Cbc), SCIP is faster and still provides access to its source code for research purposes (although unfortunately not under an open-source license). Compared with the commerical solvers, SCIP has a much lower barrier to entry in terms of licensing costs. Google itself has chosen to use SCIP internally and supports it through its [or-tools](https://github.com/google/or-tools) package.

Providing first-class access to SCIP from Julia will enable new applications of 
integer programming, made easier by Julia and JuMP's high-level syntax and abstractions
over solvers. SCIP will be suitable for use in courses, research, and industrial applications.

### Technical details

SCIP has quite a large API, so it is a good idea to generate
the Julia wrappers automatically. There was a
[previous attempt](https://github.com/ryanjoneil/SCIP.jl) at wrapping
SCIP which may serve as a useful starting point. Since then,
the ``Ref{}`` syntax in Julia 0.4 may make some of the wrapping
easier than before.

This project requires knowledge of basic linear programming, experience with C, and ideally experience with JuMP or another algebraic modeling language.

### Mentors

* [@mlubin](https://github.com/mlubin)
* [@joehuchette](https://github.com/joehuchette/)

### Contact

[NumFocus GSOC list](https://groups.google.com/a/numfocus.org/forum/#!forum/gsoc)

[JuliaOpt list](https://groups.google.com/forum/#!forum/julia-opt)


## Solve complex SDPs with the [Convex.jl](https://github.com/JuliaOpt/Convex.jl/i) modeling language

### Abstract

**Convex.jl** is a [Julia](http://julialang.org) package for [Disciplined Convex Programming](http://dcp.stanford.edu/). Convex.jl makes it easy to describe optimization problems in a natural, mathematical syntax, and to solve those problems using a variety of different (commercial and open-source) solvers, through the [MathProgBase](http://mathprogbasejl.readthedocs.org/en/latest/) interface.
This project would add support for solving complex semidefinite programs (SDP) to Convex.jl.

### Motivation

Convex.jl is widely used in industry and research to solve structured 
convex optimization problems, including LP, SOCP, and SDP with real variables and data.
This project extends the problem types that can be solved using Convex.jl.

Many problems in applied mathematics, engineering, and physics are most
naturally posed as convex optimization problems over complex valued
variables and with complex valued data. These include

a) Phase retrieval from sparse measurements.
b) Optimization problems in AC power systems
c) Frequency domain analysis in signal processing and control theory

While optimization over complex numbers can always be encoded as
optimization over real variables through transformations, this often
results in significant overhead (both in user effort and computation
time) in many applications. Support for complex convex
optimization in Convex.jl would boost the usage of Julia
as a language of choice for users working on these and other
applications.

### Technical details

This project would add support for complex variables and data to Convex.jl.
This work entails writing functions to transform complex SDPs into equivalent
real valued SDPs, and to transform the solutions back from real to complex
variables. 

Students with further background and motivation could continue to improve
the SDP solver itself. In particular, the transformations used by Convex.jl
to write a problem as an SDP often introduce many extra variables and constraints
than are necessary, and may result in poor conditioning. A presolve routine,
eliminating redundant variables and constraints and improving conditioning before
passing the problem to a solver, would be a welcome addition to the Convex.jl library.
While many tricks for presolving LPs are well known, there is significant room for 
imagination in writing a presolve for SDP; the project might well lead to a publication
were the GSOC student so inclined.

This project requires knowledge of basic linear algebra, convex optimization, 
and Julia programming.

### Mentors

* [@madeleineudell](https://github.com/madeleineudell)
* [@mlubin](https://github.com/mlubin)
* [@dvij](https://github.com/dvij)

### Contact

[NumFocus GSOC list](https://groups.google.com/a/numfocus.org/forum/#!forum/gsoc)

[JuliaOpt list](https://groups.google.com/forum/#!forum/julia-opt)

