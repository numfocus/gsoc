# Online NNMF​

## Abstract

The goal of project is to make an efficient parallel version of online NNMF algorithm. This algorithm is widely used in recommender systems. The implementation is based on fast Cython code, with BLAS code snippets and also efficient parallelization techniques similar to ones used in LibFM implementation of this algorithm.

## Technical Details

1. Read related articles, understand problem statement, main problems with parallel implementation and possible solutions. The basic article is LibFM parallel implementation of NNMF: http://www.csie.ntu.edu.tw/~cjlin/papers/libmf/libmf_journal.pdf

2. Implement initial sequential version of algorithm using Python + Cython,
measure its performance. Then make the implementation parallel.

3. Test algorithm with thinking of corner cases, also corner cases in performance.
Make sure that sequential and parallel versions output similar results.

4. Test on datasets​ . Proposed benchmark datasets are Netflix and MovieLens
datasets. Measure how performance depends on number of cores used.

5. Write tutorial including examples of usage, necessary code snippets,
performance measurements.

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

Read related articles and check LibFM implementation; find and understand how to work with benchmark datasets

### May 29th - June 3rd

Implement sequential version of NNMF using Python + Cython

### June 5th - June 9th

Implement sequential version of NNMF using Python + Cython

### June 12th - June 16th

Test the basic version of NNMF on datasets, check correctness and
measure speed performance

### June 19th - June 23th, **End of Phase 1**

Code parallel version of algorithm

### June 26 - June 30th, **Begin of Phase 2**

Code parallel version of algorithm

### July 3rd - July 7th

Test correctness of parallel implementation

### July 10th - July 14th

Add BLAS snippets to algorithm

### July 17th - July 21th, **End of Phase 2**

Add BLAS snippets to algorithm

### July 24th - July 28th, **Begin of Phase 3**

Test correctness of implementation with BLAS

### July 31st - August 4th

Test advanced algorithm version on datasets

### August 7th - August 11th

Measure performance on benchmark datasets, compare
performance with sequential version

### August 14th - August 18th

Write documentation, include code snippets, test results and all
necessary plots

### August 21st - August 25th, **Final Week**

Write documentation, include code snippets, test results and all
necessary plots

### August 28th - August 29th, **Submit final work**

## Future works

Finding bottlenecks in algorithm implementation and future optimizations

## Development Experience

In detail, I have experience in C++ including STL, MPI, OpenMP, CUDA. Also I have experience with Python including such extensions as Cython, Scipy, Numpy, deep learning framework Keras, machine learning library Scikit-Learn and also Gensim. Have experience in accelerating industry code initially written in Python + Numpy by rewriting it to Cython. I know typical parallel algorithms terms (acceleration, efficiency etc.) and methods (types of parallelization).

## Other Experiences

I have experience of using Gensim library for research project, in detail, I used doc2vec to creating vectors for messages in social networks and then classify them into positive and negative opininons about particular places in the city.
Link to contribution to Gensim: https://github.com/RaRe-Technologies/gensim/pull/1239

## Why this project?

I am fond of solving non-trivial programming tasks and implementing state-of-the art algorithms from articles, especially connected with machine learning. This project is focused on modern algorithm, widely used in recommender systems and other applications. The coding part of project seems very interesting to me because it requires parallel implementation which always gives a lot of points to think of and opportunity to solve interesting sub-tasks in the algorithm.

## Appendix

LibFM implementation of Online NNMF algorithm: http://www.csie.ntu.edu.tw/~cjlin/papers/libmf/libmf_journal.pdf
