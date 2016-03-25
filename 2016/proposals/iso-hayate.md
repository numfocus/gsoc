# Title

Online Word2Vec development

## Abstract

* Word embedding (as known as Word2Vec) is one of the major topics in natural language processing research and we can make it easily using Gensim.

* However, we cannot update Word2Vec model after first training, so we have to retrain Word2Vec model every time you get the new data.

* Therefore, I want to try to add online learning feature to Gensim Word2Vec model.

## Technical Details

* Online machine learning is a method which uses for stream data and update every time you get data. Vanilla Word2Vec train the model every time you get the data. The word usage continue to change so we need to update Word2Vec model constantly.

## Schedule of Deliverables

### now -  May 24th (not GSoC period)

* Understand Mikolov's papers [1][2][3] and Gensim Word2Vec architecture.

### May 25th -  June 7th

* Read [online Word2Vec for Gensim](http://rutumulkar.com/blog/2015/word2vec/)[4] and Gensim's issue [#435](https://github.com/piskvorky/gensim/pull/435) and [#615](https://github.com/piskvorky/gensim/pull/615) to develop online Word2Vec.

### June 8th - June 21th

* Start developing online Word2Vec refer to Rutu's code.

* I want to reproduce online Word2Vec at least in local environment.

### June 22nd - July 5th

* Continue developing online Word2Vec.

* Evaluate online Word2Vec using Rutu's data.

* Start surveying the method of evaluating Word2Vec.

### July 6th - July 19th

* Reread Gensim Word2Vec architecture to understand online Word2Vec code in general perspective.

* Find the error to resolve AppVeyor error.

### July 20th - August 2nd

* Evaluate online Word2Vec using some data sets such as [Lee corpus](http://www.socsci.uci.edu/~mdlee/lee_pincombe_welsh_document.PDF)[5].

* Test online Word2Vec using several parameters.

* Write documentation for users to easily use online method and choose parameters.

### August 3rd - August 16th

* Start writing a blog post about online Word2Vec usage.

### August 17th - August 21th 19:00 UTC

* Write a blog post about online Word2Vec performance.

## Future works

* I'm interested in word embedding methods, not only Word2Vec but also Doc2Vec and something like that. I think online method is applicable for other word embedding methods.

* Therefore, I want to continuous contribution for Gensim to add online features.

## Open Source Development Experience

* I haven't had Open source development experience so far. Google Summer of Code is good opportunity for me to start developing Open source software.

## Academic Experience

* I got a B.S. in statistics at Osaka University and I'm in Computational Linguistics Laboratory at Nara Institute of Science and Technology now.

* I studied statistics in the theoretical point of view. In my earlier lab, I studied sparse estimation for high-dimensional data. Many people use Lasso for high-dimensional data, but I used Boosting. Boosting also has a good feature for sparse estimation.[6]

* I also studied machine learning as a hobby. I finished [machine learning course](https://www.coursera.org/learn/machine-learning)[6] by Andrew Ng and read [Foundations of machine learning](http://www.cs.nyu.edu/~mohri/mlbook/)[7] which show machine learning as statistical perspective.

* Current research interest is natural language processing, especially word embedding.

* I also have an experience working as internship to develop recommender system using Gensim. I used job applicants resume data and job offer data to job matching by Gensim implementation of Latent Dirichlet allocation.

## Why this project?

* The reason for choosing Gensim project is that I use Gensim a lot!
I belong Computational Linguistic Laboratory and I study word embedding.
Gensim give us to use some kinds of word embedding implementation easilly.
I want to make a contribution to Gensim comunity and at the same time understand Gensim architecture deeply.

* Therefore, I choose Gensim project as Google summer of code 2016.

## Appendix
[1] Tomas Mikolov, Wen-tau Yih, Geoffrey Zweig, "Linguistic Regularities in Continuous Space Word Representations." 2013, NAACL

[2] Tomas Mikolov, Kai Chen, Greg Corrado, Jeffrey Dean, "Efficient estimation of word representations in vector space" 2013, ICLR

[3] Tomas Mikolov, Ilya Sutskever, Kai Chen, Greg S Corrado, Jeff Dean, "Distributed representations of words and phrases and their compositionality", 2013, NIPS

[4] Online Word2Vec for Gensim http://rutumulkar.com/blog/2015/word2vec/

[5] Lee, M., Pincombe, B., & Welsh, M. "An empirical evaluation of models of text document similarity.", 2005, Proceedings of the 27th Annual Conference of the Cognitive Science Society

[6] Machine learning course at Coursera https://www.coursera.org/learn/machine-learning

[7] Mehryar Mohri, Afshin Rostamizadeh, and Ameet Talwalkar, "Foundations of Machine Learning", 2012, MIT Press.
