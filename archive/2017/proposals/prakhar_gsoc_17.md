# Performance Improvement in gensim and fastText

## Name and Contact Information 

**Name :**  Prakhar Pratyush <br/>
**University :** [Indian Institute of Technology Roorkee](http://www.iitr.ac.in/), India  <br/>
**Email :** er.prakhar2b@gmail.com <br/>
**Github/ Gitter :** [prakhar2b](https://github.com/prakhar2b) <br/>
**Video Chat :** hangouts (er.prakhar2b@gmail.com) <br/>

## Pre- GSoC involvements 
Here is the list of PRs I have contributed to -<br/><br/>

- [#1186](https://github.com/RaRe-Technologies/gensim/pull/1186) **(merged)**: Fix [#824](https://github.com/RaRe-Technologies/gensim/issues/824) : no corpus in init, but trim_rule in init in word2vec<br/>
- [#1190](https://github.com/RaRe-Technologies/gensim/pull/1190) **(merged)**: Opened [#1187](https://github.com/RaRe-Technologies/gensim/issues/1187) and modified error message in word2vec<br/>
- [#1200](https://github.com/RaRe-Technologies/gensim/pull/1200) :  Fix [#1198](https://github.com/RaRe-Technologies/gensim/issues/1198) changed params to make API consistent with the code<br/>
- [#1208](https://github.com/RaRe-Technologies/gensim/pull/1208) **(merged)**:  grepped through all the wrapper codes to fix  [#671](https://github.com/RaRe-Technologies/gensim/issues/671)<br/>
- [#1226](https://github.com/RaRe-Technologies/gensim/pull/1226) : Fix  [#691](https://github.com/RaRe-Technologies/gensim/issues/691) : index object pickle to be always under `index.output_prefix`<br/><br/>

Other contributions in [giacbrd/shallowLearn](https://github.com/giacbrd/ShallowLearn) (relevant to this project)

- [#16](https://github.com/giacbrd/ShallowLearn/issues/16) -  opened the issue to clarify the ambiguity in the performance graph
- [#13](https://github.com/giacbrd/ShallowLearn/issues/13)  -  Cythonize the hash function ( currently working on this)
- [#17](https://github.com/giacbrd/ShallowLearn/issues/17) - predicting multiple documents at once (wishlist) ; @giacbrd opened
this issue after looking into this ( [Ipython](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/LabeledWord2Vec_first_run.ipynb) ) benchmark.

## Abstract
Gensim is an NLP library which trains a huge amount of data for semantic/syntactic
analysis and topic modelling. For such a huge amount of data **(~ billions of words)**,
implementation needs to be reasonably fast, otherwise difference maybe in the order of
days or even weeks of training time. <br/><br/>
Facebook Research recently released a new word embedding and classification library
fastText, which performs better than word2vec on syntactic task, and trains much faster
for supervised text classification. The architecture of supervised text classification is a
modified version of word2vec cbow model, and takes into account the n-gram features
implemented using the hashing trick. The initial implementation by facebook was in
C++, later @giacbrd produced a Python implementation **(labeled word2vec)** building
on top of gensim code. <br/><br/>
The goal of this project is :-
- Evaluate and benchmark the performance of fastText in python (labeled
word2vec).
- Taking inspiration from gensim word2vec code optimization, this project aims at
refactoring/ reorganizing, and re-implementing the frequently used and time
consuming part in Cython  **to match the performance with fastText (Facebook) and
vowpal wabbit supervised classification.**
- After finalizing the parallel version, GPU version of the code will be implemented
which might require re-writing some part in TensorFlow.
- For collocations (n-gram), gensim has phrases module. This project also aims at
multi-core implementation of **phrases module in cython.**
- To improve gensim overall performance, other bottlenecks will be found and
refactored throughout the project.
- Implement hashing trick in labeledword2vec (wishlist)

## Technical Details
In unsupervised mode, the cbow model of word2vec works by sliding a window
over the words (context), and building a weight matrix by trying to predict the
middle word (target). This weight matrix turns out to be our word vector, which
we later use for other NLP tasks. <br/><br/>
fastText supervised classification is very similar to the word2vec cbow model. It
continues the philosophy of word2vec that **"the closer the word is to the target
the more predictive it should be"**. In word2vec, the goal was to obtain a word
embedding, so the target was middle word (in context) for semantic similarity; but
in fastText, the goal is classification, so the target (predicting task) is the label.<br/><br/>
![fig 1. Word2vec cbow model](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/cbow2.png)

Instead of sliding a window over the words, in fastText we slide over each entity
related to the label (sentences, for text classification), and take N n-gram features (N is the number of labels) from each sentence which is later averaged
to form the text representation. We train the model in the same way as word2vec. <br/><br/>
![fig 2. fastText supervised classification](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/IMG_20170329_005922.jpg)

Gensim has a phrases module which provides collocations (bigram) based on
how frequently adjacent words appear in the corpus. In general, n-gram features
can be obtained by calling it multiple times. <br/><br/>
phrases module is organized into two classes -  **Phrases** and **Phraser** . Phrases
to detect phrases and Phraser (faster and memory efficient) as an alternative that
is built from initial Phrases instance. <br/><br/>
**Note**:-  **prune_vocab** only when  **len(vocab) > max_vocab** , why not prune_vocab
every time as we do in word2vec  **scale_vocab** . Discuss this (maybe in
community bonding period)<br/><br/>
![fig 3. Architecture of phrases.py in gensim](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/IMG_20170330_112000.jpg)

## phrases.py
The phrases module in gensim is implemented purely in Python, therefore speed
is quite slow as of now. If we provide input using phrases stream, for example, to
word2vec, the training consumes input faster than phrases can supply it.
Therefore, one metric to evaluate speed would be to **reduce the time word2vec
has to wait** while we provide input to it from phrases.<br/><br/>
![](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/wordphrase.png)
An initial profiling of phrases module show bottlenecks in the code that needs to
be improved. For example, the learn_vocab function in Phrase class has a for
loop that consumes most of the time.<br/>
![](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/phrase-1.png)
![](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/phrase-2.png)
```peak memory: 541.90 MiB, increment: 436.45 MiB```

export_phrases() also needs to be improved/ parallelized<br/><br/>
![](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/phrase-3.png)
![](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/phrase-4.png)
### Possible Improvements -
- Efficient memory access
- Numpy, Numba (optional) etc
- Nested loop as matrix operation
- Cythonize
- BLAS
- Other tricks
- Parallelize

![fig 4. Multiprocessing with worker threads in gensim [4]](https://github.com/prakhar2b/Weekend-Projects/blob/master/gensim/phrase-multi.jpg)

## LabeledWord2Vec
LabeledWord2Vec is currently designed as a subclass of Word2Vec. An initial
profiling over pure python version shows it needs a lot of refactoring and
optimization. Here is the [IPython] benchmark trained on dbpedia dataset.

### Proposed change in architecture:- 

**BaseNN** ( Base Shallow Neural Network )
- train()<br/>

**Word2Vec** ( child of BaseNN )
- Vocabulary building, and other codes<br/>

**LabeledWord2Vec** ( child of BaseNN  )
- Vocabulary building
- Label vocab building
- Incorporate label vocab into KeyedVector. Default ``` None ``` for
word2vec.
- Scan_vocab : currently it is count_raw_words()
- Scale_vocab, and other codes
- labeledw2v_inner.pyx - Cython code for score/loss etc.<br/>

**Wishlist** :- **( Hashing trick)** Gensim has corpora.hashdictionary. Try to introduce
that into keyedvector, as it is very crucial in the performance of facebook
fastText.<br/><br/>

There are other chunks of codes which are very similar in both word2vec and
labeledw2v, which can be organized in a better way. For example,
update_weight, reset_weight etc.Proposed 

## Timeline 
I have been involved with gensim for around two months now. I am familiar with
most of the code base, so I’ll be looking to start early so that things go smoothly
during coding period. I often set pre-deadlines which helps in improvising in the
last minutes too. I also strongly believe in Pareto principle that 80% of the effects
come from 20% of the causes, which is even more relevant for this project as the
primary goal is to streamline the bottlenecks and improve the code accordingly.
The whole project can be broadly categorized into milestones and labels :-<br/><br/>

**Milestone 1**: Phrases
- **Label 1.1** : learn_vocab
- **Label 1.2** : export_phrases
- **Label 1.3** : pseudocorpus, phrasegrams, getitems
- **Label 1.4** : multi-core
- **Label 1.5** : Test  **( Mid Term I)** <br/>

**Milestone 2** : fastText (labeledw2v)
- **Label 2.1** : BaseNN as parent class of word2vec
- **Label 2.2** : Cythonize labeledw2v training
- **Label 2.3** : Cythonize/ improve remaining parts
- **Label 2.4**: Refactor/ improve, and compare with fastText/ vowpal
wabbit
- **Label 2.5** : Write tests and create benchmark  **( Mid Term II)**
- **Label 2.6** : Precision and Recall
- **Label 2.7** : Predict multiple documents at one
- **Label 2.8** : Save prediction result <br/>

**Milestone 3** : GPU
- **Label 3.1** : Training
- **Label 3.2** : Write tests and create benchmark

**Wishlist (W)** :  work in free time / after Milestones
- **Label W.1** : Hashing in labeledword2vec (hashing trick with bucket)
- **Label W.2** : Cythonize / parallelize scan_vocab in word2vec<br/>

## Community Bonding Period
- **May 4 - May 15 :**
  -  Get more familiar with the code structure, and community by
actively Participating in discussions (mailing list /gitter) and solving
bugs/ issues.
  -  Discuss and think more about possible change in architecture of
phrases module.
  -  Profile and benchmark phrases and other models in a more
comprehensive manner.<br/><br/>

- **May 16 - May 29 :**
  -  Continue participation in mailing list /gitter / issue triage
  - Discuss and think more about possible change in architecture of
Word2vec and labeledword2vec module.
  - Research more on TensorFlow (GPU)
  - debug, profile and benchmark labeledword2vec in a more
comprehensive manner
  -  set up blogs for weekly/ fortnight update. Keep your work
documented<br/><br/>

## Week I ( May 30 - June 5)

- Start with learn_vocab() function in phrases module, and refactor by using
different optimization methods and benchmark the improvements.
- Create phrases_inner.pyx and write cython code for learn_vocab
- Work on add_vocab, and prune_vocab too.
- 30 minutes per day- write codes for fun - tensorflow (gpu)
- Accomplishment :- Label 1.1<br/><br/>

## Week II ( June 6 - June 12)

- Refactor export_phrases and cythonize it
- Write multi-core cython code for learn_vocab. There might be issues
relating to GIL, debug and run tests.
- Write blogs about work done so far
- 30 minutes for 3 day : Tensorflow (gpu)
- Accomplishment : Label 1.2 <br/><br/>

## Week III ( June 13 - June 19)

- Refactor phrasegram, getitem and other remaining parts and cythonize
getitem
- Implement multi-core version
- Accomplishment : Label 1.3, 1.4 <br/><br/>

## Week IV (June 20 - June 23)

- Refine everything done so far.
- Finish multi-core version
- Debug and test. Finish.
- Accomplishment : Label 1.5<br/><br/>

## Mid Term Evaluation I (June 26 - June 30)

- Work on feedback/review (if any)
- Refine/debug (initialize Milestone 3 if completed early)
- Accomplishment : Milestone 1<br/><br/>

## Week V ( June 30 - July 6)

- Create a BaseNN class in basemodel.py
- Refactor word2vec/labeledw2v, and make it run with BaseNN parent class
- Work on improving vocab building in labeledw2v
- Work on optimizing pure python part
- Accomplishment : Label 2.1<br/><br/>

## Week VI ( July 7 - July 13) - Week VII ( July 14 - July 20)

- Work on parallel training code in cython
- Refactor, optimize, benchmark with fastText/ vowpal wabbit
- Accomplishment : Label 2.2, Label 2.3<br/><br/>

## Week VIII (July 21 - July 25)

- Cleanup and refine the code written so far
- Work on remaining optimization tasks and create benchmark
- Accomplishment : Label 2.4 <br/><br/>

## Mid Term Evaluation II (July 24 - July 28)

- Work on feedback/ review, if any.
- Refine/ debug
- Accomplishment : Label 2.5 <br/><br/>

## Week IX ( July 29 - Aug 6)

- Work on remaining optimization tasks
- Implement evaluation measures- precision /recall. There is some  [ambiguity](https://groups.google.com/forum/#!searchin/fasttext-library/precision%7Csort:relevance/fasttext-library/pcGizsoOwAw/cJ4-_q4rAgAJ)
in facebook fastText’s implementation of precision/recall. Write from
scratch or maybe create a wrapper function for scikit-learn evaluation
measures.
- Implement prediction for multiple documents at once, and save the result in
a file.
- Work on hashing trick (It’s a key factor in the speed of fastText)
- Write tests
- Accomplishment : Label 2.6, Label 2.7, Label 2.8, Milestone 2<br/><br/>

## Week X ( Aug 7 - Aug 13)

-Start working on GPU part with tensorFlow
<br/><br/>
## Week XI ( Aug 14 - Aug 20)

- Continue working on GPU
- Testing. Benchmarking
- Accomplishment : Label 3.1<br/><br/>

## Week XII ( Aug 21 - Aug 29)

- Complete GPU version of labeledw2v
- Finalize codes and documents for final submission
- Work on feedback/review, if any.
- Wrap up everything-
- Accomplishment : Label 3.2, Milestone 3<br/><br/>

## More About Me
I am an aspiring researcher & entrepreneur, currently pursuing Electronics &
Communication Engineering (UG) at IIT Roorkee. I am passionate about
Embedded Systems & IoT, and Machine Learning. I am always more interested
in practical implementations of new technologies, and often look up and
contribute to open source.<br/><br/>
As an aspiring entrepreneur, I like to call myself - ​ Jack of all codes ​ . My journey
from writing first arduino C code for a robotic arm, to making a python
contribution in a machine learning library (gensim), is a result of curiosity and
passion. I have worked on web development (backend) , Robotics, IoT, and
Android development, to finally realize my passion in machine learning, and I
aspire to make significant contributions in this field.<br/><br/>
## Why ME ?
I have been contributing to gensim for around two months now. I have all the
prerequisites, and I’ve spent significant amount of time in understanding the bits
and pieces of this project. Once I gain momentum, I can work for longer hours at
stretch. My favourite leisure activity is also *training neurons with caffeine,* these
days​ .<br/><br/>
  ### Proficiency​ :-

  **Programming Languages**​ : C/C++, Python, Cython (in order)<br/>
  **Libraries**​ : sklearn, tensorflow, gensim<br/>
  **IDE**​ : Anaconda (IPython Notebook)/ Sublime Text on Ubuntu 14.04<br/>
  **Relevant Courses** :  [Machine Learning (Andrew Ng/ Coursera)](https://www.coursera.org/learn/machine-learning) ,
  [Deep Learning (Google/ Udacity)](https://classroom.udacity.com/courses/ud730)<br/><br/>

I’m a voracious reader as well. Apart from reading novels, I also continuously
read a lot of tech blogs, and frankly speaking, I am a self taught programmer - allcredit to the internet. I got introduced to gensim first through a tech blog at
[kaggle](https://www.kaggle.com/c/word2vec-nlp-tutorial#part-2-word-vectors) , and later while surfing through previous year gsoc organization lists.
During the timeline of this project, I have no other commitments.<br/><br/>
## Acknowledgement
1. Giacomo Berardi ([@giacbrd](https://github.com/giacbrd) ) - Thanks for replying to my emails.
2. Gensim community<br/><br/>
## References
1. [GSoC 2017 project ideas](https://github.com/RaRe-Technologies/gensim/wiki/GSOC-2017-project-ideas#performance-improvements-in-gensim-and-fasttext)
2. [Bag of Tricks for Efficient Text Classification](https://arxiv.org/abs/1607.01759)
3. [word2vec Parameter Learning Explained](https://arxiv.org/abs/1411.2738)
4. [Radim Řehůřek - Faster than Google ?](https://www.youtube.com/watch?v=vU4TlwZzTfU)
5. [Labeled w2v by @giacbrd PR#1153](https://github.com/RaRe-Technologies/gensim/pull/1153)
6. [fastText ( Facebook Research)](https://github.com/facebookresearch/fastText)
7. [Using GPUs | TensorFlow](https://www.tensorflow.org/tutorials/using_gpu)
8. [Mathworks : Optimizing Memory Access](https://in.mathworks.com/company/newsletters/articles/programming-patterns-maximizing-code-performance-by-optimizing-memory-access.html)
