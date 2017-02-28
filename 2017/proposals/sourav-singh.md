# PyMC3: Implement non-parameteric Bayesian Methods

## Abstract
[Bayes Theorem](https://en.wikipedia.org/wiki/Bayes'_theorem) is one of the theorems used in Machine Learning where you place a prior on `m`,`P(m)` and using the data, compute `P(m|D)`. This approach works for most Machine Learning problems, except when you have following example-

1) You are trying to model crop yield as a function of rainfall, amount of sunshine, amount of pesticides and fertilizers. You believe the problem is a non-linear one, so a polynomial based approach for modelling is decided. The following questions arise-

  - How do you pick `P(m)`, the prior over the order of the polynomial?
  - What is the kind of interaction between the input variables?
  - What kind of relationship is it- Cubic, Quadratic?

For these kinds of problems, non-parameteric Bayesian methods are powerful. The non-parameteric Bayesian methods are powerful since they can be derived by starting with a finite parameter and taking the limit of parameters upto infinity. Non-parameteric methods can be used to automatically infer models of adequate size and complexity without thinking about Bayesian model comparison.

The project aims to implement these models into PyMC3 to extend the functionality and make it more useful for Bayesian Statistics and Inference. 

## Technical Details

Non-parameteric Bayesian Method are described in brief in the class in MIT by Lorenzo Rosasco here [\[1\]](http://www.mit.edu/~9.520/spring11/slides/class18_dirichelet.pdf).

The various non-parameteric Bayesian methods are-

1) Dirichlet process
2) Dirichlet process mixtures.
3) Chinese Restaurant Process, a subset of Dirichlet process.
4) Gaussian Processes
5) Hierarchial Dirichlet Processes 
6) Polya Trees
7) Indian Buffet Processes
8) Dirichlet Diffusion Trees
9) Urn Model
10) Infinite Hidden Markov Models

Of these, The Gaussian Process model is implemented in PyMC3, it needs improvements and needs to be automated.
The other Bayesian methods needs to be added into the PyMC3 codebase to make it a robust toolbox for Bayesian Statistics.

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

Familiarize with the Core Developers of PyMC3, take a look at the Gaussian process model and discuss with mentors on the approach for the project. Fix some of the issues in PyMC3 code. Write a detailed report on the same.

### May 29th - June 3rd
Start work on improving Gaussian processes and Dirichlet Diffusion Trees.
Write detailed reports.

### June 5th - June 9th
- Finish improvements to Gaussian processes with updated tests and examples.
- Add tests for the implementation of Dirichlet Diffusion Trees, add API docs, Examples and 
  merge it into the dev branch
- Write a detailed report explaining the project status.

### June 12th - June 16th
Start work on Chinese Restaurant Processes and Hierarchial Dirichlet processes. Since Chinese Restaurant Process is closely connected to Dirichlet processes, the implementation should be finished early. Add corresponding tests for the processes and write a detailed report on the project. 

### June 19th - June 23th, **End of Phase 1**
- Add API documentation along with Detailed Examples for Chinese Restaurant Processes and 
  Hierarchial Dirichlet Processes.
- Push the code to dev branch of PyMC3.
- Discuss the project status with the mentors.
- Write a detailed report.
### June 26 - June 30th, **Begin of Phase 2**
Start work on Urn Models along with Tests and detailed examples for the same.
Write Detailed report on the same.

### July 3rd - July 7th
- Merge Urn model into PyMC3 dev branch.
- Start implementation of Dirichlet Processes.
- Write Detailed report on the project

### July 10th - July 14th
Finish Implementation of Dirichlet processes with tests and examples.
Write detailed report.

### July 17th - July 21th, **End of Phase 2**
Merge the changes made so far into dev branch of PyMC3 and discuss with mentors on the project status.

### July 24th - July 28th, **Begin of Phase 3**

Start implementation of Dirichlet Mixture models.

### July 31st - August 4th
- Finish implementation of Dirichlet Mixture Models.
- Write tests for the models and detailed API docs and Examples.
- Write report on the progress so far.

### August 7th - August 11th
Start implementation of Polya Trees and Infinite Hidden Markov Models.
Since the Polya trees involves Random splitting probabilities, it could be difficult 
to finish the Polya Tree model in a week. Write detailed reports.

### August 14th - August 18th
- Finish Infinite HMM with tests and detailed examples and API docs..
- Complete work on Polya Trees with feedback from Mentors.
- Write detailed reports on progress.

### August 21st - August 25th, **Final Week**
Finish work on Polya Trees and Write detailed API docs
examples illustrating working behind Polya Trees. Merge all
code into dev branch and write detailed report.

### August 28th - August 29th, **Submit final work**

## Future works
The future work with PyMC3 would involve adding benchmarks for PyMC3 to better detect regressions, Work on additional sampling methods like Ensemble Sampling and if possible, Adding support for PyTorch/Tensorflow as a backend for PyMC3. 

## Development Experience
My first open-source development experience started with a dev-sprint where I hacked on [Click](http://click.pocoo.org/5/), adding documentation. I later went on to contribute to a Docker project, scikit-image, scipy, NumPy, scikit-learn, PyPA Warehouse project, github3.py, BioPython, Shogun ML among many others. I am largely interested in the field of Bioinformatics and Neuroinformatics and have given a talk on Nipype at PyCon India 2015 and A small workshop on BioPython at SciPy India 2015.  


## Other Experiences

I am a third-year undergraduate student in Computer Engineering from India currently enrolled under University of Pune, India. I am immensely interested in the field of Machine Learning and its applications in the field of Bioinformatics and Neuroinformatics. I have completed courses in the field of Genomic Data Science, which was conducted by John Hopkins University. I am linking my [CV](https://drive.google.com/file/d/0BxUjocRT833zSWFyTjNaMF9IVTQ/view?usp=sharing) for a detailed info.

## Why this project?
As a self-learning student of Bioinformatics and Neuroinformatics, I am interested in the field of Bayesian Statistics and how it can be used to solve complex biological problems like motif discovery, simultaneous multiple sequence alignment, protein structure prediction, Seizure Prediction using Bayesian Regression etc. Contributing to the project will allow me to appreciate the beauty of Bayesian Statistics. Working on the project will also allow me to gain an understanding of the project and work on the features it truly needs. It would also be a great moment for me to be a part of the project which is used by Statisticians and Machine Learning Engineers all over the world. 
