#Implementation of AKSW topic coherence algorithm

##Abstract

Unsupervised learning methods like Latent Dirichlet Allocation and Latent Semantic Analysis are attractive methods to bring structure to otherwise unstructured text data. However they give no guarantees on the interpretability of their output by humans. This interpretability can be measured by topic coherence. A good model must have high topic coherence, i.e. be understandable by humans.
The "Agile Knowledge Engineering and Semantic Web (AKSW)" research group of Michael Röder recently found a topic coherence measure
that outperforms others

While they released an implementation of this algorithm in Java, no python implementation exists. The AKSW algorithm has applications beyond topic models. It can increase key word search efficiency in document searches, can be used to select advertising links that fit a web page and can also be used to improve automatic translations of web pages. Also, this algorithm goes beyond just comparing word pairs ad adds word probabilities and numeric comparison normalizations into the mix which is not conventional. Hence a python implementation of this algorithm seems like the need of the hour for the python data science world.

##Technical Details

As written in the [paper](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf), the following pipeline of four dimensions was followed to find the best topic coherence measure:

1. Segmentation  (set `S` of different kinds of segmentation) 
2. Confirmation measures eg. NPMI. (set `M`)
3. Word probability  estimators (set `P`)
4. Aggregate scalar values.(Function set `Sigma`)

Following was the workflow of the framework:

1. Word set `t` segmented into set of pairs of word subsets `S`.
2. Word probabilities `P` computed on given reference corpus.
3. Agreements `phi` computed by consuming `S` and `P` on pairs of `S`.
4. All those values aggregated to calculate single coherence value `c`.

Therefore the configuration space is `C = S X M X P X Sigma` (where `X` is the cross product of the sets).

Hence different permutations and combinations were obtained which also involved discovery of new coherence measures. A total of 237912 different coherences and parameterizations were tested. The best coherence measure (`C_v`) found was a new combination of the above mentioned parameters. It combines indirect cosine measure with the NPMI and boolean sliding window. This combination has been overlooked so far in the literature. Formally `C_v` is:

* __Segmentation__: S_one_set
* __Probability estimation__: Boolean sliding window with window size 110 (should be >50)
* __Confirmation measure__: Indirect cosine measure + NPMI
* __Aggregator__: Arithmetic mean

Gensim currently supports the Umass topic coherence as implemented in [#311](https://github.com/piskvorky/gensim/pull/311). Implementation of the best method from Roeder can build on top of that. 

##Schedule of deliverables

Please note that this timeline is tentative. Some parts may finish earlier than expected whereas some might take up more time if issues arise which need to be fixed before moving forward.

* __Today - April 22nd__:

    - Fix bugs, documentation. Get more comfortable with the codebase.

* __April 22nd - May 22nd__:

    - Study current implementation of coherence measure in Java. Start coding gists in python so as to understand the code. Keep contributing to Gensim.

* __May 23rd - June 5th__:

    - Ready a working implementation of `C_v` segmentation of `S_one_set` (23).

* __June 6th - June 21st__:

    - Ready a working implementation of boolean sliding window with size parameter. Each window is a virtual document. Apply boolean document to those virtual documents.

* __June 21st - June 28th__: __Midterm Evaluations__

    - Keep working on implementing boolean sliding window.

* __June 29th - July 19th__:

    - Work on implementing general formula for indirect confirmation measure (38) and NPMI (27).

* __July 20th - August 16th__:

    - Ready final algorithm. Should be built on top of existing U_mass implementation in Gensim.

* __August 17th - August 21st__:

    - Perform benchmark testing against datasets such as 20NewsGroups, New York times etc. Add examples, tests and improve documentation.

* __August 22nd - np.inf__:

    - Keep contributing.

__Note__: I will be having exams from 1st May - 12th May. In this period my contribution rate might drop but I will pick up as soon as my exams get over. Other than this period, I will be having no commitments and will be able to work on this project full time. If for any reason my code does not get merged into master, I will continue work beyond the summer to ensure that it becomes mergeable.

##Future Works

I intend to continue contributing to Gensim in the future. I would love it for Gensim to be the best NLP and IR library available in python. It would be wonderful if I could work beyond the summer to achieve this goal.

##Open Source Experience

I have been contributing to Scikit-learn machine learning organisation in python since December 2015. I have contributed some patches over this period and thus I'm already comfortable with the contributing procedure. I am already comfortable with using git, mailing lists, automatic testing etc and thus am aware of the general workflow followed by organisations. Following are some of the contributions I have made for scikit-learn:

1. https://github.com/scikit-learn/scikit-learn/pull/6221 - (merged) - LabelBinarizer single label case now works for sparse and dense case.
2. https://github.com/scikit-learn/scikit-learn/pull/6176 - [MRG] - Added rebasing and squashing section to contributing.rst.
3. https://github.com/scikit-learn/scikit-learn/pull/6404 - (merged) - Added memory workaround to DBSCAN doc.
4. https://github.com/scikit-learn/scikit-learn/pull/6229 - (merged) - LabelEncoder now works for 0-D arrays.
5. https://github.com/scikit-learn/scikit-learn/pull/6213 - (merged) - Added link to profile in whats_new.rst.
6. https://github.com/scikit-learn/scikit-learn/pull/6182 - (merged) - SKF raises error if all n_labels for individual classes < n_folds.
7. https://github.com/scikit-learn/scikit-learn/pull/6173 - (merged) - FeatureHasher now accepts string values.
8. https://github.com/scikit-learn/scikit-learn/pull/6153 - (merged) - Doc: Fix redundant doc for f_regression.
9. https://github.com/scikit-learn/scikit-learn/pull/6139 - (merged) - Fixed doc in lfw.py.
10. https://github.com/scikit-learn/scikit-learn/pull/6135 - (merged and backported to 0.17.X by [@jakirkham](https://github.com/jakirkham)) - FIX #5608 in randomized_svd flip sign.
11. https://github.com/scikit-learn/scikit-learn/pull/6134 - (merged) - FIX #6132 broken link.
12. https://github.com/scikit-learn/scikit-learn/pull/6041 - (merged) - Fix in plot_out_of_core_classification.py

##About me

My name is Devashish Deshpande. I'm a third year undergraduate student of BITS Pilani, Goa campus. I'm currently pursuing a BE(hons.) in computer science. I am very interested in the field of machine learning and data science and have done a few projects and courses in these areas. The courses I have completed include a course on data mining and an online course on Big Data and Machine Learning offered by BerkeleyX. I am currently pursuing a course on Information Retrieval and the popular MOOC course offered by Andrew Ng. Apart from these, I have done projects on signal analysis and natural language processing which can be found on my GitHub profile. All of these projects involved me working with python hence I'm comfortable with the language. Other than these projects, I have also done developmental projects for different organisations in languages such as Java, C++ and C# and therefore have sufficient knowledge of the object oriented approach.

##Why this project

I came across the problem of topic modelling while working on one of my NLP projects and found it very intriguing. With all the algorithms we have right now for topic modelling, if they don't give a guarantee of human interpretability they cannot be trusted 100% of the time. Human understandabiility is the most important task of these algorithms. Topic coherence can ensure this and thus give added value to topic modelling algorithms.

##Appendix
* http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf
* https://github.com/piskvorky/gensim
* https://github.com/piskvorky/gensim/pull/311
* https://github.com/piskvorky/gensim/issues/278
* https://github.com/AKSW/Palmetto
* http://palmetto.aksw.org/palmetto-webapp/
