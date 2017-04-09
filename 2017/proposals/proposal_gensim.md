
### **GSoC ‘17 Project Proposal**
### **Online Non Negative Factorisation**
##### Krishna Kant Singh
##### March 2017

### **Basic Information**
#### **Name and Contact Info**
 KrishnaKant Singh(Kris)
 Email:cs15mtech11007@iith.ac.in
 IRC Nickname: kris1
 Phone +918895952597
 Github/Gitter: kris-singh
Video-chat: www.appear.in/kris-singh
#### **University and Current Enrollment**
University : Indian Institute of Technology, Hyderabad
Field of Study: Computer Science(2018)
#### **Coding Skills**
Python : Fluent, Good knowledge of OOPS
C++: Fluent, Good knowledge of Meta Programing, OOPS, Policy
based design through templates
Cython: Beginner but can come up to speed in quick time.
#### **Development Environment**
Ubuntu/Linux variants
Ides: Eclipse, PyCharm, Sublime
Good Familiarity with conda/venv
Version Control: Fluent with Git
### **Technical Details**
#### **Background**
NMF finds usage in the document clustering and recommendation system among
many other areas. Many algorithms like word2vec and Probabilistic LSI can also
be derived/ thought of as a form of NMF. But the most useful/immediate aspect
for gensim is in the text mining domain wherein it is used as a tool to cluster
the document into different categories.
There exist a couple of standard ways of solving the given problem. Projected
Gradient Descent Model and HALS(Hierarchical Alternating Least Square). All
these methods can shown to be derived from the Block Coordinate Descent
Method. We give a brief overview of these algorithms.

**Projected Gradient Descent**
Tries to convert the non-convex objective to a convex objective using alternating
minimization. This is achieved by minimising one parameter at a time keeping
the other constant.
The gradient is computed with respect to F at F^k keeping G constant. G(t)
indicates the value of G at the t iteration. We do the same thing for updating
the G matrix but keeping F as a constant.
Projected Gradient Descent is not guaranteed to converge and also if it converges it does so very slowly O(Tndr + TKJr2(d + n)). Where T is the number
of iterations, K is the average number of PGD iterations for updating F or G in
one round, and J is the average number of trials needed for implementing the
Armijo rule for updating the step size.

**Hierarchical Alternating Least Square**
Hierarchical ALS makes use of simple insight. When the Dimension of the input
matrix X is very high we can safely assume that it low rank. And we do not
need to process all the entries of the matrix in order to estimate the factor the
matrix into F and G. We can consider alternating factorization of much smaller
dimension. Which leads to faster solution times.

**Existing Solutions**
1. Scikit Learn at present has support for both Projected Gradient Descent[3] and
the HALS [4] Algorithm. Though both the algorithms are neither online nor
distributive.For initialisation of the Factor matrices, Scikit learn uses the aforementioned NNSVD, NNSVDA, NNSVDVar. Scikit doesn’t provide a way for choosing the number of components in the factors.
Con: Neither online nor out of core
2. Libmf[5]has implementations for the Hogwild Algorithm[5] and the DSGD algorithm in c++.
Con: Hogwild suffers from duplicates updates while Dsgd resolves the problem but is much harder to implement
3.This includes c++ parallel implementation of the HALS, Multiplicative Updates, Block Pivot Partition algorithm.
Pros: Is faster that MU. Has good
results based on speed
Cons: None really that significant
4. The beta Divergence Multiplicative update library[7] provides both an online
manner(batch nmf) and out of core computation(theano based). The paper
describes multiplicative updates that can be done in an online fashion for various
kind of update schemes. Also it multiplicative updates to a general loss function
the Beta divergence. Where β = 2 is equivalent to euclidean loss β = 1 is
equivalent to generalised kl divergence.
Con: Multiplicative updates have been known to slow for large problems. We have to see what batch sizes provide good results wrt speed and accuray.


