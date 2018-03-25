# Multi-stream API

## Name and Contact Information
* **Name**: Dmitry Persiyanov
* **University**: [Moscow Institute of Physics and Technology](https://mipt.ru/english/)
* **Email**: persiyanov@phystech.edu
* **Github**: [persiyanov](https://github.com/persiyanov/)


## Pre-GSoC open source experience

Some pull requests I have contributed to:

* TensorFlow, 2017: [#7948](https://github.com/tensorflow/tensorflow/pull/7948). Fixed second derivative computation in softmax op.
* Gensim, 2018: [#1957](https://github.com/RaRe-Technologies/gensim/pull/1957). Made it possible to add new word vectors into `KeyedVectors` in a manual way.

Gensim issues currently I'm working on:

* New binary corpus format [#1697](https://github.com/RaRe-Technologies/gensim/issues/1697)
* Word2Bits benchmark [#1991](https://github.com/RaRe-Technologies/gensim/issues/1991)


## Abstract

Gensim is an NLP library which claims to be highly effective during training and produce linear performance growth with increasing the number of threads.

Currently, that is not true on machines with a large number of cores (>10) and large data files. The reason of this is that almost all Gensim models which support multithreaded training work in the following way. There is **single job producer** -- worker which reads the data and pushes the chunks into the job queue. Also, there are **many job consumers** -- workers which pull the chunks and update the model parameters in parallel.

The problem is that consumers'  code is optimized well, so this leads to **workers starvation** problem. Job producer just can't fill the queue at such a high pace. This is the case even using fastest `read the line, split it and yield` corpus iterator.

This problem could be solved by allowing users to pass `K` data streams (currently only single-stream == single job producer thread is supported), e.g. which point to `K` large files and use `K` job producers to fill the job queue.

The final goal of the project is:

1. Quantitatively measure current bottlenecks and performance of training with _single job producer_.
2. Develop _multi-stream_ versions and measure the performance boost.
3. Introduce _multi-stream API_ and integrate it into all Gensim models which support parallel training.
4. Write blog post + clear benchmark (e.g. in Jupyter Notebook) showing the advantages of _multi-stream API_ for powerful machines.

## Technical Details

From user's point of view, _multistream API_ will be implemented as `input_streams` parameter (of which the current parameter `corpus` stream is a special case, `input_streams = [corpus]`).

Implementation will be different for smth2vec models (word2vec, doc2vec, fastText) and for LDA (only these models are implemented to work in parallel). The former requires creating many producer threads [here](https://github.com/RaRe-Technologies/gensim/blob/develop/gensim/models/base_any2vec.py#L216) instead of one. The latter, LDA, requires the same and pushing data chunks in parallel [here](https://github.com/RaRe-Technologies/gensim/blob/develop/gensim/models/ldamulticore.py#L250).

All the code changes must be thoroughly benchmarked. I propose to use the whole EN Wikipedia (large) and some medium-sized dataset. Performance measuring code must be written. I plan to set up performance evaluation environment during first weeks.

In the evaluation, there are some degrees of freedom:

* Dataset size (medium, large)
* Number of cores on the machine
* Number of workers
* Model parameters (CPU intensive / not-intensive, e.g. word2vec with large / small window size).
* Corpus iterator speed (w/ or w/o CPU-bound preprocessing)

Metrics to improve:

* Number of words per sec.
* Average/median input queue size
* Total training time

### High-level timeline:
* Write benchmarking/evaluation code (two datasets, medium and large, whole EN Wikipedia & part of it for example).
* Measure _single stream_ performance with current smth2vec (word2vec, doc2vec, fastText) models.
* Write _multiple stream_ versions of these models (basically, create many producer threads [here](https://github.com/RaRe-Technologies/gensim/blob/develop/gensim/models/base_any2vec.py#L216) instead of one).
* Evaluate performance gain.
* Introduce _develop branch ready_ multi-stream API and merge it to develop branch.
* Evaluate _single stream_ LDA model.
* Write _multiple stream_ version & evaluate it.
* Introduce _develop branch ready_ multi-stream API and merge it to develop branch.
* Refactor current `AuthorTopicModel` and `LdaSeqModel` to support multistream API.
* Write a final report.

During all the phases I will report my progress in Gensim blog: https://rare-technologies.com/blog/.

## Timeline

### Community Bonding Period (April 23 -- May 13)

During community bonding:

* Continue working on Gensim issues I'm currently involved in -- [#1697](https://github.com/RaRe-Technologies/gensim/issues/1697) and [#1991](https://github.com/RaRe-Technologies/gensim/issues/1991). 
* Become more familiar with the codebase, especially with `base_any2vec.py, doc2vec.py, word2vec.py, fasttext.py, ldamulticore.py` and their Cython parts.
* Participate in mailing list / gitter.
* Explore the ways to use Cython in order to optimize _multistream API_.

### Week 1 (May 14 -- May 20)

* Set up evaluation environment, write benchmarking code.
* Start benchmarking word2vec, doc2vec, fastText models.

### Week 2 (May 21 -- May 27)

* Benchmarks for word2vec, doc2vec, fastText models.
* Start implementing _multistream API_ for word2vec, doc2vec, fastText models.
* Evaluate performance gain.

### Week 3 (May 28 -- June 3)
* Optimizing the basic _multistream_ version.
* Implementing full support of _multistream_ mode for word2vec, doc2vec, fastText.
* Writing tests & documentation.

### Week 4 (June 4 -- June 10)

* Addressing reviewers comments.
* Merge _multistream API_ for word2vec, doc2vec, fastText.

### Week 5 (June 11 -- June 17) (evaluation)

* Write Jupyter Notebook with clear examples of _multistream API_ on word2vec.
* Benchmark _single stream_ LDA model.

### Week 6 (June 18 -- June 24)

* Basic version of _multistream_ LDA.
* Evaluate the performance gain.
* Start optimizing the basic version.

### Week 7 (June 25 -- July 1)

* Finish optimizing _multistream_ LDA.
* Evaluate the performance gain.
* Writing tests & documentation.


### Week 8 (July 2 -- July 8)

* Addressing reviewers comments on _multistream_ LDA.
* Merge it to develop.

### Week 9 (July 9 -- July 15) (evaluation)

* Write Jupyter Notebook showing _multistream_ LDA performance & usage.
* Blog post about new features.

### Week 10 (July 16 -- July 22)

* Refactoring `AuthorTopicModel` in order to support _multistream API_.
* Refactoring `LdaSeqModel` in order to support _multistream API_.

### Week 11 (July 23 -- July 29)

* Addressing reviewers comments.
* Merge refactored `AuthorTopicModel` and `LdaSeqModel` into develop.

### Week 12 (July 30 -- August 5)

Cleaning the house:

* Fix bugs if any.
* Fix docstrings.
* Write more tests.

### Last week (August 6 -- August 12) (evaluation + submitting the code)

* Write final report -- blog post about the project and results.

## More about my experience

I am a 5th-year student at Moscow Institute of Physics and Technology, Department of Computer Science. Currently pursuing Master’s Degree in Computer Science and my professional interests are NLP, Deep Learning, Reinforcement Learning.

My [bachelor’s thesis](https://github.com/persiyanov/diploma-rl/blob/master/final-diploma/paper.pdf) was about applying RL to chit-chat neural conversational models in order to tune them for auxiliary tasks, such as reducing offensive replies or adapting conversation style (e.g. talk like technical support). In my master’s thesis, I continue to set up experiments in this area, tackling similar problems, such as maximizing user sentiment or maximizing engagement. I was invited to write a guest [article](https://blog.statsbot.co/chatbots-machine-learning-e83698b1a91e) about chit-chat models at Stats & Bots Medium Blog.

Also, I've been working for 1.5 years at the largest Russian IT company "Yandex", as a software engineer in personal assistant “Alice” team (it's Siri / Google Assistant in Russian). Here, my responsibilities are improving quality of our core NLU (natural language understanding) module; designing & implementing new scenarios; provide best NLU recognition for “Alice” on Windows; improving quality of anaphora resolution module. In order to be able to participate at GSoC full-time, I plan to take 3 months vacation.

My primary programming language is Python (~ 3 years of experience), while I also know C++ and a bit of Java, Scala, Ruby. I have some open-source experience which includes my own project “Skip-thought TF” ([https://github.com/persiyanov/skip-thought-tf](https://github.com/persiyanov/skip-thought-tf)) and some contributions to Scrapy and TensorFlow ([https://showmyprs.com/user/persiyanov](https://showmyprs.com/user/persiyanov)). Recently, my pull request was merged into gensim core ([https://github.com/RaRe-Technologies/gensim/pull/1957](https://github.com/RaRe-Technologies/gensim/pull/1957)).

## Why me?

It’s difficult to dive in into a project for open-source newbies because of lack of mentoring or feedback. GSoC provides these things by design. Also, participating in GSoC allows me to impact the world by contributing to the project, Gensim, which is used by thousands of developers in their production services. I have relevant experience and skills, so it is possible for me to succeed in this project.

## Acknowledgements

* Thanks @menshikh-iv for fruitful discussions.

## References

1. https://github.com/RaRe-Technologies/gensim/wiki/GSOC-2018-Guide
2. https://github.com/RaRe-Technologies/gensim/wiki/GSoC-2018-project-ideas
3. https://github.com/RaRe-Technologies/gensim/issues/336
4. https://github.com/RaRe-Technologies/gensim/issues/532
5. https://rare-technologies.com/machine-learning-hardware-benchmarks/
