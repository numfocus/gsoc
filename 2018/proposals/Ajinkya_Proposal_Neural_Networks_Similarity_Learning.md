# Neural networks (similarity learning, state-of-the-art language models)

## Contact Information

**Name :** Ajinkya Takawale<br/>
**University :** [Indian Institute of Technology, Kharagpur](http://www.iitkgp.ac.in/)<br/>
**Email-id :** [ajinkya.takawale@iitkgp.ac.in](mailto:ajinkya.takawale@iitkgp.ac.in) ,      [ajinkya.takawale97@gmail.com](mailto:ajinkya.takawale97@gmail.com)<br/>
**GitHub username :** [ajinkyaT](https://github.com/ajinkyaT)<br/>
**Time-zone :** UTC + 5:30

## Abstract

Gensim is an open source Python library for natural language processing, with a focus on topic modeling and semantic/syntactic analysis . Gensim stands for **gen**erate **sim**ilar, and has traditionally focused on working with vector representations for words, sentences and documents, followed by e.g. cosine similarity to assess the similarity between these word/sentence/document vectors: sim = cossim(vector1, vector2). 

Recent advancements in the applications of CNN, RNN, and their variants like LSTM with Attention Mechanism have claimed to obtain the state of the art results on the NLP tasks. Though there's a lot of hype around the Neural Networks applications in NLP, this project aims to find some novel and quantifiable use cases of one such Neural Network called "Siamese Neural Network"  for the "similarity learning" task in NLP domain, which has been traditionally dealt with by representing the texts as vectors. 

## Technical Details

### Background

#### Similarity Learning:

As mentioned on the wikipedia, Similarity learning[\[1\]](https://en.wikipedia.org/wiki/Similarity_learning) is an area of supervised machine learning in artificial intelligence. It is closely related to regression and classification, but the goal is to learn from examples a similarity function that measures how similar or related two objects are. It has applications in ranking, in recommendation systems, visual identity tracking, face verification, and speaker verification.

 In the NLP domain, one usually creates a corpus in the Vector Space Model and transform it between different vector spaces. A common reason for such a charade is that we want to determine a similarity between pairs of documents, or the similarity between a specific document and a set of other documents (such as a user query vs. indexed documents). As it turns out it is not so important for one to represent the texts as vectors as a required intermediate step; it is only important to assess the similarity: sim = blackbox(text1, text2). Methods that don't use vectors or methods that use vectors only internally as an ancillary step are discussed further.

#### Siamese Neural Network: 
Siamese neural network[\[2\]](http://yann.lecun.com/exdb/publis/pdf/chopra-05.pdf) is a class of neural network architectures that contain two or more identical subnetworks. Identical here means they have the same configuration with the same parameters and weights. Parameter updating is mirrored across both subnetworks.

![](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2017/09/07/practical_gan_3.gif)

Siamese neural networks are popular among tasks that involve finding a similarity or a relationship between two comparable things. Some examples are paraphrase scoring, where the inputs are two sentences and the output is a score of how similar they are; or signature verification, where figure out whether two signatures are from the same person. Generally, in such tasks, two identical subnetworks are used to process the two inputs, and another module will take their outputs and produce the final output. The picture below is from Bromley et al (1993)[\[3\]](https://papers.nips.cc/paper/769-signature-verification-using-a-siamese-time-delay-neural-network.pdf). They proposed a Siamese architecture for the signature verification task.

![](https://qph.ec.quoracdn.net/main-qimg-1a64e6b37db230538c7b82d7abbf34f6) <!-- .element height="50%" width="50%" -->
 
### Potential Use Cases  

#### Similar Question Retrieval:

Community Question Answering services like Quora, StackOverflow etc. provide a platform for interaction with experts and help users to obtain precise and accurate answers to their questions. The time lag between the user posting a question and receiving its answer could be reduced by retrieving similar historic questions from the archives. This paper[\[4\]](https://www.semanticscholar.org/paper/Together-we-stand%3A-Siamese-Networks-for-Similar-Qu-Das-Yenala/62a97ac04e742ad1513cf164760e4d6a25d93203) discusses implementation of Siamese Convolutional Neural Network to find the semantic similarity between the current and the archived questions.


#### Finding Duplicate Questions:

On a large Community Question Answering service like Quora one may encounter a situation where duplicate questions arise which may lead to confusion to seek the right answer. It is also an unpleasant situation for a writer to answer same question twice or to a reader to find a hyperlink to the answer previously written for a similar question.
A competition[\[5\]](https://www.kaggle.com/c/quora-question-pairs) was hosted on the Kaggle previous year to solve this exact issue. 

This Tensorflow implementation of Siamese Neural Network[\[6\]](https://github.com/dalmia/Quora-Question-Pairs) discusses how it managed to obtain a rank within the top 25%.

Here is a Keras implementation[\[7\]](https://github.com/bradleypallen/keras-quora-question-pairs) for the above challenge. It benchmarks other approaches aswell.

#### Finding Relevant Answers to the Questions:

In Question Answering, some recent studies have used Siamese architectures to score relevance between a question and an answer candidate[\[8\]](https://arxiv.org/abs/1512.05193). So one input is a question sentence, the other input is an answer, and the output is how relevant is the answer to the question. Questions and answers don’t look exactly the same, but if the goal is to extract the similarity or a connection between them, a Siamese architecture can work well, too.

#### Evaluating Semantic Similarity between Sentences:

So far most of the sentence similarity tasks were traditionally dealt with transforming text into vector spaces[\[9\]]( https://spacy.io/usage/vectors-similarity) or using conventional Machine Learning models like SVM or Tree based models like XGBoost, Random Forest etc. after pre-processing text.

This paper introduces use of Siamese recurrent architectures for learning sentence similarity[\[10\]](https://www.aaai.org/ocs/index.php/AAAI/AAAI16/paper/view/12195). Here is a Theano implementation for the same[\[11\]](https://github.com/aditya1503/Siamese-LSTM).

### Implementation Details

Convolutional Neural Network (CNN) and Recurrent Neural Network (RNN), the two main types of DNN architectures, are widely explored to handle various NLP tasks. CNN is supposed to be good at extracting position-invariant features and RNN at modeling units in sequence. The state of the art on many NLP tasks often switches due to the battle between CNNs and RNNs. This paper compares CNN and RNN on a wide range of representative NLP tasks[\[12\]](https://arxiv.org/abs/1702.01923). 

I would therefore like to benchmark following approaches on the similarity learning task. 

- Siamese Convolutional Neural Network
- Siamese Convolutional Neural Network with Attention Mechanism [\[13\]](https://arxiv.org/pdf/1707.01378.pdf) 
- Siamese LSTM Neural Network
- Siamese LSTM Neural Network with Attention Mechanism
- Siamese Bi-directional LSTM Neural Network with Attention Mechanism
- Combination of above methods.
- Conventional vector representations of the texts followed by sliding window Convolutional Neural Network [\[9\]](https://spacy.io/usage/vectors-similarity)

Apart from that it is also interesting to compare results using different embedding methods like word2vec, GloVe and other methods. We can further compare if GRU can be a good substitute for LSTM.

**Keras** is a high-level neural networks API, written in Python and capable of running on top of TensorFlow, CNTK, or Theano. It was developed with a focus on enabling fast experimentation. I will therefore primarily try to build models using Keras, given that I have some past experience with it. TensorFlow and PyTorch will be used if attempts to implement in Keras fails owing to the flexibility of these frameworks.

Following data set's are available for the above mentioned tasks:

- First Quora Dataset Release: Question Pairs[\[14\]](https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs) [***Binary Classification***]
- The Stanford Natural Language Inference (SNLI) Corpus[\[15\]](https://nlp.stanford.edu/projects/snli/) [***Multi Class Classification***]
- The Multi-Genre NLI Corpus [\[16\]](https://www.nyu.edu/projects/bowman/multinli/) [***Multi Class Classification***]
- Ubuntu Dialogue Corpus v2.0 [\[17\]](https://github.com/rkadlec/ubuntu-ranking-dataset-creator) [***Binary Classification***]

If required, I can also scrape publicly available data from the Internet. **Scrapy**[\[18\]](https://scrapy.org/)is a powerful web scraping library in Python known for its asynchronous tasks management.

### Formal Description of The Task

#### Step One (Encoding): 
Convert text to encoding. This could be done using Embedding vectors such as GloVe or word2vec, or by using character level modelling methods such as SentencePiece [\[19\]](https://github.com/google/sentencepiece), which is language independent and takes care of out of vocabulary words.

#### Step Two (Training): 

Once we have encoding for our text we can then pass it to our Siamese Network. If our task is just to find relatedness of the two texts we can use contrastive loss which is a distance-based loss function. Like any distance-based loss, it tries to ensure that semantically similar examples are embedded close together and is calculated on pairs. Taken from the paper [\[20\]](http://yann.lecun.com/exdb/publis/pdf/hadsell-chopra-lecun-06.pdf), 

![](https://qph.ec.quoracdn.net/main-qimg-46f7ff378e2bbbed04a3ada34ce630da)

Here, Dw is any distance function. A simple case can just be an Euclidean distance between outputs of X1 and X2 vectors (X1 and X2 will be the network’s output of each input of the pair)

Here is a PyTorch implementation for the same:

```python
loss_contrastive = torch.mean(
 (1-label)*torch.pow(euclidean_distance, 2) + 
 (label)*torch.pow(torch.clamp(self.margin-euclidean_distance, min=0.0), 
 2))
```

The outputs from the twin networks will give us relatedness score between two texts, which can be simply Cosine , Euclidean  or Manhattan distance. We may also stack another dense layer to predict binary output of 1 (similar) or 0 (not similar).  Below is how our architecture might look like.

![](https://cdn-images-1.medium.com/max/800/1*SZM2gDnr-OTx9ytVKQEuOg.png)

Figure taken from MaLSTM's architecture [\[21\]](http://www.mit.edu/~jonasm/info/MuellerThyagarajan_AAAI16.pdf) — Similar color means the weights are shared between the same-colored elements

In some cases datasets available demands training for multi-class problems. E.g The Stanford Natural Language Inference (SNLI) Corpus and the Multi-Genre NLI Corpus both have three different class of labels for a given pair of sentences which are neutral, entailment and contradiction. Below is the example taken from Multi-Genre NLI Corpus: 

| Sentence 1        | Sentence 2           | Label  |
| ------------- |:-------------:| -----:|
| The Old One always comforted Ca'daan, except today.      | Ca'daan knew the Old One very well. | neutral |
| yes now you know if if everybody like in August when everybody's on vacation or something we can dress a little more casual or | August is a black out month for vacations in the company.      |   contradiction |
| At the other end of Pennsylvania Avenue, people began to line up for a White House tour. | People formed a line at the end of Pennsylvania Avenue.| entailment|

In this cases we might be interested in extracting distance features from Siamese Network as an input to further classify them under different labels. The paper, "Learning Sentence Similarity with Siamese Recurrent Architectures"[\[21\]](http://www.mit.edu/~jonasm/info/MuellerThyagarajan_AAAI16.pdf) and this article from Quora Engineering team[\[22\]](https://engineering.quora.com/Semantic-Question-Matching-with-Deep-Learning) discusses how they used distance and angle between two vectors from the output of Siamese Network to feed them into a classifier. Below image taken from Quora article shows how our architecture might look like.

![](https://qph.ec.quoracdn.net/main-qimg-9aeac46862d3c2f570fef5ada09a57ec)

#### Step Three (Feature Representation): 

Once we have trained our network, we can then generate vector representation of features from the output of any twin network (since they both share same weights) for all the documents in a database to demonstrate potential use cases of similar text retrieval. A contrastive loss function during training can constrain the network to learn an vector space where a specific distance metric like Euclidean or Cosine corresponds to semantic similarity. We can then index these document vectors to retrieve and rank them according to the given query. This approach was implemented in this paper, "Learning Deep Representations of Medical Images using Siamese CNNs with Application to Content-Based Image Retrieval" [\[23\]](https://arxiv.org/abs/1711.08490)

### Related Work

This repository[\[24\]](https://github.com/bradleypallen/keras-quora-question-pairs) explains implementation of Keras model on the Quora Question Pairs dataset[\[14\]](https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs). The model architecture is based on the Stanford Natural Language Inference [\[15\]](https://nlp.stanford.edu/projects/snli/) benchmark model developed by Stephen Merity [\[25\]](https://github.com/Smerity/keras_snli). It also provides benchmark results for various architectures used.

This project[\[26\]](https://github.com/tlatkowski/multihead-siamese-nets)  is similar to the one we are trying to accomplish. The repository contains implementation of Siamese Neural Networks in Tensorflow built based on 3 different and major Deep Learning architectures:

- Convolutional Neural Networks
- Recurrent Neural Networks
- Multihead Attention Networks

The project is still underway and comparison of above methods is yet to be done.  Indeed studying the code base of this project would surely help in our project. 

## Schedule of Deliverables

Tasks for this project will be divided into two milestones

#### Milestone 1: Getting state-of-the art or comparable result

- Benchmark results obtained from different variants of Siamese Network mentioned above and conventional vector representation methods.
- Try different encoding methods like using word2vec, Glove, SentencePiece [\[19\]](https://github.com/google/sentencepiece) (Character level modeling).
- Compare results from different datasets mentioned above.

#### Milestone 2: Developing possible use cases and API

- Once we have our model trained we can further use it to demonstrate potential use cases mentioned above.
- Also a model trained on sufficiently large corpus could be used as a demo API for sentence similarity task like Space demo[\[27\]](https://demos.explosion.ai/similarity/?text1=This%20is%20a%20sentence&text2=This%20is%20another%20sentence). 

I hope to achieve Milestone 1 before midterm evaluation II. My time commitment will be 35-40 hours per week between April 28 - July 14 and 30-35 hours per week between July 15 - August 14. Below is task divided by dates.

#### April 23 - May 14 (Community Bonding Period):

- I will be having my end semester exams till April 27th. So my time will be limited in that period.
- I have primarily used Keras for all the Deep Learning tasks and haven't yet used Pytorch, so I will try to get my hands dirty on PyTorch and also TensorFlow.
- I will also start maintaining a blog describing my weekly work summary.
- Although most of the datasets required for the task have been mentioned above, if needed I will also try to gather other data sources or try to create one from the publicly available information from the Internet.

#### Week I ( May 14 - May 20)
- Start with Quora Question Pair dataset to benchmark.
- Start with Non-Neural Network approaches like bog of words, TF-IDF.
- Finish Non-Neural Network approaches. 
- Finish vector similarity methods like sliding window Convolutional Network described here[\[9\]](https://spacy.io/usage/vectors-similarity).


#### Week II ( May 21 - May 27)

- Finish all non-Siamese approaches.
- Start with Convolutional variants of Siamese Network mentioned in technical details.

#### Week III ( May 28 - June 3)

- Try experimenting with different text encoding methods.
- Finish different variants of Convolutional Siamese Network.
- Try the best method found on Ubuntu Dialogue Corpus v2.0
- Keep maintaining blog!

#### Week IV (June 4 - June 10)

- Refine and debug everything so far.
- By now I hope to achieve reasonably good result by Convolutional approach.
- Summarize everything done so far on the blog.

#### Mid Term Evaluation I (June 11 - June 15)

Work on feedback/review (if any)

#### Week V ( June 18 - June 24)

- Start with Recurrent variants of Siamese Network.
- Try experimenting with different text encoding methods.

#### Week VI ( June 25 - July 1) 

- Finish different variants of Recurrent Siamese Network.
- Try the best method found on Ubuntu Dialogue Corpus v2.0
- Keep maintaining blog!

#### Week VII ( July 2 - July 8)

- Refine and debug everything so far.
- By now I hope to achieve reasonably good result by Recurrent/LSTM approach.
- Summarize everything done so far on the blog.
- Achieve Milestone 1

#### Mid Term Evaluation II (July 9 - July 13)

Work on feedback/ review, if any.
Refine/ debug

#### Week VIII ( July 16 - July 22)

- Demonstrate potential use case of multi-class classification problem on the Multi-Genre NLI Corpus [\[16\]](https://www.nyu.edu/projects/bowman/multinli/)
- Demonstrate potential use case of similar questions retrieval on the Ubuntu Dialogue Corpus v2.0 [\[17\]](https://github.com/rkadlec/ubuntu-ranking-dataset-creator)

#### Week IX ( July 23 - July 29)

- Develop an API for sentence similarity using best pre-trained model.
- Start making IPython Notebooks for tutorials.


#### Week X ( July 30 - Aug 5)
- Finish working on API and tutorials
- Summarize all the work and start documentation.

#### Submit final work ( Aug 6 - Aug 14)

- Finalize codes and documents for final submission
- Work on feedback/review, if any.
- Wrap up everything
 

## More About Me

I am a third year undergraduate at IIT Kharagpur majoring in Mechanical Engineering. Towards the end of my freshman year I discovered this wonderful field of Machine Learning and its applications. Intrigued by this field, I completed multiple certified MOOC's on the topics such as Data Science and Machine Learning. After exploring the field for quite some time then and being fascinated by the idea of automated future, I decided to dedicate my near future to the field of Machine Learning applications. 

So far I have done three internships in the field of Data Science and Machine Learning with the recent one being to related to finding potential use cases of Deep Learning applications in the medical images. Two of my internships were related to NLP, one in sentiment analysis of reviews and the other one developing query based content retrieval system using tf-idf similarity. Apart from that I regularly take part in Machine Learning competitions and Hackathons and had also won in one of them wherein I got a chance to attend DataHack Summit 2017[\[28\]](https://www.analyticsvidhya.com/datahacksummit/), India's largest conference on Artificial Intelligence and Machine learning. 

Through my previous experiences, I have gained  proficiency in common Python Libraries associated with data processing and Machine Learning tasks like **Numpy, Pandas, Matplotlib, scikit-learn, Keras and TensorFlow**. Below is the list of some of my past work done relevant for this project.

- Deep Learning for Medical Images[\[29\]](https://github.com/ajinkyaT/RoroData_internship)
- Following programming assignments completed in the Sequence Models Course[\[30\]](https://github.com/ajinkyaT/Coursera_Sequence_Models) 
  * Character-Level Language Modeling
  * Music generation with LSTM
  * Neural Machine Translation with Attention
  * Trigger word detection from speech data
    
- Simple Twitter Sentiment Analysis using TextBlob Python library[\[31\]](https://github.com/ajinkyaT/twitter_sentiment_analysis)
- Web crawler written for scraping amazon.in data[\[32\]](https://github.com/ajinkyaT/ori/blob/master/ama.py)

I am also attaching my [CV](https://drive.google.com/open?id=0Byqx076qBK7jcGtsVFc1dVpUWmc) for a more detailed explanation of my previous experiences.

I have opened a pull request for this project here [#1976](https://github.com/RaRe-Technologies/gensim/pull/1976) 

## Why this project?

So far Gensim only has an option of Latent Semantic Analysis and doc2vec (paragraph2vec) for handling text similarity tasks. I would therefore like to propose a benchmark model trained on sufficiently large corpus like Ubuntu Dialogue Corpus to be used for demo api like this implementation from Spacy [\[27\]](https://demos.explosion.ai/similarity/?text1=This%20is%20a%20sentence&text2=This%20is%20another%20sentence). 

I am also planning to obtain Master's in Data Science after getting relevant work experience in this field. A project like this will add a lot of value to my application for the program. If possible, I would also like to contribute to Gensim after GSOC period as an Incubator program student. 

## Acknowledgement 

Ivan Menshikh [\[GitHub\]](https://github.com/menshikh-iv). Thank you for giving valuable feedback and replying to my emails!
## Appendix

Below is a list of all the web articles, GitHub repositories and research papers mentioned in the proposal.

1. https://en.wikipedia.org/wiki/Similarity_learning
2. http://yann.lecun.com/exdb/publis/pdf/chopra-05.pdf
3. https://papers.nips.cc/paper/769-signature-verification-using-a-siamese-time-delay-neural-network.pdf
4. https://www.semanticscholar.org/paper/Together-we-stand%3A-Siamese-Networks-for-Similar-Qu-Das-Yenala/62a97ac04e742ad1513cf164760e4d6a25d93203
5. https://www.kaggle.com/c/quora-question-pairs
6. https://github.com/dalmia/Quora-Question-Pairs
7. https://github.com/bradleypallen/keras-quora-question-pairs
8. https://arxiv.org/abs/1512.05193
9. https://spacy.io/usage/vectors-similarity
10. https://www.aaai.org/ocs/index.php/AAAI/AAAI16/paper/view/12195
11. https://github.com/aditya1503/Siamese-LSTM
12. https://arxiv.org/abs/1702.01923
13. https://arxiv.org/pdf/1707.01378.pdf
14. https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs
15. https://nlp.stanford.edu/projects/snli/
16. https://www.nyu.edu/projects/bowman/multinli/
17. https://github.com/rkadlec/ubuntu-ranking-dataset-creator
18. https://scrapy.org/
19. https://github.com/google/sentencepiece
20. http://yann.lecun.com/exdb/publis/pdf/hadsell-chopra-lecun-06.pdf
21. http://www.mit.edu/~jonasm/info/MuellerThyagarajan_AAAI16.pdf
22. https://engineering.quora.com/Semantic-Question-Matching-with-Deep-Learning
23. https://arxiv.org/abs/1711.08490
24. https://github.com/bradleypallen/keras-quora-question-pairs
25. https://github.com/Smerity/keras_snli
26. https://github.com/tlatkowski/multihead-siamese-nets
27. https://demos.explosion.ai/similarity/?text1=This%20is%20a%20sentence&text2=This%20is%20another%20sentence
28. https://www.analyticsvidhya.com/datahacksummit/
29. https://github.com/ajinkyaT/RoroData_internship
30. https://github.com/ajinkyaT/Coursera_Sequence_Models
31. https://github.com/ajinkyaT/twitter_sentiment_analysis
32. https://github.com/ajinkyaT/ori/blob/master/ama.py