### Proposed API
The Api is inspired from both BetaNMF and the Sklearn NMF implmentation.
We do not show here the API for the Synchronisation module. We go with
sklearn implementation so that integrates with the sklearn API’s seamlessly.
This is no way a complete API
#### Base Class
```python
class NMF():
    """
    Non Negative matrix Factorisation Base class
    Find two non-negative matrices (W, H) whose
    product approximates the non-negative matrix X.
    This factorization can be used for example for
    dimensionality reduction, source separation
    for topic extraction.
    || X - WH ||_F^2 + alpha ||W||_1 + alpha || H||_1
    Where F indicates the forbenius norm
    || ||_1 indicates the L1 norm
    """
    def __init__(n_components, init, solver, tol, max_iter,
                 random_state):
    """
    Parameters
    ---------------------
    n_components : int or None
        Number of components,
        if n_components is not set all features
        are kept.
    init :  Method used to initialize the procedure.
        This could be one of the following
        'random' | 'nndsvd' |  'nndsvda'
        | 'nndsvdar' | 'custom'
    solver : 'cd' | 'mu' | 'musag'| 'hal' | ...
        Numerical solver to use. This could projected gradient,
        co-ordinate descent, multiplictive update,
        multiplicative updates
        with sag etc.
    tol : double, default: 1e-4
        Tolerance value used in stopping conditions.
    max_iter : integer, default: 200
        Number of iterations to compute.
    random_state : integer seed, RandomState instance,
                    or None (default)
        Random number generator seed control.
    alpha : double, default: 0.
        Regularisation constant
    shuffle : boolean, default: False
        If true, randomize the order of coordinates
        in the CD solver. Shuffle the outputs
    Examples
    --------
    >>> import numpy as np
    >>> X = np.array([[1,1], [2, 1], [3, 1.2],
                     [4, 1], [5, 0.8], [6, 1]])
    >>> from gensim import NMF
    >>> model = NMF(n_components=2, init='random',
        random_state=0)
    >>> model.fit(X)
    NMF(alpha=0.0, beta=1, eta=0.1, init='random',
        l1_ratio=0.0, max_iter=200, n_components=2,
        nls_max_iter=2000, random_state=0,
        shuffle=False, solver='cd', sparseness=None,
        tol=0.0001, verbose=0)
    >>> model.components_
    array([[ 2.09783018,  0.30560234],
           [ 2.13443044,  2.13171694]])
    >>> model.reconstruction_err_
    0.00115993...
    """
    pass

    def fit(self, data, **params):
    """
    This module is work horse of the whole program. Here we
    actually call the solvers using parameters.

    Parameters
    ----------
    X: {array-like, sparse matrix}, shape (n_samples, n_features)
            Data matrix to be decomposed
    Returns
    -------
    self
    """
    pass

    def compile_theano(self):
    """
        This module compiles all theano based function
        mostly useful for multiplicative updates and FAST HALS
    Returns
    ---------
    self
    """
    pass

    def compute_grad(self):
    """This function is responsible for computing the gradients
       we use Theano to compute gradients for us.
    Returns
    -------
    self
    """
    pass

    def transform(self, data):
    """Project data X on the basis W
    Parameters
    ----------
    X : array
        The input data
    Returns
    -------
    H : array
        Activations
    """
    pass

    def prepare_batch(self, data):
    """Useful for online learning
    scheme for NMF. This module creates batches
    from the given data
    Returns
    --------------
    batch: return a subset of the data
    """
    pass
```
### **Deliverables**
#### **Primary Goals**
Implementation of a Naive Parallel algorithm and High-Performance
parallel Algorithm Defined in Kanana et al and implemented in NMLF
Library.
1. Implementation of the Online NMF solver described in the Wang et
al with Naive Parallel Algorithm.
2. FAST HALS Solver implementation the Nmlf Library that uses the
Naive the parallel algorithm for distribution among cores.
3. Online Multiplicative Update Solver Implementation using cython/theano/tensorfow using BaseNMF library
#### **Secondary Goal**
1. Add a real live Demo example of using online NMF for text mining.
Possible in recommendation systems
2. Complete the Dynamic topic Model Work PR840.
#### **WishList**
1. Implementation of the synchronisation module described in LIBMF
for solving the Distributed SGD.
### **Timeline**
I am available for the full 3 months barring a single weekend where I am busy
with some prior commitments.
I can dedicate work for long hours and give 40hrs/ week would not be challenging. I have prepared the time line so that I can finish ground work early and
actually concentrate on implementing the solution in the actual GSoC period
without any snags.
All dates mentioned here are weeks- starting from Monday and ending on
Sunday.
**4April-1May**
Finish exams and other curriculum related work and projects.
**4May-7May**
+ Introduce myself to the community and discuss at length the pros and cons of
the solution proposed.

