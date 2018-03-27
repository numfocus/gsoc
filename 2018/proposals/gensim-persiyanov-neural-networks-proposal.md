# Neural networks (similarity learning, state-of-the-art language models)

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

Gensim is an NLP library and its name stands for **gen**erate **sim**ilar. It provides highly optimized implementations for all most popular models for learning distributional representations of texts, such as Word2Vec, fastText, Poincare embeddings, LDA, LSI etc.

During last years, deep neural networks have become popular due to their ability to solve problems from a raw representation of the data. Convolutional networks help self-driving cars to percept the world, recognize pedestrians and street signs. Recurrent neural networks and attention mechanisms made it possible to deploy state-of-the-art machine translation to production.

That said, it would be great to have neural network based models in Gensim. Ideally, Gensim user could use them in several ways:

1. Take pre-trained model for document similarity.
2. Take pre-trained model, finetune it on user's domain data.
3. Train document similarity model from scratch.

Some requirements:

* User should be able to train the model on both supervised & unsupervised data.
* Model API must be flexible and allow to change neural network architecture easily.

The final goal of the project is:

* Introduce NN-based model for document similarity into Gensim core.
* Pretrain several models on popular datasets.
* Implement Jupyter Notebook with examples of usage. This notebook should answer user on HOW and WHY questions.

## Technical Details

There is a class of models for document similarity which is called [Deep Structured Semantic Model (DSSM)](https://www.microsoft.com/en-us/research/project/dssm/). They were firstly introduced by Microsoft Research.


The DSSM network consists of two “towers” for two documents. Each document is passed to a corresponding tower (towers could share the parameters == single tower is used). The tower takes its input and embeds it in semantic vector space (vectors R and C on the illustration below). Then, the similarity between two vectors is computed, i.e. using cosine similarity C^T*R/(||C||*||R||). The illustration below was given in chat-bot case (computing similarity between context and candidate reply).

![DSSM illustration](https://cdn-images-1.medium.com/max/1600/1*VXs7kE1Y9baXua3Cc9faTg.png).

DSSM models are trained using triplets of (document, pos_document, neg_document). It was shown in **"Sampling Matters in Deep Embedding Learning"** [paper](https://arxiv.org/abs/1706.07567) that loss function and sampling scheme are crucial. So, in experiments, I want to pay some attention to negative samples mining and losses.

Also, there is a trade-off between quality/size and training/inference time of the model. It's another direction for experiments. I want to train at least two models: 

* Light model. Faster, less number of parameters, possibly with bag-of-words on char n-grams ([BPE](https://arxiv.org/abs/1508.07909) could be useful here) encoding following with several dense layers.
* Heavy model. Slower, more parameters, better quality. Possibly with CNN/RNN encoders.


Additional details:

* As for implementation, I prefer PyTorch, because it allows rapid prototyping, it is easy to debug and gives readable code.
* As for datasets, I want to start with **Quora Duplication Pairs** dataset. It consists of labeled paraphrased questions which could be very useful to learn document similarity. Also, **Ubuntu Dialogue Dataset** could be used for unsupervised training. I plan to research more and find other datasets to evaluate on.



### High-level timeline:

* Implement light model and benchmark it.
* Experiment with negative sampling & losses.
* Provide _develop branch ready_ neural network model API.
* Implement heavy model using introduced API.
* Experiment with architecture, document encoding schemes, negative sampling, and losses.
* Pretrain best light/heavy architectures.
* Write a final report.

During all the phases I will report my progress in Gensim blog: https://rare-technologies.com/blog/.

## Timeline

### Community Bonding Period (April 23 -- May 13)

During community bonding:

* Continue working on Gensim issues I'm currently involved in -- [#1697](https://github.com/RaRe-Technologies/gensim/issues/1697) and [#1991](https://github.com/RaRe-Technologies/gensim/issues/1991). 
* Become more familiar with Gensim codebase.
* Participate in mailing list / gitter.
* Explore the latest papers to get ideas for NNs architectures.

### Week 1 (May 14 -- May 20)

* Start implementing evaluation environment.
* Implement simple document similarity model in order to make the whole pipeline work as quickly as possible.
* Train it on Quora Duplication Pairs dataset.

### Week 2 (May 21 -- May 27)

* Benchmarks on Quora and other datasets.
* Start implementing different losses and sampling schemes.

### Week 3 (May 28 -- June 3)
* Find the best _light_ (small number of parameters, fast training & inference) model configuration and fix it.
* Start writing _develop branch ready_ code.

### Week 4 (June 4 -- June 10)

* Submit PR with implemented neural network document similarity model.
* Write tests & documentation.

### Week 5 (June 11 -- June 17) (evaluation)

* Address reviewers comments on PR.
* Write Jupyter Notebook with an example of usage.

### Week 6 (June 18 -- June 24)

* Start experimenting with _large_ models in order to achieve ~SOTA quality.

### Week 7 (June 25 -- July 1)

* Continue achieving state-of-the-art on chosen tasks.

### Week 8 (July 2 -- July 8)

* Fix best model configuration.
* Start preparing it for users.

### Week 9 (July 9 -- July 15) (evaluation)

* Prepare _develop branch ready_ code with new concepts implemented. (I believe that I will not cover all possible model configuration options in the first version)
* Submit PR with new code introduced.

### Week 10 (July 16 -- July 22)

* Address reviewers comments on PR.
* Write Jupyter Notebook which shows examples of usage.

### Week 11 (July 23 -- July 29)

* Addressing reviewers comments.
* Have PR merged.

### Week 12 (July 30 -- August 5)

Cleaning the house:

* Fix bugs if any.
* Fix docstrings.
* Write more tests.

### Last week (August 6 -- August 12) (evaluation + submitting the code)

* Write final report -- the blog post about the project and results.

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

1. https://arxiv.org/abs/1508.07909
2. https://www.microsoft.com/en-us/research/project/dssm/
3. https://blog.statsbot.co/chatbots-machine-learning-e83698b1a91e
