# Title

Convert sampling methods to pure Theano

## Abstract

pymc3 is a well known probabilistic programming framework known for its Hamiltonian Monte Carlo sampling method.

Hamiltonian Monte Carlo [HMC] (https://arxiv.org/pdf/1206.1901) works by reducing the correlation between successive sampled states by using a Hamiltonian evolution between states and additionally by targeting states with a higher acceptance criteria than the observed probability distribution. This causes it to converge more quickly to the absolute probability distribution.

Theano is a numerical computation library for Python. In Theano, computations are expressed using a NumPy-like syntax and compiled to run efficiently on either CPU or GPU architectures

In the current implementation, the sampling methods are a mixture of calls to Python functions and data structures, use Theano partially. By completely porting the methods to Theano, significant improvement in performance is expected.


## Technical Details

HMC is already implemented in pymc3 but is currently inefficient as it doesn't leverage Theano's efficient machinery very well yet. First thing to do would be to be to identify the parts of the code not yet currently using Theano's machinery. Once identified, we should identify how this can be ported to Theano. 

Anand Patil's implementation [here] (https://github.com/apatil/pymc-theano) can be useful in identifying how to leverage Theano correctly.

Theano uses Basic Linear Algebra Subprograms [BLAS] (www.netlib.org/blas/) which prescribes a set of low-level routines for performing common linear algebra operations such as vector addition, scalar multiplication, dot products, linear combinations, and matrix multiplication. This makes Theano extremely fast and efficient which cannot be achieved by vanilla python calls. By using Theano, this can be used for our benefit.

 

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

- A big aim of part of Google Summer of Code is to introduce students to open-source and encourage them to become active members of open-source. This gives a chance to bond with the community and share ideas and work. I have a blog [here](http://alekhka.wordpress.com/) where I plan to give regular reports of the summer work I will be doing, and communicate the changes I will be doing to everyone.

- Help with existing PRs and help around with issues and bugs. 

- Communication with mentors to clarify doubts, read the papers and implementations. 

### May 29th - June 3rd

- Gain more insight into sampling methods and its implementation in pymc3.
 

### June 5th - June 9th

- Look at current implementation and identify code not using theano yet.


### June 12th - June 16th

- Look at the current implementation and look for bottlenecks in the code and devise plans to mitigate them.


### June 19th - June 23th, **End of Phase 1**

- Getting a proper road map in place and document it.

- We will have to consult the mentors regarding this and get suggestions.


### June 26 - July 7th, **Begin of Phase 2**

- Phase 2 would is the start of the bulk of the coding work. 

- A blog post about the changes to be made and expected improvements.

- Identifying the Theano machinery to be used and start implementing.

### July 10th - July 14th

- By now, lot of improvement would have happened and it would be taking a good shape.

### July 17th - July 21th, **End of Phase 2**

- This would be enough time for most changes. Additional improvements if needed will also be addressed. 

- The rest of the time would be devoted to testing, benchmarks, further documentation.

### July 24th - August 4th, **Begin of Phase 3**

- The performance of the sampling methods is to be properly documented.

- The bulk of this period will involve finish testing, bugs and improve documentation.

### August 7th - August 11th

- Another blog post detailing the work done so far.

- Wrap up tests. Perform new performance benchmarks.

### August 14th - August 18th

- Finishing the documentation with proper details about changes made so that bug fixing becomes easier in the future.

### August 21st - August 25th, **Final Week**

- The last week will involve any remaining code cleaning and make changes if advised by the mentors.

### August 28th - August 29th, **Submit final work**

- Have the PR merged.



## Future works

I intend to keep contributing to pymc3 with issues and PRs, and be an active part of the community. I would like to further improve the sampling methods if there is a chance for it.

## Open Source Development Experience

I am huge fan of Open-source software and have contributions in them. I have served as the Lead Programmer for a autonomous vehicle and UAV project in my Sophomore year. I have extensively contributed to its private repository.

I have interned at 1byzerolabs, a startup in Bangalore area, where I have worked for almost an year (remotely for 6 months). I have contributed to the company's code base.

Finally, I have served as lead programmer for my college fest (8th mile) and have contributed significantly there too.

Even though I don't have experience with scientific computing libraries, I have been on the mailing lists of many for long and thus have good knwoledge about expectations and development pipelines.  

I intend to kick start my contribtuions to large open-source projects with GSoC, and to keep contributing to pymc3!

## Academic Experience

I am a student at RV College of Engineering, Bangalore. I have worked with student projects specialising in autonomous vehicles and UAVs. I have also completed a research internship. My [resume](https://drive.google.com/open?id=0B4lOWUYt1pLFWWhkOWIzQzJTUGs) details my previous internships and research experiences.

## Why this project?

I have always been motivated towards open-source and programming. I have heard a lot of good things about pymc3 from various quarters and was always wanted to be part of it. I was pleasantly surprised when I saw pymc3 in GSoC. I have interacted with the mentor (Dr Colin Carroll) and he has been very helpful. All these made me feel that I would be a right fit for the project.

## Appendix

MCMC using Hamiltonian dynamics - Appears as Chapter 5 of the Handbook of Markov Chain Monte Carlo - Radford M. Neal, University of Toronto.

Anand Patil's theano implementation: https://github.com/apatil/pymc-theano





