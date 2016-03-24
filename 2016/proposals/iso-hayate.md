# Title

Online Word2Vec development

## Abstract

Word embedding (as known as Word2Vec) is one of the hot topic in natural language processing research and we can implement it easily using gensim.

However, gensim word2vec implementation has a disadvantage. It cannot update word2vec model after initial training, so we have to retrain word2vec model every time you get the new data.

Therefore, I want to try to add online learning feature to gensim word2vec model.



## Technical Details

Online learning is a method which uses for stream data and update every time you get data. Vanilla word2vec train the model every time you get the data. The word usage continue to change so we need to update word2vec model constantly.



## Schedule of Deliverables

### now -  May 24th (not GSoC period)

Understand Mikolov's papers and gensim word2vec architecture.



### May 25th -  June 7th



Read [Online Word2Vec for Gensim](http://rutumulkar.com/blog/2015/word2vec/) and gensim's issue #435 and #615 to develop online Word2Vec.

### June 8th - June 21th



Transcribe online word2vec and implement it at least in local environment.



### June 22nd - July 5th



Review gensim word2vec architecture to understand online Word2Vec code in the and resolve AppVeyor error. 



### July 6th - July 19th



Evaluating online Word2Vec using [Lee corpus](http://www.socsci.uci.edu/~mdlee/lee_pincombe_welsh_document.PDF) and start writing documentation for gensim users.



### July 20th - August 2nd

If I had my way, extend online method to other implementation such as doc2vec.

If I hadn't my way, fix bug.



### August 3rd - August 16th

Test online Word2Vec using several parameters and write documentation for users to easily use online method and choose parameters.



### August 17th - August 21th 19:00 UTC



Refactor code, write tests, improve documentation, etc.


## Future works

Extend online implementations to other word embedding methods.



## Open Source Development Experience

I haven't had Open source development experience so far.

Google Summer of Code is good opportunity for me to start developing OSS.



## Academic Experience

I got a BS in statistics at Osaka University and I'm in Computational Linguistics Laboratory at Nara Institute of Science and Technology now.

I studied some machine learning method in statistical perspective such as SVM, Boosting and something like that and I deeply understand about sparse estimation.

Current research interest is natural language processing, especially word embedding.

## Why this project?

The reason for choosing gensim project is that I use gensim a lot!
I belong Computational Linguistic Laboratory and I study word embedding.
Gensim provide us to use some kinds of word embedding implementation easilly.
I want to make a contribution to gensim comunity and at the same time understand gensim architecture deeply.
Therefore, I choose gensim project as Google summer of code 2016.

## Appendix

I have an experience working as internship to develop recommender system using gensim.