**8May-14May**
+ Understand the code of LibMF HALS algorithm
+ Understand the Pro’s and cons for implementing the proposed solution
using TensorFlow and Theano
+ Read the Dynamic Topic Models Blei Paper.

**15May-21May**
+ Understand the High-Performance Parallel Nmf method from Kanan papers.
+ Get comfortable with theano/tensorflow for implementing nmf code. I
already have good knowledge of TensorFlow for implementing the neural
network so will not be a problem
+ Start Work on PR840 Get rid of all c/c++ coding styles

**22May-29May**
+ Understand the FastHAlS implementation code the authors provide.
+ Benchmark code against scikit code.
+ Convert sslm class to cython PR840

**22May-28May**
+ Finish Reading upon the Code of FAST HALS.
+ Read upong BaseNMF library code for Multiplicative Updates
+ Submit PR for Document Influence Model
**This almost finishes PR840**

**29May-4June(Week 1)**
+ Discuss about possible implementations details of HALS with open mpi
using Naive Parallel. Algorithm/HPC(whichever is picked by mentors) for
now I assume we go with Naive initially.
+ Finish up on MPI Readings.
+ Start implementing the FastHALS code with cython/theano/tensorflow/

**5June-11June(Week 2)**
+ Start work on Documentation.
+ Finish Work on FAST HALS
+ Start writing Test for FAST HALS
**12June-18June(Week 3)**
+ Read the code for FAST HALS+ Naive Parallel Algorithm from NMF
Library.
+ Start implementation of FAST HALS+ Naive Parallel.
+ Finish Writing Test for FAST HALS BenchMark Code against LibMF
library.

**19June-25 June(Week 4)**
+ Finish the work FastHALS(Cython) + MPI + Naive Parallel.
+ Write Basic Test cases for the Fast HALS + Parallel Algorithm.
+ Start to work on online Multiplicative Updates algorithm. (MU+SG)
** Milestone Reached Primary Goal 1 point 1.**

**26June-2 July(Week 5)**
+ Clean up all jobs for Mid-term evaluation.
+ Write test checking the parallel implementation and Benchmarks.
+ Finish Work on MU+SG algorithm
+ Buffer week for Mid-term evaluation, make the work presentable.

**3 July-9 July(Week 6)**
+ Start work on writing Blog Post. Explain the use of both Fast HALS+MU.
Show real Document clustering application using the same algorithms.
+ Write Test for MU+SG algorithm. Benchmark against FastHALS code.
+ Start ground work on implementation of online version of NMF.(Wrt
Fast HAls online implementation) This could be implementation based
on Kanana et al paper [8] or going with LibMf parallel implementation [5]
+ Finish online MU+SAG implementation
**10-16 July(Week 7)**
+ Discussion on how to implement Online NMF(Show Basic Algorithm).
+ Start work on online NMF implementation + integration with Naive/
HPC environment kanan et al paper.
+ Start work on writing on ipynb showing online NMF
**17- 23July(Week 8)**
+ Online NMF continues
+ Start on Trivial Test for online NMF
+ Finish Any remaining Documentation
**24- 30July(Week 9)**
+ Finish Online NMF.
+ Write Tests to check. parallelism and benchmark.
+ Start working on MU+ SAG algorithm.
**Primary Goal Achieved point 2.**
**Start with Secondary Goals.**

**31- 6August(Week 10)(2 Phase Evaluation)**
+ Buffer Week.
+ Clean up Code finish pending works.
+ Finish work MU + SAG algorithms
+ Start writing tests for MU+SAG.

**7- 13August(Week 11)**
+ Write Ipynb Demonstrating Uses of Online NMF.
+ Finish Writing Documentation for online NMF.
+ Finish Writing blog post discussing ONline NMF, Online Multiplicative
+ Updates. Show benchmarks for speed against sklearn.
**Primary Goal Achieved point 3.**

**14- 20 August(Week 12)**
+ Buffer Week
+ Commenting Code, doc string, passing pr reviews, docker container etc
**21- 27 August(Week 13) Wrap Up**
+ Finish Any work if remaining.
+ Do cosmetic changes to the code.
+ Prepare for final submission.

