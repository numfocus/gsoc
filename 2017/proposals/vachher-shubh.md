# Performance improvements in Gensim and current FastText implementation

## Introduction

Gensim has a history of being a library that caters to the pythonistas of the world; but in all this, it is also all about getting the job it does, done efficiently[1]. The results of this can be noted by looking at the ever increasing number of citations[2] that gensim gets from researchers around the world. This being the case, python does have its downsides when it comes code runtime efficiency.

Until now, gensim has been able to tackle this very well, in fact providing solutions that are verifiably faster than their counterpart C implementations!

My use of gensim for the last few months has been its powerful unsupervised word embedding implementation in word2vec and also to the very useful Phraser module as an improvement to the word2vec inputs and outputs. Thus, taking on this topic is best suited to me; providing me with a much needed platform to be able to help other researchers in their work as word2vec has helped me in mine.

Building over word2vec in recent times is the FastText word embedding implementation by researchers at Facebook. Considering the massive user base and use cases of the word2vec implementation, it is very likely that a supervised learning solution like FastText will be a welcome addition to the gensim library. This will provide people with an end to end solution to word embedding applications and it has been shown, will be faster to train alternative rather than word2vec followed by supervised learning.

## Technical Details


A very helpful tutorial on how Radim Řehůřek[3] took the implementation of word2vec in python and made it faster is available to us here[1]. An approach similar in method is something I hope to follow for the major planned chunk of my work : Speedup of the FastText algorithm and integration in gensim.

The python implementation of the FastText algorithm is available here[4]

The idea is to use this implementation as a fallback from the Cython implementation that I am planning to code. The Cython implementation will take a look at the bottlenecks in FastText python and speed them up. Cython speeds up programs using tricks that I plan to use like 
- Defining C types for all used variables
- Performing efficient indexing using : np.ndarray[np.int_t, ndim=2] array_name
- @cython.boundscheck(False)  # Perform important tests before using this
- @cython.wraparound(False)  # Perform important tests before using this

After this, and methods defined in [1] we will know we can move on when we have a single core implementation of FastText that has speeds comparable to the speeds of FastText writted in native C++.

This will be followed by trying to effectively use multicore system architecture and then followed by GPU architectures. For the multicore implementations I plan to try out the multithreading library for the Cython code and multprocessing for in case there is a fallback to Python implementation. 

Since we have a neural network, implementing GPU code using existing architecture optimized for GPU like TensorFlow, should provide much appreciated boosts in training time. Although, since the network we are implementing is shallow, I definitely plan to run benchmarks on if a GPU implementation of such will provide a useful decrease to training and test time.


## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**


- A blog will be maintained on wordpress for weekly reports of the work that is done with gensim over the summer! It will allow for faster suggestions as well as feedback from the community due to its reader friendly format of being written.

- Implement a Numba implementation of all computation intensive methods of FastText python to gauge the kind of speed ups we might expect. Numba speedups are closely correlated to speedups from effective Cython implementations [5].

- Profiling the FastText module. In fact, a lot of this work will be underway while figuring out the methods that will benefit the most from a Numba speedup.

- Profiling of the Phraser module will be done before the start of the official coding period. This will allow a directed approach to Cythonizing of Phraser module.

### May 29th - June 9th (2 weeks)


- Cythonize Phraser and other smaller, requested modules of the gensim library. This will give us a much needed idea of the challenges we might face when Cythonizing FastText so that we are able to discuss them beforehand.

### June 12th - June 23rd (2 weeks)


- Continue any more cythonizing requests

- Start of refactoring of current fastText implementation as in [4]. I do plan to take ideas from the implementation provided here[8] but the problem there is that it is not an implementation for supervised learning as FastText is.

- Thus by the end of Phase 1 we will have a gensim implementation of FastText that we can cleanly work on to optimise using Cython in Phase 2.

### June 26th - July 7th, **Phase 2 begins** (2 weeks)


- Profiling of Python implementation got at the end of Phase 1. Should be quick due to earlier profiling work done.
- Cythonizing of Fasttext module implementation in gensim
- Profiling of FastText done before and understanding of problems faced during Cythonizing Phraser will push this to fair completion in two weeks time allotted.

### July 10th - July 14th (1 week)

