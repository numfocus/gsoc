# Distributed Word2vec on CPUs on multiple machines

## Abstract

Distributed computing allows to work with large corpuses, which are difficult to process on one machine, using a cluster. Gensim contains distributed implementations of several algorithms (distributed LSA, distributed LDA). The implementations use [Pyro4](http://pythonhosted.org/Pyro4/) for network communication and are fairly low-level.

Word2vec model can use many worker threads for fast training on multicore machines, but it doesn’t have distributed version for fast training on the cluster.

We can re-implement gensim word2vec algorithm in [Tensorflow](https://www.tensorflow.org/) in order to enable distributed computation. We can try [Spark](http://spark.apache.org/) for this task and compare the results. For visualization the model we can use Tenserflow special tool - Tensorboard UI. 

In my university project I used gensim and word2vec for oneclass classification task, so I can make tutorial from it.

## Technical Details

Word2vec consists of two model architectures: CBOW (predicts target words from context words) and Skip-gram (predicts context words from the target words). As these models are similar, I will be implementing both simultaneously. 

As we will use In TensorFlow, the following steps are required for successfully completed project:

### *Build the graphs for both models* 

... and re-implement function `train()` in class Word2Vec using [TensorFlow word2vec implementation](https://www.tensorflow.org/tutorials/word2vec) and [Gensim word2vec implementation](https://github.com/RaRe-Technologies/gensim/blob/develop/gensim/models/word2vec.py)

### *Distribute the graphs across the cluster*

TensorFlow cluster is defined using a `tf.train.ClusterSpec()` object. Cluster consists of “jobs”, each divided into lists of one or more “tasks”.  Each “task” will be run on a different machine and associated with a TensorFlow “server” (by creating `tf.train.Server`), which contains a “master” (for creating sessions), and a “worker” (for executing operations in the graph). There are two types of “jobs”:  `ps` (parameter server), which hosts nodes that store and update variables and `worker`, which is responsible for compute-intensive tasks (for this project each `worker` will be train the same model on its own mini-batches of data). 

TensorFlow has tools for different approaches to this structure of replicated training: in-graph/between-graph replication, asynchronous/synchronous training. I’m going to find out, which is the best for this project.

**Resourses:** [Distributed TensorFlow guide](https://www.tensorflow.org/deploy/distributed)

### *Visualize with Tensorboard*

**Resourses:**  [TensorBoard: Visualizing learning](https://www.tensorflow.org/get_started/summaries_and_tensorboard)

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

- Help with issues and bugs and anwser the questions on the [gensim mailing list](https://groups.google.com/forum/#!forum/gensim)

- Research what tool would be better for our project (TensorFlow or Spark) 

- Re-read the [paper](http://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf) by Mikolov et al. and gensim word2vec code to make sure everything is clear

- Communication with mentor and community
Make sure that I have everything for testing and I can create a cluster for it.

### May 29th - June 3rd

- Choose the tool for distribution 

- Benchmarking existing code. Check the Skip-gram model implementation and improve it if it will be necessary.  Start coding CBOW model (in Tenserflow it will be building the graph).

### June 5th - June 9th

- Continue creating the CBOW model (in Tenserflow it will be building the graph).

### June 12th - June 16th

- In Gensim word2vec algorithm was extended with additional functionality, that must be available in distributed version. This week I will integrate my code of both models with existing methods.

### June 19th - June 23th, **End of Phase 1**

- Finish basic (non-distributed) realization of CBOW and Skip-gram models, that can work with all existing word2vec methods. End of re-implementing word2vec algorithms. 

### June 26 - June 30th, **Begin of Phase 2**

- Test CBOW and Skip-gram models.
- Start working on distributed version: first of all, it would be necessary to determine the structure of parallel computing (what each node will do).

### July 3rd - July 7th

- Add functionality for creating the cluster.

### July 10th - July 14th

- Make Skip-gram model distributed, start making CBOW model distributed.

### July 17th - July 21th, **End of Phase 2**

- Finish coding distributed models. End of making word2vec distributed.

### July 24th - July 28th, **Begin of Phase 3**

- Test distributed word2vec with different numbers of machines in cluster. 

### July 31st - August 4th

- Add visualization with TensorBoard.

### August 7th - August 11th

- Make Jupyter Notebook with my university project as tutorial, start working on documentation for distributed word2vec.

### August 14th - August 18th

- Make documentation for distributed word2vec.

### August 21st - August 25th, **Final Week**

- Test all again, check documentation and tutorials.

### August 28th - August 29th, **Submit final work**

- Have PR merged.

## Future works

Distributed computing is actively developed now so I will be follow the news in it and add new features whenever possible. For example, in TensorFlow they are working now for easier way to set cluster specification than existing.

I will keep contributing to gensim with issues and PR and be active part of community. 

## Development and Academic Experience

I’m a 3rd year student of Moscow State University, [Faculty of Computation Mathematics and Cybernetics](https://cs.msu.ru/en). 

I’m specialized in Machine Learning and Data Mining. For my last research project in University (“Research models of vector representations of texts based on word2vec algorithms”) I learned and used different NLP algorithms. The main part of it is about word2vec and doc2vec (and we used gensim implementation), so I well know how exactly it work. 

In this year in University we study distributed systems and parallel processing of data and I worked on cluster. 

I’m new at open-source community but I really want to be part of it. 

## Why this project?

I want to improve my knowledge in deep learning and distributed computing, because nowadays it’s very important skills. I choose this project because it’s about word2vec that I used and it’s will be useful for gensim. When it will be finished, many researches (and me too) will have the opportunity to work with word2vec and big data. Gensim helped me in my academic project, so I want to do gensim better. 