### About Me
I am a pre-final year student doing Master in Computer Science from Indian Institute of Technology India. I also have a internship experience at
Robert Bosch for 3 months working on Diabetic Retinopathy. I have doing machine learning for 2+ years now and have active in the open Source
community for about 6 months. I have contributed in PGMPY a graphical
model library in Python and also to Gensim a document modelling library
in python. Some of my projects include Implementing Recommendation System using ANLS(implemented from Scratch), Library for Contextual Multiarmed Bandits(using offline evaluation), Graph Kernel based learning for sub
graphs(Tentative submission at ECML-PKDD 2017), Implemented the reinforce
algorithm for attention mechanism in images for generating captions paper. I
have done a Deep Learning course(mostly followed Hinton Coursera), Optimisation course( Based on CMU optimisation Course). I have also done Andrew
Ng Coursera course and also Daphne koller Graphical Models Course. I very
motivated to apply what i have learnt till now and looking forward for great
summer with Gensim.
### Past Experinces with open Source
I have been contributior to PGMPY a probablistic graphical modelling library in python similar to pymc3 for long time. I have 4 requested merged and around 8 open PR's that I am working on or the issues have been resolved. Here is a list of some of my [PR's]
(https://github.com/pgmpy/pgmpy/pulls?utf8=%E2%9C%93&q=is%3Apr%20kris-singh)
Here is the list of issues I have [raised](https://github.com/pgmpy/pgmpy/issues/created_by/kris-singh)

I also have been contributing to MLPack a machine learning Library in C++
I have 1 merged and 3 open [PR's] (https://github.com/mlpack/mlpack/pulls/kris-singh) that are close to being merged.

### Why "me" for this project?
I am highly motivated person for this project as I have already learnt and implemented NALS techniques in python for my Movie Recommendation Project.
I have no prior commitments in the summer and no research work in the summer. Hence, I can fully concentrate on GsoC. I am already familiar with reading
research journals and implementing them. I have been contributing to PGMPY
for more than a 3 months now, hence I have good knowledge of contributing
to a opensource project based on python. I have used gensim word2vec model
extensively in my research work where I had to modify the internal implementation of the word2vec model so, I am also pretty comfortable with the gensim
code base. I really Love coding and actually code for fun also. So, putting in
40 hrs + every week is not a problem for me

### Refrences
1. W. Liu, J. Wang, H. Chen, and X. Ma, “Symbolic Model Checking APSL,”
2008 2nd IFIP/IEEE International Symposium on Theoretical Aspects of
Software Engineering, pp. 39–46, 2008.
2. A. Cichocki and A. H. Phan, “Fast local algorithms for large scale nonnegative matrix and tensor factorizations,” IEICE Transactions on Fundamentals
of Electronics, Communications and Computer Sciences, vol. E92-A, no. 3,
pp. 708–721, 2009.
3. C.-J. Lin, “Projected gradient methods for nonnegative matrix factorization.,” Neural computation, vol. 19, pp. 2756–2779, 2007.
4. C. Hsieh and I. Dhillon, “Fast coordinate descent methods with variable selection for non-negative matrix factorization,” Proceedings of the 17th ACM
SIGKDD . . ., pp. 1064–1072, 2011
5. W.-s. Chin, Y.-c. Juan, and C.-j. Lin, “LIBMF : A Library for Parallel
Matrix Factorization in Shared-memory Systems,” vol. 17, pp. 1–5, 2016
6. R. Gemulla, E. Nijkamp, P. J. Haas, and Y. Sismanis, “Large-scale matrix
factorization with distributed stochastic gradient descent,” Proceedings of
the 17th ACM SIGKDD international conference on Knowledge discovery
and data mining - KDD ’11, pp. 69–77, 2011
7. R. Serizel, S. Essid, and G. Richard, “Mini-batch stochastic approaches for
accelerated multiplicative updates in nonnegative matrix factorisation witheta-divergence,” IEEE International Workshop on Machine Learning for
Signal Processing (MLSP), no. 3, pp. 2–7, 2016
8. G. Tech and H. Park, “A High-Performance Parallel Algorithm for Nonnegative Matrix Factorization,” pp. 1–10, 2015.

