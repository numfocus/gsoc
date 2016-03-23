# Title

Dynamic Topic Model for Gensim.
## Abstract

Dynamic Topic Models [\[1\]](http://www.stat.uchicago.edu/~lafferty/pdf/dtm.pdf) [\[2\]](https://en.wikipedia.org/wiki/Dynamic_topic_model) are used to model the evolution of topics in a corpus, over time. The Dynamic Topic Model is part of a class of probabilistic topic models, and unlike the previous models developed [\[3\]](https://www.cs.princeton.edu/~blei/papers/Blei2012.pdf), takes into account time-series data. The idea behind this project proposal is to implement Dynamic Topic Models while taking care of Gensim's philosophy of being memory-independent and robust. There is already an academic implementation available in C++ [\[4\]](https://code.google.com/archive/p/princeton-statistical-learning/downloads), but it is not well-supported and the python wrapper already implemented [\[5\]](https://radimrehurek.com/gensim/models/wrappers/dtmmodel.html) uses this rather flimsy academic implementation. Completing this project would be of great use in the industry, and in academia [\[6\]](http://nlp.stanford.edu/pubs/hall-emnlp08.pdf) , [\[7\]](http://jmlr.org/proceedings/papers/v28/shalit13.pd) where having a quick implementation of Dynamic Topic Models would do wonders, especially in the Humanities [\[8\]](http://sappingattention.blogspot.in/2013/01/keeping-words-in-topic-models.html) where evolution of topics in literature and sociology is heavily studied. This work can be extended to include Document Influence Models [\[9\]](https://www.cs.princeton.edu/~blei/papers/GerrishBlei2010.pdf), where the influence of certain documents affect topic evolution. My personal interest in this project is motivated by my extensive use of Dynamic Topic Models for my research work on tracking evolution of Computer Science research topics on academic graph data sets, and have a fair amount of experience on working with various topic models [\[10\]](https://github.com/santonus/bigscholarlydata).

## Technical Details

Dynamic Topic Models were first described academically here [\[1\]](http://www.stat.uchicago.edu/~lafferty/pdf/dtm.pdf). One of the main tenets of the Dynamic Topic Model is that the topics in time `t` have evolved from the topics in time `t-1`. To make this happen, there is a relationship between the hyper parameters associated with each time slice. All of this is a stark difference from your typical LDA implementation, and the challenges/differences in DTM start making themselves clear now.
For starters, we can't sample documents and the topics they've evolved from the way we normally do. In typical language modeling applications, Dirichlet distributions are used to model uncertainty about the distributions over words. However, the Dirichlet is not amenable to sequential modeling. Blei's paper describes the models for drawing your parameters (`α`, `β`), and the topics. As described by him - "Our approach is thus to model sequences of compositional random variables by chaining Gaussian distributions in a dynamic model and mapping the emitted values to the simplex." How this works into our code would be a topic of discussion.

We are soon faced with another small problem - "Working with time series over the natural parameters enables the use of Gaussian models for the time dynamics; however, due to the nonconjugacy of the Gaussian and multinomial models, posterior inference is intractable."  He goes on to explain a  variational method for approximate posterior inference. They suggest Kalman Filtering [\[11\]](https://en.wikipedia.org/wiki/Kalman_filter) and Wavelet Regression as ways to go about this. 

Other things to keep in mind while going about this project is Gensim's philosophy of not loading in memory, and addressing the lack of a timed stream functionality. 

## Schedule of Deliverables

### Community Bonding Period

- Before the official time period begins I intend to fix some of the outstanding issues [\[#113\]](https://github.com/piskvorky/gensim/issues/113) and add a few basic functionality (such as [\[#64\]](https://github.com/piskvorky/gensim/issues/64), among others.

### May 25th -  June 7th

- Work on sampling the documents, words, and topics, according to Gaussian State Space Model described.


### June 8th - June 21th

- Can start with tests to make sure the sampling methods work right.
- Figure out how to workaround posterier inference approximation and add functionality of Kalman Filtering or Wavelet Regression if needed.
- The basic building blocks of DTM are being assembled so far.

### June 22nd - July 5th

- Engineer in time-slices to our existing system and test our above results over a range.
- Since time-slices have not been introduced in Gensim before, we might need to spend some time working on
how to build our API for this. 

### July 6th - July 19th

- By now we should have a crude representation of Dynamic Topic Model set up.
- Put our building blocks together and make sure the results are at least on par with the current gensim wrapper.
- Test using the corpus described here - [\[12\]](http://arxiv.org/pdf/1510.03797.pdf).

### July 20th - August 2nd

- Now that we have our Dynamic Topic Model, it's time to test and fine-tune and give insights.
- There are already papers which talk and articles which discuss the use of Dynamic Topic Modelling and their insights [ [\[13\]](http://ceur-ws.org/Vol-974/lakdatachallenge2013_01.pdf)  [\[14\]](http://jmlr.org/proceedings/papers/v28/shalit13.pdf)].
- After tuning on different scenarios and the corpuses the above references use, we can see how this implementation holds up to standard previous implementations such as [\[4\]](https://code.google.com/archive/p/princeton-statistical-learning/downloads)].

### August 3rd - August 16th

- Work on possible bugs and problems our implementation will have at this point. Make sure all edge and corner cases are handled.
- As mentioned in the project deliverables, time, memory usage and accuracy is what we will primarily concern ourselves with at this point. 

### August 17th - August 21th 19:00 UTC

- By now there will be a good amount of insight of how our model works, and knowledge of parameter selection, number of topics will be known according to different scenarios.
- Documentation of the same, cleaning, and seeing how we can set up our code to implement multiple cores should be discussed, if not completed.

## Future works

The project timeline described above does not include an implementation across multiple cores, but an extension to include this can be attempted. Ideas used in Dynamic Topic Model are also used in Document Influence Model, and this could be a direction to go in after Dynamic Topic Model is implemented.

## Open Source Development Experience

My first tryst with Open Source Development was when I was learning it through [Code Combat](https://codecombat.com/), a popular game used to learn Python and JavaScript. While playing, I stumbled across an error [\[#25\]](https://github.com/differentmatt/filbert/issues/25) in the sorting function. This had me stuck, so I decided to dive in and fix the bug [\[#42\]](https://github.com/differentmatt/filbert/pull/42) - it was quite nice to see my contribution in a game I enjoyed playing. Since, I have been largely working on research work in data science and stepped away from open source contribution, but some of this academic work has been made public  [\[15\]](https://github.com/bhargavvader/CASApythonPort), and I hope it has helped those who needed help in porting MATLAB to Python and do work in Blind Source Separation. I look forward to getting back to regularly contributing to the Open Source and Machine Learning Community.

## Academic Experience

I am a 3rd year Undergraduate Computer Science student enrolled at BITS Pilani University, India. I have undertaken a variety of Data Science projects and coursework, my most recent in being analyzing growth of topics in Computer Science research areas using the [Microsoft Academic Graph](http://research.microsoft.com/en-us/projects/mag/). I have worked in a Machine Learning [start-up](https://zero.ai) and undertaken research projects on Mining High-Dimensional Datasets with pioneering researchers such as [Prof. Ashwin Srinivasan](http://www.bits-pilani.ac.in/goa/ashwin/profile). I am attaching my [CV](https://drive.google.com/a/goa.bits-pilani.ac.in/file/d/0By80y9AXd1WsOVdBOGdWRzNxaWM/view) as well, for a more detailed explanation of my previous experiences. 

## Why this project?

Implementing Dynamic Topic Models came from a rather selfish point of view - I wanted a python implementation of it to use in my undergraduate research project! I have a fair amount of experience in the mathematical theory and implementation of Topic Models because of my use of the same in my work with Professors at my University, and in pet projects. I am glad to have a formal platform to implement Dynamic Topic Models and look forward to contributing to Gensim towards this end. It would also be a great joy to be a part of a project which will most definitely be used by a large number of people in industry and academia. 

## Appendix

Find below a list of all the web articles and research papers described above.

 1 - http://www.stat.uchicago.edu/~lafferty/pdf/dtm.pdf
 2 - https://en.wikipedia.org/wiki/Dynamic_topic_model
 3 - https://www.cs.princeton.edu/~blei/papers/Blei2012.pdf
 4 - https://code.google.com/archive/p/princeton-statistical-learning/downloads
 5 - https://radimrehurek.com/gensim/models/wrappers/dtmmodel.html
 6 - http://nlp.stanford.edu/pubs/hall-emnlp08.pdf
 7 - http://jmlr.org/proceedings/papers/v28/shalit13.pd 
 8 - http://sappingattention.blogspot.in/2013/01/keeping-words-in-topic-models.html
 9 - https://www.cs.princeton.edu/~blei/papers/GerrishBlei2010.pdf
 10 - https://github.com/santonus/bigscholarlydata
 11 - https://en.wikipedia.org/wiki/Kalman_filter
 12 - http://arxiv.org/pdf/1510.03797.pdf
 13 - http://ceur-ws.org/Vol-974/lakdatachallenge2013_01.pdf
 14 - http://jmlr.org/proceedings/papers/v28/shalit13.pdf