- We can now compare Cython FastText performance versus plain Python implementation and also the C implementation of FastText from Facebook.
- This step is important as it will give us a direct comparison of run times of each step and insight into further optimizations possible with our implementation.
- At the end of the week we must have a FastText implementation that has a single core fit and predict speed comparable to c implementation.

### July 17th - July 21th, **End of Phase 2** (1 week)

- Begin implementation of FastText over multiple cores. First, a multiprocessing implementation will be provided to the plain python code. This is just as a start to figure the work to be done with Cython over Phase 3.
- multiprocessing is needed for python as the GIL disallows training over multiple cores. This drastically reduces training speed in case of python code.

### July 24th - July 28th, **Begin of Phase 3** (1 week)

- Multithreading implementation begins for Cython code
- Being low level C, the GIL of python will not affect the Cython code and multithreading module can be used.

### July 31st - August 11th (2 weeks)


- Plan to rewrite training part of neural network used in FastText in TensorFlow or if possible even Theano.
- This will allow use of GPU for training that should give us good expected speedups 

### August 7th - August 18th (2 weeks)

- Documentation of all code will be started here.
- Adding of tutorials with all possible cases of having multiple cores, vs c compilers, vs GPU etc
- An IPython notebook to show implementation of code for a given case

### August 21st - August 25th, **Final Week**


- Scrubbing up of documentation and wrapping up ipython notebook tutorial.
- Any requested documentation, etc. Cleaning up of files to prepare for clean merge.
- Answer any community questions related to any parts of implementation

### August 28th - August 29th, **Submit final work**

- Merge and time to celebrate.

## Future works

More important than the coding period is time that follows, when people use the code and provide feedback. This part will entail constant factoring in of community feedback from people using the module. This will be quite possible as I have another semester(6 months after GSoC) of courses to complete my undergraduate degree.

Further optimizations of other models in the gensim library will be quite easier to code up due to the exposure gained during the GSoC period. I will try to continue to optimize the other models available in the gensim library looking at the ones most used and the ones that will benefit the most from optimizations. This second part will be easier to gauge due to the extensive time spent with gensim codebase during GSoC.

## Development Experience


I have worked extensively on implementing the gensim word2vec module; most importantly during an internship with a startup. In fact the word2vec implementation I provided caused a complete revamp of the startup’s backend(tf-idf) and resulted in much improved results and better user feedback.

I have worked for almost 2 years on distributed systems and big data, constantly looking to optimize routines and most importantly looking to be able to run them concurrently. This has saved me 176 hours of coding time, earlier spent waiting for results! I hope to provide the same experience to other users of gensim and for other programs in general.

I have also a strong philosophy of writing open source code. In fact, the work done on my last summer internship has all been put up by me on my github account for other students to refer to.

## Other Experiences

Most notably, I have worked on implementing machine learning solutions to biological and social problems, the first with professor Raviprasad Aduri on campus and the second as a pet project of my own. I have interned at a very reputed electronics research facility in India - CEERI, implementing a machine learning solution to a computer vision problem useful to industry.

I have attached my CV for reference[6]

## Why this project?

The user base of gensim is very strong! In fact, in the past 10 months there have been 4 workshops organized on campus by people who eventually end up introducing gensim’s word2vec; including one that was conducted by Lev[7] in person!

My idea to implement this project is also driven by selfish interest. As an ardent user of gensim’s word2vec, this implementation/ enhancement would add value to me personally as an individual user. It would be quite a fruitful experience to learn and to see all this work be used and evaluated by others in the academia and more. Finally, I would most definitely need to use FastText in one of my existing projects and I would love to use it right from here at gensim!

## Appendix

[1] (https://rare-technologies.com/word2vec-in-python-part-two-optimizing)
[2] (https://scholar.google.com/citations?view_op=view_citation&hl=en&user=9vG_kV0AAAAJ&citation_for_view=9vG_kV0AAAAJ:NaGl4SEjCO4C)
[3] (https://radimrehurek.com/about/)
[4] (https://github.com/giacbrd/ShallowLearn/blob/master/shallowlearn/models.py#L74)
[5] (https://jakevdp.github.io/blog/2013/06/15/numba-vs-cython-take-2/)
[6] (https://drive.google.com/file/d/0ByKpnyupKVlpeW5aY2p4NFVEMlk/view?usp=sharing)
[7] (https://rare-technologies.com/author/lev/)
[8] (https://github.com/RaRe-Technologies/gensim/pull/1153)

