# Word Mover's Distance for Gensim

## Abstract

The Word Mover's Distance (WMD) measures the dissimilarity between two text documents as the minimum amount of distance that the embedded words of one document need to “travel” to reach the embedded words of another document. This distance can be viewed as an instance of Earth Mover's Distance, a well studied transportation problem for which several highly efficient solvers have been developed.  The WMD metric leads to unprecedented low k-nearest neighbor document classification error rates and has no hyperparameter. While there is an academic implementation in C, there is no implementation of WMD available in Python. I will contribute a scalable implementation of WMD to the data science world in Python. A quality implementation will be widely used in the industry.

## Technical Details

Word2Vec [1, 2] is a continous word representation technique for creating word vectors to capture the syntax and semantics of words. The vectors used to represent the words have many interesting features, for example `king-man+woman=queen`.

Many methods are proposed on how to measure distance between sentences in this new vector space. "Word Mover's Distance" (WMD) [3] is a novel distance-between-text-documents measure. It outperforms simple combinations like sum or mean. Visually, the distance between the two documents is the
minimum cumulative distance that all words in document A need to travel to exactly match document B. 

For example, these two sentences are close with respect to WMD even though they only have one word in common: "The restaurant is loud, we couldn't speak across the tabel" and "The restaurant has a lot to offer but easy conversation is not there". [4]

**Goals**

1. Demonstrate understanding theory and practice of document distances by describing, implementing and evaluating WMD.

2. Implement the WMD. Processing must be done in constant memory independent on the full training set size. The implementation must rely on Python's NumPy and SciPy libraries for high performance computing. Optionally implement a version that can use multiple cores on the same machine. 

3. Practise modern, practical distributed project collaboration and engineering tools (git, mailing lists, continuous build, automated testing).


**Deliverables**

1. Code: a pull request against gensim [6] on github [7]. Gensim is an open-source Python library for Natural Language Processing. The pull request is expected to contain robust, well-tested and well-documented industry-strength implementation, not flimsy academic code. I will check corner cases, summarize insights into documentation tips and examples. 

2. Report: timings, memory use and accuracy of your WMD using the freely available datasets in [3], for example the "20 newsgroups" corpus [8]. A summary of insights into parameter selection and tuning of document distances.

**Resources**:

[1] [Mikolov, Tomas, et al. "Efficient estimation of word representations in vector space." arXiv preprint arXiv:1301.3781 (2013)](http://arxiv.org/pdf/1301.3781v3.pdf)

[2] [Gensim word2vec tutorial at Kaggle](https://www.kaggle.com/c/word2vec-nlp-tutorial/details/part-2-word-vectors)

[3] ["From Word Embeddings to Document Distances" Kusner et al 2015](http://jmlr.org/proceedings/papers/v37/kusnerb15.pdf) 

[4] [Sudeep Das "Navigating themes in restaurant reviews with Word Mover’s Distance", 2015] (http://tech.opentable.com/2015/08/11/navigating-themes-in-restaurant-reviews-with-word-movers-distance/)

[5] [Matthew J Kusner's WMD in C on github](https://github.com/mkusner/wmd)

[6] [Radim Řehůřek and Petr Sojka (2010). Software framework for topic modelling with large corpora. Proc. LREC Workshop on New Challenges for NLP Frameworks](http://www.fi.muni.cz/usr/sojka/papers/lrec2010-rehurek-sojka.pdf)

[7] [Gensim on github](https://github.com/piskvorky/gensim)

[8] [The 20 newsgroups dataset](http://qwone.com/˜jason/20Newsgroups/)

[9] [Gensim github issue #482](https://github.com/piskvorky/gensim/issues/482)


## Schedule of Deliverables

### May 25th -  June 7th

Get better acquainted with Gensim, study the WMD paper [4].

### June 8th - June 21th

First prototype implementation of the WMD distance.

### June 22nd - July 5th

Tests for correctness, finding and creating tests for corner cases. Possibly trying out another word embeddings apart from word2vec.

### July 6th - July 19th

Cleaning up / enhancing of the implementation.

### July 20th - August 2nd

Tests, benchmarking, bug hunting.

### August 3rd - August 16th

Writing up documentation & report.

### August 17th - August 21th 19:00 UTC

Week to scrub code, improve documentation, etc.

## Future works

Implement and develop ideas sketched in the original WMD paper, implement other ideas from the Gensim student project  list (such as implementation of the AKSW topic coherence measure or on-line algorithm for non-negative matrix factorization).

## Open Source Development Experience

I've contributed to the SageMath project. 

## Academic Experience

Three peer-reviewed research articles in differential geometry / global analysis.

## Why this project?

I am slowly transitioning from pure mathematics to the field of data analysis. Gensim seems like a perfect place to get my feet wet. Also, I've talked with Radim Rehurek after his talk on word2vec and he seemwed like a nice guy.

## Appendix

You can find more details about me at `https://www.linkedin.com/in/vittucek`