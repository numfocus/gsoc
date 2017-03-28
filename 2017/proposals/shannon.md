# PyMC3: Implement non-parameteric Bayesian Methods #

## Abstract ##

Bayesian nonparametrics (BNP) is a major part of the cutting edge in machine learning today. Since the initial work of Tom Ferguson on the Dirichlet process in the 1970s, there have been tremendous extensions and forays into the field. Bayesian nonparametric models promise flexibility and scalability while maintaining strong theoretical guarantees. In particular, the Bayesian paradigm offers decision-theoretic guarantees on consistency, while the nonparametric approach ensures liberates the modeler from assumptions that are rarely satisfied and difficult to test for, yet near impossible to do inference without.

However, BNP models are not as popular as other supervised or unsupervised learning techniques for several reasons. Firstly, BNP models begin by placing probability distributions on spaces of probability distributions. The mathematical maturity required to read papers on Bayesian nonparametrics makes them inaccessible to many practitioners of data analysis, who would otherwise benefit from these techniques. Secondly, posterior inference in many classes of BNP models is intractable necessitating approximations, though this is less of a problem with frameworks like PyMC3 and Stan. Finally, there exist very few if any practical implementations of Bayesian nonparametric techniques.

In this GSoC project, I'd like to implement some of the more commonly used Bayesian nonparametric models in PyMC3. I'd also like to write extensive documentation and tutorials to help make PyMC3 and Bayesian nonparametric modelling more accessible to end users who may not have any experience with advanced machine learning.

## Technical Details ##

Over the course of the summer, I'd like to implement combinatorial BNP models in PyMC3, as well as improve on inference techniques for these models. I plan on implementing the Dirichlet process \[[Ferguson 1973,](https://projecteuclid.he maorg/euclid.aos/1176342360), [Sethuraman 1994](http://www3.stat.sinica.edu.tw/statistica/oldpdf/A4n216.pdf)\], the Dirichlet process mixture model \[[Antoniak 1974](https://projecteuclid.org/euclid.aos/1176342871)\], Polya trees \[[Müller 2013](https://projecteuclid.org/download/pdfview_1/euclid.cbms/1362163749)\] the hierarchical Dirichlet process \[[Teh et al. 2006](https://www.stats.ox.ac.uk/~teh/research/npbayes/jasa2006.pdf)\].

[Edward](https://github.com/blei-lab/edward) \[[Tran el al. 2016](https://arxiv.org/abs/1610.09787)\] has recently added Dirichlet processes and I hope to use their implementation as a reference.

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

Spend time familiarizing myself with the PyMC3 codebase, and start with API design for the underlying combinatorial stochastic processes. Also, review existing literature to familiarize myself with any identified issues with implementing Bayesian nonparametric models.

### May 29th - June 9th

Implement the Dirichlet process, including tests and documentation. Compile a notebook explaining how Dirichlet processes work with examples. This should be at accessible to someone with a machine learning or statistics course under their belt.

### June 12th - June 16th

Implement Dirichlet process mixture models based on the Dirichlet process, along with tests.

### June 19th - June 23th, **End of Phase 1**

Commit and submit a pull request. Fix anything that has been overlooked and merge into the dev branch. I've kept a buffer period for any delays.

### June 26 - June 30th, **Begin of Phase 2**
Begin implementing Polya trees for density estimation.

### July 3rd - July 14th
Complete implementing Polya trees for density estimation. Implement clustering models based on mixtures of Polya trees.

### July 17th - July 21th, **End of Phase 2**
Write up the documentation for Polya trees. Commit and submit a pull request, and fix anything that's been overlooked.

### July 24th - July 28th, **Begin of Phase 3**
Begin implementing hierarchical Dirichlet processes for clustering and grouping data.

### July 31st - August 4th
Finish implementing hierarchical Dirichlet processes.

### August 7th - August 11th
Add documentation and tests for hierarchical Dirichlet processes. At this point, I'd like to start working on a tutorial on BNP models in PyMC3 accessible to someone with just a first course in machine learning or statistics.

### August 14th - August 18th
Complete writing the tutorial.

### August 21st - August 25th, **Final Week**
Submit a final pull request for hierarchical Dirichlet processes, and fix anything that's been overlooked.

### August 28th - August 29th, **Submit final work**

## Future work and extensions
Bayesian nonparametrics is advancing at an extraordinary pace. A straightforward extension to the work I've proposed would be to add other prior processes: Pitman-Yor,
hierarchical beta, and other stickbreaking prior processes. I plan to write the code in a modular way so that the prior distributions are decoupled from the clustering and density estimation models. This would make implementing other priors for the same models, and other models for the same priors as straightforward as possible. I plan on continuing working on BNP models in PyMC3 after GSOC as a regular contributor.

## Why this project?
As a statistics graudate student, I plan on working on theoretical guarantees for Bayesian nonparametric models. Implementing these models for PyMC3 is a great way to get open source software development, while bettering my grasp on the literature. Equally importantly, I'm very interested in helping people learn and understand, writing tutorials for lay audiences will help improve my communication skills before grad school.
