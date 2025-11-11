# Training and Topic Visualizations

## Abstract

Knowing about the progress and performance of a model, as we train them, could be very
helpful in understanding it’s learning process and makes it easier to debug and optimize them. It
could also help us affirm the results from our models or inspect them in case of counterintuitive
behaviors. Hence, I aim to provide a step-by-step process of visualizing training statistics for
people, who want to keep a tab on learning processes of their gensim models, and want to
optimize it through experimentation with hyperparameters.

The next phase of my project would aim to introduce and build a visualization module based on
Gensim models and features. This would allow the interactive exploration of applications based
on Gensim and would also enable users to do a qualitative assessment of their models and
analyze the results. I aim to focus on implementing a visual framework for the exploration of
topic models, taking cues from [TMVE](http://www.princeton.edu/~achaney/tmve/wiki100k/browse/topic-presence.html), [pyLDAvis](https://github.com/bmabey/pyLDAvis), and add more options to visualize data
attributes and compare across the different topic models.

This work can naturally be extended to various other features in Gensim and to the upcoming
ones, to have an associated visualization.

## Technical Details

First phase - Training statistics visualization:
Implement the logging of evaluation metrics (cumulative loss after each epoch and alpha) in
Word2vec/Doc2vec in python and cython versions, and the model specific metrics (coherence
and perplexity) of LDA. The next step would be to write a notebook explaining the steps to
visualize these training statistics using TensorBoard summary plots.

Second phase - Topic modelling visualizations:
The visualizations will primarily be built using d3.js. They will be implemented for use within a
Jupyter notebook and can also be saved to a stand-alone HTML file for using it in web browser.
A tutorial notebook explaining an example usage will also be added for every visualization.

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

- Before starting with the project, I will resolve the remaining parts of ​[497](https://github.com/RaRe-Technologies/gensim/issues/497). [PR-1121](https://github.com/RaRe-Technologies/gensim/pull/1121)
already have the gensim’s core dependencies in place so I’ll add the external packages
of wrappers and create the official image with documentation.

- Being a part of gensim community, I’ll continue to help around with issues and bugs, and
answer queries on the community channels of gensim. To keep track of my project, I will
be posting my weekly blog here.

### May 29th - June 3rd

- Introduce loss/alpha logging statements for word2vec/doc2vec.
- Introduce perplexity/coherence logging statements for LDA.
- Visualize the example case using TensorBoard.

### June 5th - June 16th

- Write Jupyter notebook tutorial of how these logged parameters can be visualized in
TensorBoard.
- Put the tutorial for review by anyone/people requesting the logging feature.

### June 19th - June 23th, End of Phase 1

- Blog about how these training stats can be used to optimize and understand our models.

### June 26 - June 30th, Begin of Phase 2

- I’ll discuss my visualization ideas for topic models, with the mentors and get reviews on
it. The design and features for the visualizations will be finalized in this period.

- Decide the API design, figuring out what options can be provided to the user and how to
structure the Gensim’s visualization module.

### July 3rd - July 14th

- Add methods to transforms the topic model distributions and related corpus data into the
data structures needed for the visualization in d3.
- Build the functional features of the visualization(widgets/plots using d3).

### July 17th - July 21th, End of Phase 2

- Add templates to embed visualization in notebook.
- Provide user options, to dynamically assess and explore the model.

### July 24th - July 28th, Begin of Phase 3

- Improve the UI/design, make it attractive, elegant and intuitive to understand.
- Test the interactivity and results of provided options comprehensively with example
dataset, handle any corner cases.

### July 31st - August 11th

- Write the notebook tutorial explaining the usage of topic model visualization.
- Blog about what data attributes can be visualized and how it can help analyze or
compare across the topic models.

### August 14th - August 18th

- Address reviews on visualization UI.
- Address review comments on TensorBoard training stats tutorial and other visualization
tutorials.

### August 21st - August 25th, **Final Week**

- Last week will involve code cleaning, adding proper docstrings, fixing possible bugs or
handling corner cases.

### August 28th - August 29th, **Submit final work**

- Have the PRs merged :)

## Future works

After the GSoC period, I plan to add the visualization for Dynamic Topic models also which
would require few different features as it takes into account the time-slices of the data as well.
I’ll also help future contributors of gensim interested in data visualization to extend the
visualization module and continue to contribute myself.

## Open Source Development Experience

Most of my experience with Open Source contribution involves contributing to gensim mainly. I
started out with some small bug fixes - [994](https://github.com/RaRe-Technologies/gensim/blob/develop/docs/notebooks/Wordrank_comparisons.ipynb), [1050](https://github.com/RaRe-Technologies/gensim/blob/develop/docs/notebooks/Wordrank_comparisons.ipynb), and then did a feature addition where I added
a python wrapper([1066](https://github.com/RaRe-Technologies/gensim/blob/develop/docs/notebooks/Wordrank_comparisons.ipynb)) around WordRank. This also involved justifying the usefulness of this
addition by comparing it to the existing word embedding models/wrappers in gensim. So, I wrote
an explanatory [tutorial](https://github.com/RaRe-Technologies/gensim/blob/develop/docs/notebooks/Wordrank_comparisons.ipynb) and [blog](https://rare-technologies.com/wordrank-embedding-crowned-is-most-similar-to-king-not-word2vecs-canute/)) for these comparisons. Few other PRs that I submitted or
reviewed in gensim are: [1125](https://github.com/RaRe-Technologies/gensim/pull/1125), [1247](https://github.com/RaRe-Technologies/gensim/pull/1247), [1256](https://github.com/RaRe-Technologies/gensim/pull/1256), [1217](https://github.com/RaRe-Technologies/gensim/pull/1217). I’ve also been active on answering some of
the queries that come up on gensim’s community channels including mailing list and gitter.
Open Source has been a great learning experience for me, and I look forward to my continued
contributions to gensim and open-source community.

## Other Experiences

I am a 3rd year Undergraduate student of Maths and IT student enrolled at CIC, University of
Delhi, India. I have undertaken relevant courses and projects in NLP and Data Sciences, my
most recent being about the comparisons of Embedding models through a variety of evaluation
metrics and test datasets. I was also a part of Innovation Project funded by DU last year, in
which I worked on analyzing genes/SNPs related to cancer. I am attaching my CV
as well, for a more detailed explanation of my previous experiences.

## Why this project?

While working on a visualization task under my previous contribution to gensim, I came across
many example visualizations in the field of NLP, which revealed some of the important
underlying structure and relations in data points. It made me realize the potential and
importance of Data visualization and how it can help us understand our data. This project is a
very good opportunity for me to work on this crucial aspect of Data science and enhance my
skills in the art of visualization.

## Resources

1. Exploiting Similarities among Languages for Machine Translation
2. https://tedunderwood.com/2012/11/11/visualizing-topic-models/
3. http://textvis.lnu.se/
4. https://research.google.com/bigpicture/
