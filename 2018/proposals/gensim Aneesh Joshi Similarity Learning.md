# Neural Networks for Similarity Learning

## Name and Contact Information 
**Name :**  Aneesh Joshi <br/>
**Email :** aneeshyjoshi@gmail.com <br/>
**Github/Gitter :** [aneesh-joshi](https://github.com/aneesh-joshi) <br/>

## Pre-GSoC involvements 
Gensim Contributions:<br/>
- [#1915](https://github.com/RaRe-Technologies/gensim/pull/1915) **(merged)** : Fix [#465](https://github.com/RaRe-Technologies/gensim/issues/465) Allow initialization with `max_final_vocab` in lieu of `min_count` for `gensim.models.Word2Vec`.
- [#1887](https://github.com/RaRe-Technologies/gensim/pull/1887) **(merged)** : Fix [#1878](https://github.com/RaRe-Technologies/gensim/issues/1878) Fix deprecation warning for py3 from `inspect.getargspec`. 
- [#1962](https://github.com/RaRe-Technologies/gensim/pull/1962) **(ongoing)**  Fix [#1654](https://github.com/RaRe-Technologies/gensim/issues/1654) Use Bounter for approx frequency counting
- [#1440](https://github.com/RaRe-Technologies/gensim/pull/1440) **(extended and finished in)** [#1800](https://github.com/RaRe-Technologies/gensim/pull/1800) Add word embedding viz to word2vec notebook by [@markroxor](https://github.com/markroxor)

Docs related PRs:<br/>
- [#1426](https://github.com/RaRe-Technologies/gensim/pull/1426) **(merged)** : Fix incorrect link in notebook (word2vec)
- [#29](https://github.com/RaRe-Technologies/bounter/pull/29) **(merged)** : Fix incorrect image link in `experiments.md` (for bounter repo)
- [#31](https://github.com/RaRe-Technologies/bounter/pull/31) **(merged)** : Include blog link in ReadMe (for bounter repo)

Relevent Non-Gensim contributions:<br/>
- [Bidirectional Long Short Term Memory(LSTM) Part of Speech(POS) Tagger using keras](https://github.com/aneesh-joshi/LSTM_POS_Tagger)
- Blog titled: [Learn Word2Vec by Implementing it in tensorflow](https://towardsdatascience.com/learn-word2vec-by-implementing-it-in-tensorflow-45641adaf2ac)


## Abstract
Reseachers and Industry experts alike turn to gensim for easy-to-pick up and production ready code. Recently, Deep Learning techniques have taken the fore front in apparent "state of the art" performances, however, these techniques haven't made their way into production due to unreproducability and lack of availability. This project will evaluate these newer techniques, justify their benefit in reproducible manners and integrate  them into the gensim toolkit.


## Introduction
Deep Learning essentially operates on allowing the model to find a suitable vector representation for a given input signal. Thus, it lends itself well to the task of Similarity Learning where we want to find the similarity between two documents without explicity deciding the way it is represented. Similarity Learning has several use cases like:

- document retrieval
- question answering
- conversational response ranking
- paraphrase identification 

<table>
  <tr>
    <th width=30%, bgcolor=#999999 >Tasks</th> 
    <th width=20%, bgcolor=#999999>Text 1</th>
    <th width="20%", bgcolor=#999999>Text 2</th>
    <th width="20%", bgcolor=#999999>Objective</th>
  </tr>
  <tr>
    <td align="center", bgcolor=#eeeeee> Paraphrase Indentification </td>
    <td align="center", bgcolor=#eeeeee> string 1 </td>
    <td align="center", bgcolor=#eeeeee> string 2 </td>
    <td align="center", bgcolor=#eeeeee> classification </td>
  </tr>
  <tr>
    <td align="center", bgcolor=#eeeeee> Textual Entailment </td>
    <td align="center", bgcolor=#eeeeee> text </td>
    <td align="center", bgcolor=#eeeeee> hypothesis </td>
    <td align="center", bgcolor=#eeeeee> classification </td>
  </tr>
  <tr>
    <td align="center", bgcolor=#eeeeee> Question Answer </td>
    <td align="center", bgcolor=#eeeeee> question </td>
    <td align="center", bgcolor=#eeeeee> answer </td>
    <td align="center", bgcolor=#eeeeee> classification/ranking </td>
  </tr>
  <tr>
    <td align="center", bgcolor=#eeeeee> Conversation </td>
    <td align="center", bgcolor=#eeeeee> dialog </td>
    <td align="center", bgcolor=#eeeeee> response </td>
    <td align="center", bgcolor=#eeeeee> classification/ranking </td>
  </tr>
  <tr>
    <td align="center", bgcolor=#eeeeee> Information Retrieval </td>
    <td align="center", bgcolor=#eeeeee> query </td>
    <td align="center", bgcolor=#eeeeee> document </td>
    <td align="center", bgcolor=#eeeeee> ranking </td>
  </tr>
</table>
-- [1]

## Technical Details
Most of the Similarity Learning techniques make use of some form of neural network which is trained in order to reduce the error for a given task. Depending on the way the inputs, outputs and loss functions are configured, we get broadly 4 ways of using Similarity Learning:

**1. Regresssion Similarity Learning:** given `document1` and `document2` we need a `similiarity measure`

**2. Classification Similarity Learning:** given `document1` and `document2` we need to classify them as similar or disimilar

**3. Ranking Similarity Learning:** given `document`, `similar-document` and `disimilar-document`, we want to learn a `similarity-function` such that `similarity-function(document, similar-document)  > similarity-function(document, disimilar-document)`

**4. Locality Sensitivity Hashing:** hashes input items so that similar items map to the same "buckets" in memory with high probability.

--[2]

In order to keep concrete evaluation and impementations of the Similarity Learning algorithms, we will use:
* Dataset: <a href="https://www.microsoft.com/en-us/download/details.aspx?id=52419">WikiQA</a>. It is a popular benchmark dataset for answer sentence selection in question answering.
* Deep Learning Library: [Keras](https://keras.io/). _Keras is a high-level neural networks API, written in Python and capable of running on top of TensorFlow, CNTK, or Theano. It was developed with a focus on enabling fast experimentation. Being able to go from idea to result with the least possible delay is key to doing good research._

**Disclaimer:**
_A lot of the motivation, ideas and details for this proposal has been taken and inspired from [MatchZoo](https://github.com/faneshion/MatchZoo), a toolkit for text matching. MatchZoo has implemented a lot of famous Similarity Learning algorithms in keras. For the benefit of time and effort, I will be using their keras implementations for making baselines and further improve upon them (rather than writing from scratch). My best effort will thus go into benchmarking, improving and integrating these models. I have consulted [Menshikh Ivan](https://github.com/menshikh-iv) for the same._


### The steps that will be taken are:

1. Evaluate and benchmark existing Similarity Learning algorithms inorder to decide which are worth providing
2. Develop efficient, production ready implementations
3. Integrate implementations into gensim, providing easy apis, model serialization and other gensim guarentees
4. Disseminate the feature additions through blogs, ipython notebooks and video tutorials

## The Similarity Learning models will have the following three phases/modules:
**1. Data Preparation**<br/>
**2. Model Construction**<br/>
**3. Model Training**

### 1. Data Preparation
In this phase, the input data will be converted to the form required for training the given model. 
For example, the Deep Structured Semantic Model(DSSM) requires the input stream to be converted into character wise trigrams.<br/>
Eg: `hello -> #hello# -> #he, hel, ell, llo, llo#`<br/>
While minor functionalities like these may not be available in gensim, most of the other data preparation can be handled by `Word2VecVocab`-like functions.

Majorly, the following data preparation needs are foreseen:<br/>
**1. Word Indexing and Vocab Pruning:**<br/>
This involves:<br/>
1. Making a word2index dictionary
2. Removing high and low frequency words<br/>
A large part of the `Word2VecVocab` code can be reused here.

**2. Word Slicing:**<br/>
As mentioned above, the words need to be sliced into the token pairs. The number of tokens (like `hel`, `ld#`, `abc`, etc) can be predetermined or dependent on the corpus.
A seperate dict will have to be introduced for `token2index` like translations.

**3. Dataset Conversion:**
Deep Learning models require the data to be present in input, output formats (Example : `X_train`, `Y_train`)
In Similarity Learning, we mostly have tasks like:
Given `document1` and `document2`, optimize some sort of objective like classification or ranking.
The data will have to be:
1. converted into the `token`  form (and then)
2. converted to the train, test form

### 2. Model Construction
In this phase, a model will be built in keras using the parameters provided to the gensim API by the user. Since the goal of this project is to provide Similarity Learning models "out-of-the-box" similar to how `word2vec` works out of the box, the models like DSSM, MV-LSTM, etc. (mentioned later) will be provided. 
The user will just have to specify:
1. training data
2. the hyperparameters like learning rate, etc
3. loss functions and optimizers

We may even include a functionality where the user can specify network architectures. For example, the DSSM architecture defined in [this paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/cikm2013_DSSM_fullversion.pdf) suggests a neural architecture of 3 layers with `300, 300 and 128` neurons. The user may want to experiment with something different like `300, 150, 75 and 64`. In this case, we may expose some of the keras functionalities.
Example: 
```
# get the keras model
DSSM_model = DSSM_model.expose_keras_model(reset_layers=True)

# modify it
DSSM_model.add(Dense(300))
DSSM_model.add(Dense(150))
DSSM_model.add(Dense(75))
DSSM_model.add(Dense(64))

DSSM_model.compile()

# Note: this is just example psuedo code
```


### 3. Model Training
In this phase, the user can train the model and monitor its improvements.
The module will provide:<br/>
**1. A metrics parameter:** to specify which metrics will be monitored during training<br/>
**2. A verbosity parameter:** to supress or express feedback to different degrees<br/>
**3. A tensorboard/matplotlib visualization of the metrics**<br/>
_(Further parameters TBD)_

## Example Usage (Exact semantics TBD)
```
from gensim.similarity import DSSM
import gensim.downloader as api
import pickle

# You may use api data directly
# SL_data = api.load('SomeSimilarityTaskData')

with open('my_data', 'rb') as f:
	docs, labels = pickle.load(f)

DSSM_model = DSSM(docs=docs,
		  labels=labels,
		  min_count=4,
		  epochs=25,
		  train_validate_test_split = (0.8, 0.1, 0.1))

print(DSSM_model.predict_similarity(["what does LDA stand for", "what is the full form of LDA"]))
```


## Broadly, the following algorithms for Similarity Learning are planned:
1. DSSM : <a href="https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/cikm2013_DSSM_fullversion.pdf">Learning Deep Structured Semantic Models for Web Search using Clickthrough Data</a>
2. CDSSM : <a href="https://www.microsoft.com/en-us/research/publication/learning-semantic-representations-using-convolutional-neural-networks-for-web-search/">Learning Semantic Representations Using Convolutional Neural Networks for Web Search</a>
3. MV-LSTM : <a href="https://arxiv.org/abs/1511.08277">A Deep Architecture for Semantic Matching with Multiple Positional Sentence Representations</a>
4. aNMM : <a href="http://maroo.cs.umass.edu/pub/web/getpdf.php?id=1240">aNMM: Ranking Short Answer Texts with Attention-Based Neural Matching Model</a>
5. ARC-I : <a href="https://arxiv.org/abs/1503.03244">Convolutional Neural Network Architectures for Matching Natural Language Sentences</a>
6. ARC-II : <a href="https://arxiv.org/abs/1503.03244">Convolutional Neural Network Architectures for Matching Natural Language Sentences</a>
7. DRMM : <a href="http://www.bigdatalab.ac.cn/~gjf/papers/2016/CIKM2016a_guo.pdf">A Deep Relevance Matching Model for Ad-hoc Retrieval</a>.
8. K-NRM : <a href="https://arxiv.org/abs/1706.06613">End-to-End Neural Ad-hoc Ranking with Kernel Pooling</a>
9. MatchPyramid :<a href="https://arxiv.org/abs/1602.06359"> Text Matching as Image Recognition</a>
10. DUET : <a href="https://dl.acm.org/citation.cfm?id=3052579">Learning to Match Using Local and Distributed Representations of Text for Web Search</a>

Luckily, MatchZoo has already provided their benchmarking and analysis, but we can always try and reproduce it. (NDCG : Normalized Cumulative Discounted Gain)
<table>
  <tr>
    <th width=10%, bgcolor=#999999 >Models</th> 
    <th width=20%, bgcolor=#999999>NDCG@3</th>
    <th width="20%", bgcolor=#999999>NDCG@5</th>
    <th width="20%", bgcolor=#999999>MAP</th>
  </tr>
  <tr>
    <td align="center", bgcolor=#eeeeee> DSSM </td>
    <td align="center", bgcolor=#eeeeee> 0.5439 </td>
    <td align="center", bgcolor=#eeeeee> 0.6134 </td>
    <td align="center", bgcolor=#eeeeee> 0.5647 </td>
  </tr>
  <tr>
  	 <td align="center", bgcolor=#eeeeee> CDSSM </td>
  	 <td align="center", bgcolor=#eeeeee> 0.5489 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6084</td>
  	 <td align="center", bgcolor=#eeeeee> 0.5593 </td>
  </tr>
  <tr>
  	 <td align="center", bgcolor=#eeeeee> ARC-I </td>
  	 <td align="center", bgcolor=#eeeeee> 0.5680 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6317 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.5870 </td>
  </tr>
  <tr>
  	 <td align="center", bgcolor=#eeeeee> ARC-II </td>
  	 <td align="center", bgcolor=#eeeeee> 0.5647 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6176 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.5845 </td>
  </tr>
  <tr>
  	 <td align="center", bgcolor=#eeeeee> MV-LSTM </td>
  	 <td align="center", bgcolor=#eeeeee> 0.5818 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6452 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.5988 </td>
  </tr>
  <tr>
  	 <td align="center", bgcolor=#eeeeee> DRMM </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6107 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6621 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6195 </td>
  </tr>
  <tr>
     <td align="center", bgcolor=#eeeeee> aNMM </td>
     <td align="center", bgcolor=#eeeeee> 0.6160 </td>
     <td align="center", bgcolor=#eeeeee> 0.6696 </td>
     <td align="center", bgcolor=#eeeeee> 0.6297 </td>
  </tr>
  <tr>
  	 <td align="center", bgcolor=#eeeeee> DUET </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6065 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6722 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6301 </td>
  </tr>
  <tr>
  	 <td align="center", bgcolor=#eeeeee> MatchPyramid </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6317 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6913 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6434 </td>
  </tr>
  <tr>
  	 <td align="center", bgcolor=#eeeeee> DRMM_TKS </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6458 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6956 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6586 </td>
  </tr>
  <tr>
  	 <td align="center", bgcolor=#eeeeee> K-NRM </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6268 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6693 </td>
  	 <td align="center", bgcolor=#eeeeee> 0.6256 </td>
  </tr>
</table>

Along with the loss:

![alt-text](https://github.com/faneshion/MatchZoo/blob/master/docs/_static/images/matchzoo.wikiqa.loss.png)

Free datasets for Similarity Learning will also be added to [gensim-data](https://github.com/RaRe-Technologies/gensim-data) :

* [Quora Duplicate Questions](https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs) (already in gensim-data)
* [WikiQA](https://www.microsoft.com/en-us/download/details.aspx?id=52419)
* [Ubuntu Dialogue Corpus](https://github.com/rkadlec/ubuntu-ranking-dataset-creator)

# Deliverables

The whole project can be broken down into the following phases:
1. Setup of evaluation scheme for all the models which will be introduced
2. Collect/Implement models for comparisons, benchmarking and parameter tuning
3. Choose the best models and integrate them into gensim
4. Work on improving model

### Community Bonding Period (April 23 to May 14)
* Read papers relating to Similarity Learning
* Work on ongoing PRs
* Address existing issues and doubts on gitter and stack overflow


### Week 1 (May 14 to May 20)
* Discuss and create an evaluation pipeline for possible models to be included
* Evaluate current list of models to be implemented and make possible additions/deletions

(It's possible that the list of models may change at this point and accomodations will be made for the same.)

### Week 2 (May 21 to May 27)
* Develop or use existing implementation of DSSM, CDSSM, MV-LSTM
* Tune parameters and establish performance benchmark

### Week 3 (May 28 to June 3)
* Develop or use existing implementation of aNMM, ARC-I, ARC-II
* Tune parameters and establish performance benchmark

### Week 4 (June 4 to June 10)
* Develop or use existing implementation of DRMM, K-NRM, Match Pyramid
* Tune parameters and establish performance benchmark

### Midterm Evaluation 1 (June 11 to June 15)
* Work on feedback
* Catch up on any unseen work

### Week 5 (June 16 to June 24)
* Develop or use existing implementation of DUET
* Write a detailed blog with the benchmarks and process
* Make a final list of models to be integrated into gensim

### Week 6 (June 25 to July 1)
* Design an API for the new models to be integrated
* Start integrating models into gensim (make similar API, add streaming support, tests, documentation)

### Week 7 (July 2 to July 8)
* Continue integrating models into gensim (make similar API, add streaming support, tests, documentation)

### MidTerm Evaluation 2 (July 9 to July 13)
* Work on feedback

### Week 8 (July 14 to July 22)
* Finish up all model integrations
* Make code merge-ready

### Week 9 (July 23 to July 29)
* Start working towards making models tuned to get SOTA performance
* Discuss and evaluate current models for possible improvements
* Write blog about all the new models

### Week 10 (July 30 to August 5)
* Clean up code and merge it into `gensim/develop`
* Add datasets into `gensim-data`
* Release benchmark results comparing all models and all datasets
* Release code and tutorial showing demonstrations

### Final Submission (August 6 to August 14)
* Work on any feedback
* Get changes merged

**GSOC Complete!**

## About me
I am a 4th year Computer Science and Engineering student at Manipal Institute of Technology, Manipal, India. Before I got interested in training deep neural networks, I used to develop games. I spent a good deal of time developing [bearmomentum](https://drive.google.com/open?id=1SDXLPdGqfFJ86O8UhN-mHQAsU4sAAJ4i), a game about exploiting conservation of momentum to get a mama bear to meet her long lost child (using guns!). When I first learnt of word2vec, it felt like magic and my interests moved to Deep Learning. Several LSTMs, CNNs and GANs later, I am a big Deep Learning fan. I am also a student of the Udacity Self Driving Car Nanodegree where I made some awesome projects like a [Traffic Sign Classifier](https://github.com/aneesh-joshi/Traffic_Signs_Classifier), a [Behavioural Clone](https://github.com/aneesh-joshi/SelfDrivingCarBehaviouralClone) and a [Vehicle Detection system](https://github.com/aneesh-joshi/VehicleDetectionSystem). I am a big open source fan and have contributed to gensim and [OpenMined](https://github.com/OpenMined). Apart from all this technical stuff, I like making [digital art](https://www.instagram.com/aneesh._.joshi/) :)

## Gensim and I
I was first introduced to gensim during my internship in TheTextMiners, where I had to use word2vec and LSTMs for automatic information exctraction. I was instantly impressed by the ease of use gensim provided along with efficient implementations. From the time I have been involved in the development of gensim (almost a year ago), I have come to appreciate the inner intricacies even more.<br/>
For example, when I worked on my PR for adding `max_final_vocab` to Word2Vec, I discovered the focus of the maintainers on code clarity and efficiency, the not-so-popular yet amazing repository of [bounter](https://github.com/aneesh-joshi/bounter) and the useful [gensim-data](https://github.com/RaRe-Technologies/gensim-data) repository. Although the above mentioned PR took a month to get merged, I learnt a lot about gensim and open source. I feel inspired by gensim's mission to allow **gen**eration of **sim**ilar results along with their focus on writing industry ready code. I would be grateful if I was selected to work on gensim over this summer.
Regardless of the outcome, I will keep on hunting down issues and pester @menshikh-iv with questions :)

## Acknowledgements
* Menshikh Ivan
* Gordon Mohr

## References:
[1] [MatchZoo Repo](https://github.com/faneshion/MatchZoo) <br/>
[2] [Wikipedia Article](https://en.wikipedia.org/wiki/Similarity_learning)