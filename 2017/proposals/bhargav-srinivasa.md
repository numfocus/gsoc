# Title

Implementing Riemannian Manifold HMC

## Abstract

pymc3 has a powerful range of sampling methods, with the Hamiltonian Monte Carlo (and NUTS) being the crown jewel.
And with the [NUTS](https://arxiv.org/abs/1111.4246) algorithm, HMC can be optimized adaptively - also, empirically, NUTS performs at least as efficiently as and sometimes more efficiently than a well tuned standard HMC method, without requiring user intervention or costly tuning runs. There are some minor drawbacks to this - HMC doesn't always work so well on extremely complex models, and sometimes NUTS can fail by terminating prematurely before reaching the optimal integration time.

We can build on the HMC method by introducing Riemannian Manifold HMC to pymc3, which improves the auto-tuning behavior of the NUTS algorithm and provides independent samples on extremely complex models. In the [paper](https://arxiv.org/pdf/1304.1920.pdf) on RMHMC to NUTS, we are explained the motivation behind this - ``` The No U-Turn Sampler identifies these turning points for Euclidean manifolds, but the criterion begins to fail when applied to more complex distributions and Riemannian manifolds. Appealing to the geometry of HMC, however, admits a straightforward generalization of the No U-Turn Sampler that is not only amenable to Riemannain manifolds but also isolates the turning points in more compli- cated, non-convex target distributions.```

By implementing RMHMC as an alternate sampling method for pymc3, we can tackle more problems and provide a powerful alternative to the existing sampling methods.


## Technical Details

Michael Betancourt's implementation [here](https://github.com/betanalpha/jamon) would serve as a very useful blueprint while going about RM-HMC. It is also coded in Stan, but not exposed (due to potential fragility of higher-order autodiff). This [issue](https://github.com/stan-dev/stan/issues/304) discusses this in more detail, also linking to the files in particular relevant to the RMHMC implementation. We would require higher-order autodiff for probability distributions, which would be a mini-task in itself, but with Theano this should be managed. 

The current implementations use the Softabs metric, and this approach is explained in [this paper](https://arxiv.org/pdf/1212.4693.pdf).

Other useful links include a [MATLAB implementation of RMHMC](https://github.com/a-kramer/ode_rmhmc), and the [paper](http://www.dcs.gla.ac.uk/publications/PAPERS/9149/RMHMC_MG_BC_SC_07_09.pdf) by Girolami & Calderhead.

The API would require discussion - but could be similarily structured to the [HMC directory](https://github.com/pymc-devs/pymc3/tree/master/pymc3/step_methods/hmc), i.e create a RMHMC directory with it's own NUTS algorithm. 

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

- A huge part of being part of the open-source community and Google Summer of Code is sharing your ideas and work. I've started a blog [here](https://summerofcode2017.wordpress.com/) to give regular reports of the summer work I will be doing, and try and break down the world of Probabilitic Programming to everyone.

- Wrap up exisitng PRs and continue to help around with issues and bugs. I would also start work on setting up RM-HMC and NUTS. It would be useful to have the basic API and structure ready so that we can focus on heavy lifting the rest of the period. 

- Communication with mentors to clarify doubts, re-read the papers and Stan code to make sure everything is in place. 

### May 29th - June 3rd

- Since the current implementation is based on the Softabs metric, it would make sense to first see if we can replicate [this](https://github.com/stan-dev/stan/blob/develop/src/stan/mcmc/hmc/hamiltonians/softabs_metric.hpp), and whether we would need to at all.

- If we haven't already, decide the API which we will start working with. 

### June 5th - June 9th

- The current road-block in the Stan implementation is because of instability of higher-order autodiffs. Exploring this problem and how a pymc3 implementation of the same will handle these problems is the next-step.


### June 12th - June 16th

- By now we will have our basic road-map in place, and can start implementing the algorithm. Before Phase 2 begins a PR should be opened which has the skeleton code ready!

- It would also be important to decide how much of the existing implementations (linked to before) we would like to be influenced by. 

### June 19th - June 23th, **End of Phase 1**

- As phase 1 ends, the design decisions should end and RM-HMC should starting taking up some shape.

- We will follow the philosophy of test driven development, and regularly document the work as it progresses.

### June 26 - July 7th, **Begin of Phase 2**

- Phase 2 would mark the start of the bulk of the coding work. 

- A blog post detailing how MCMC, HMC and RM-HMC work would be a nice way to talk more about pymc3 and GSoC 2017.

### July 10th - July 14th

- By now, RM-HMC would have taken considerable shape - our focus would now be on how to extend the NUTS algorithm appropriately to this.

### July 17th - July 21th, **End of Phase 2**

- This would be enough time to be done with RM-HMC and the NUTS algorithm, coinciding with the end of Phase 2.

- The rest of the time would be detailed to testing, benchmarks, further documentation (with tutorials).

### July 24th - August 4th, **Begin of Phase 3**

- The performance of the RMHMC can assessed by performing posterior inference on logistic regression models, log-Gaussian Cox point processes, and stochastic volatility models.

- The bulk of this period will involve finish testing, bugs and edge cases.

### August 7th - August 11th

- Another blog post detailing the work done so far, and how NUTS works.

- Wrap up tests, documentation. Perform memory/speed benchmarks.

### August 14th - August 18th

- Jupyter Notebook to accompany the rest of the code.

### August 21st - August 25th, **Final Week**

- The last week will involve any remaning code cleaning, and finishing the tutorial.

### August 28th - August 29th, **Submit final work**

- Have PR merged, celebrate!

## Future works

With regard to sampling methods and MCMC/HMC in particular, Betancourts work is particularly interesting [this](https://arxiv.org/pdf/1601.00225.pdf) paper talks about XMC, which claims to be twice as fast as the regular NUTS algorithm. I intend to keep contributing to pymc3 with issues and PRs, and be an active part of the community.

## Open Source Development Experience

I enjoy and regularly contribute to open source scientific computing libraries. I was previously selected to participate in Google Summer of Code 2016 with Gensim under the NumFOCUS umbrella, where I implemented Dynamic Topic Models. My [blog](https://summerofcode2017.wordpress.com/) details my experiences during summer of 2016.

I have been using pymc3 for my research work at INRIA, and to learn probabilitic programming myself. I've recently started contributing to pymc3 - these are my [contrinutions](https://github.com/pymc-devs/pymc3/issues?q=is%3Aopen+mentions%3Abhargavvader).

I'm still a regular contributor for [Gensim](https://github.com/RaRe-Technologies/gensim/pulls/bhargavvader) - I have 29 pull requests merged, and actively help on the mailing list and issues. I have given talks at PyCon France 2016 and PyCon Slovakia 2017 about my experience with Gensim and GSoC 2016. I also contribute to [metric-learn](https://github.com/all-umass/metric-learn/pulls?q=is%3Apr+author%3Abhargavvader+is%3Aclosed), [Edward](https://github.com/blei-lab/edward/pulls/bhargavvader), and [spaCy](https://github.com/explosion/spacy-notebooks/pulls?q=is%3Apr+author%3Abhargavvader+is%3Aclosed). The links direct to my PRs/issues for the respective repos. My [GitHub profile](https://github.com/bhargavvader) has a more detailed summary of all my contributions. 

I intend to continue my open source contributions, and to keep contributing to pymc3 as well!

## Academic Experience

I am a student researcher at INRIA, France. I work with the MODAL (Models Of Data Analysis and Learning) team, where I work on Predictor Aggregation and Metric Learning problems. I'm finishing up my undergraduate education in Computer Science Engineering from BITS Pilani University, India. My [resume](https://drive.google.com/file/d/0By80y9AXd1WsRUJWTlFfeldITGc/view?usp=sharing) details my previous internships and research experiences, which are all in the field of Machine Learning, Data Science and Software Engineering.

## Why this project?

One of pymc3's main "selling points" are it's powerful suite of sampling methods. While NUTS is arguably the most powerful tool in the box, it can be bettered, and RMHMC is a way to do this. Working on this project would mean that I am contributing to one of the most important parts of pymc3, while providing an addition which isn't there in any other Probabilistic Programming languages/packages yet. 
On a personal note, the project serves as a way to better my Physics (and Math) while contributing, subjects I have always loved but never spent the amount of time I would like on.

## Appendix

The No-U-Turn Sampler: Adaptively Setting Path Lengths in Hamiltonian Monte Carlo - Matthew D. Hoffman, Andrew Gelman

Riemann Manifold Langevin and Hamiltonian Monte Carlo - Mark Girolami, Ben Calderhead, Siu A. Chin 

A General Metric for Riemannian Manifold Hamiltonian Monte Carlo - Michael Betancourt

Generalizing the No-U-Turn Sampler to Riemannian Manifolds - Michael Betancourt

Adaptive Hamiltonian and Riemann Manifold Monte Carlo Samplers - Ziyu Wang, Shakir Mohamed, Nando de Freitas



