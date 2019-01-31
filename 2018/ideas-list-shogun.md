# Shogun ML

Shogun is and open-source machine learning library that offers a wide range of efficient and unified machine learning methods.
For information regarding how to get started and submit a successful GSoC application please check our [wiki](https://github.com/shogun-toolbox/shogun/wiki/Google-Summer-of-Code-2018-Projects).

# Projects


- [Fundamental Machine Learning Algorithms III: Finding the bad guys](#fundamental-machine-learning-algorithms-iii-finding-the-bad-guys)
- [Continuous detoxification](#continuous-detoxification)
- [Distributing Shogun](#distributing-shogun)
- [Improving the user experience](#improving-the-user-experience)
- [Inside that black box](#inside-that-black-box)
- [Flexible modelselection](#flexible-modelselection)



## Fundamental Machine Learning Algorithms III: Finding the bad guys
We are continuing the highly popular project of the last years: the aim is to improve our implementations of fundamental ML algorithms.
As this year's focus is on user experiences with Shogun, we focus on ***finding the bad guys***.
Who are the bad guys?
Those are implementation of algorithms in Shogun that are embarrassingly in one of: runtime, memory efficiency, code-style, API, documentation ... we don't want to embarrass ourselves ;)

While we don't need Shogun to be the fastest/best/most pretty library in all tasks, it at least should not suck.
This project is about identifying fixing all those "bad guys".

### Mentors
 * [Heiko](Heiko%20Strathmann) (github: [karlnapf](https://github.com/karlnapf), IRC: HeikoS)
 * [Sergey](Sergey%20Lisitsyn) (github: [lisitsyn](https://github.com/lisitsyn), IRC: lisitsyn)
 * [Ryan Curtin from mlpack](https://github.com/rcurtin), IRC: rcurtin
 * [Marcus Edel from mlpack](https://github.com/zoq), IRC: zoq


### Difficulty & Requirements
**Medium to difficult**, you need to dig into existing code and you will need:

 * ML Algorithms in C++
 * Re-factoring existing code / design patterns
 * Knowledge of basic ML
 * Basic Linear Algebra, Shogun's `linalg` framework
 * Experience with other ML toolkits (preferably Python, such as [scikit-learn](http://scikit-learn.org/), or c++ such as [mlpack](http://www.mlpack.org/))
 * Desirable: Experience with the [benchmarking system](https://github.com/zoq/benchmarks)

### Details
Here are some examples of what topics should be covered.

### Runtime

Have a look at benchmark comparisons of Shogun with other libraries at [mlpack's benchmarking framework](http://www.mlpack.org/benchmark.html).
You will notices that sometimes Shogun does quite well, like for KMeans

| dataset | mlpy | scikit | shogun | weka | mlpack |
|---------|-------|-------|--------|------|--------|
| corel-histogram | 3.59s | 0.73s | 1.11s | 19.43s | 1.92s |
| mnist | 119.83s | 46.13s | 16.02s | 1558.07s | 61.35s |

On the other hand, there are situations that are less than optimal, like for linear regression, where Shogun fails.

| dataset | mlpy | scikit | shogun | weka | mlpack |
|---------|-------|-------|--------|------|--------|
| arcene | failure | 0.24s | failure | 3.16s | 0.42s |
| cosExp | 0.13s | 0.08s | failure | 17.42s | 0.13s|

Anotoher one is linear ridge regression, where Shogun is extremely slow

| dataset |  scikit | shogun |
|---------|---------|--------|
| webpage | 1.94s | >9000s |

Again, we don't want Shogun to be the fastest candidate everywhere. We only don't want it to be the slowest by far.

### Awkward API
Example: have a look at [GMM](shogun.ml/CGMM).
It has 3 train methods, awkward methods like `get_nth_mean`, multiple methods to `apply` it (`::cluster, ::get_likelihood_example`), etc.
A first step would be to rename the methods to something that looks nice, or to remove them (we have tags so no need for getters/setters anymore).
Next, GMM is nothing else but a supervised learning algorithm, so it should support that interface: `fit, predict`, and not offer its own methods.
Next, GMM is also a distribution that can be sampled from, so it should be possible to turn it into an API that supports sampling.

We actually wrote some API desiderata for the [user experience project](GSoC_2018_project_usability), which overlaps with the project in terms of API.
Think: you identify bad API, and how it should be instead, user experience project person implements basics for your changes to be possible, you change the algorithm.

### Documentation issues
Some bad examples:

* [Example of one sentence docs](http://shogun.ml/CCCSOSVM)
* [Example of no documentation at all](http://shogun.ml/CLibSVM)
* [Example of bad documentation](http://shogun.ml/CLibSVMCMulticlassLibSVM) -- no description of what happens or how expensive it is.
* [Example of bad documentation](http://shogun.ml/CRandomKitchenSinksDotFeatures) -- talks about using the features in ERM, but this class is just about feature embeddings, so it should talk about the embedding, it computational costs, and what one can do with it: pass it to linear algorithms (like a linear SVM).

You get the point...

### First steps

 * Increase coverage of Shogun in the benchmark framework. Ideally all algorithms in the framework should be populate with Shogun
 * Make a priority list of algorithms where Shogun doesn't do well: runtime & memory
 * Make a list of badly or un-documented algorithm classes (missing`@brief`, one sentence docs)
 * Make a list of algorithms with awkward API
 * Take a single instance and work on it until things are better.
 * Whenever you touch the internals, make sure to also polish: `linalg` usage, API, class design
 * Work on a one-by-one basis
 * Whenever you improve something, make sure to provide a "before-after" comparison.


### Why this is cool
This project offers the chance to learn about many fundamental ML algorithms from a practical perspective, with a focus on usability and efficiency. As we want to start with important algorithms first, it is likely that many people will use (and appreciate) code that you wrote.

### Useful resources
* [benchmark framework entrance task](https://github.com/shogun-toolbox/shogun/issues/4046)

## Continuous detoxification
This project is the third of its kind: attempting to clean the internals of Shogun and replace obsolete code and concepts with more modern counterparts.
This year, we want to focus on data representations and linear algebra API.


### Mentors
 * [Viktor](Viktor%20Gal) (github: [vigsterkr](https://github.com/vigsterkr), IRC: wiking)
 * [Michele](https://medium.com/@mic.mazzoni) (github: [micmn](https://github.com/micmn), IRC: micmn)
 * [Pan](Pan%20Deng) (github: [oxphos](https://github.com/OXPHOS), IRC: OXPHOS)

### Difficulty & Requirements
**Medium to difficult**

You need know
 * C++
 * In particular, type systems & safety (I.e. a language that is type safe, like C++ or Java)
 * Linear algebra in computers, Eigen3, Shogun's `linalg`
 * Machine learning code basics
 * Design patterns and software engineering principles


### Details
Here are some sub-projects. We are open for more:

**NOTE**: A GSoC project will address multiple (or ideally all) of those topics.

#### Re-designing `CFeatures`
We want to modernize Shogun's main data representation, [`CFeatures`](http://shogun.ml/CFeatures).

##### Immutable features
In order to make thread-safety easier, we plan to make the interface immutable. This means making all methods `const`.
Everything that changes the state or content of a features object will have to create a copy first, and return a new instance (that might share the underlying memory).
The first step is to come up with a list of all non-`const` methods in features, decide which ones can be made `const` easily.

##### Views
We want to support slicing data points (not components), i.e. generating views, behind a clean and simple API. This will replace the [existing subsets], see `void CFeatures::add_subset(SGVector<index_t> subset)`, [CSubsetStack](http://shogun.ml/CSubsetStack), etc.
We envision an API in the lines of
```
features.view(indices) # returns new instance of features that points to the same data
```

There already is a WIP pull request [here](https://github.com/shogun-toolbox/shogun/pull/3970).

##### Cross-validation
Once that immutable features and views are done, we really would like to see a multithreaded version of cross-validation implemented, using **shared memory** for the features (no cloning), see [here](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/evaluation/CrossValidation.cpp#L258).

Something in the lines of (could be more elegant, but this is to illustrate the purpose)
```
#pragma omp parallel
for (auto i in range(num_folds))
{
  inds_train, inds_test = splitting->fold(i) # generate train/test indices
  auto feats_train = features->view(inds_train) # returns training view instance (thread-safe, no data copying)
  # ... same for validation set & labels

  fold_machine = machine->clone() # clones learning machine instance (without data), cheap
  fold_machine->fit(feats_train, labels_train) # non const call, but on my own instance, so thread safe
  result[i] = evaluation(fold_machine->predict(fets_test), labels_test)
}

```

#### Constructing features, removing templates
We would like to move towards an un-templated features type, and leave it to algorithms to check types at runtime (is only done once by the algorithm).
The old classes can stay as specializations, but the public API should not expose them, or at least not all of them but a small set.

Next, features should be easily constructed from files, streams, matrices, etc.
For that, we would like to have a global factory method that can do exactly that, i.e.
```
* `Features features(Matrix)`
* `Features features(SparseMatrix)`
* `Features features(FileStream)`
* `Features features(ArrowBuffer)`
* `Features features(Strings)`
```
The resulting features would transparently load/read/stream the underlying data and all behave the same.

##### Iterators instead of direct memory access.
Features should not offer direct access to the underlying memory, i.e. the feature vector for `CDenseFeatures`.
This is since for that one needs to know the basic word-size (float32, float64) of the underlying data, which would convolute the algorithm codes (those should be independent of the word-size).
As a consequence, we would like to remove all methods that return vectors/matrices/etc.
This is a long list of changes, and we need to start by collecting all cases, and discussing which ones to change first.

Instead, we would like to perform computation over features using an API based on iterators.
We have already made a few transitions in this direction, for example see [Perceptron](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/classifier/Perceptron.cpp).
Have a look [here](https://github.com/shogun-toolbox/shogun/tree/develop/src/shogun/features/iterators) for inspiration.

##### Untemplated Vector/Matrix classes
Picking up [this](https://github.com/shogun-toolbox/shogun/pull/3854)

##### Split implementation from header files
Many parts of Shogun are split into `.cpp` and `.h`, which makes compilation/development much easier: changes in the implementation of a low level data-structure does not cause the whole project to be re-compiled.
There are many cases, however, where this is not done (especially in templated code).
This part of the project can serve as a nice initial contribution.

#### Optional
More topics that one could work on include: serialization, smart pointers, using std:: instead of our own data-structures, and more.
Let us know if something in particular is of interest for you.
We might also change things around while the project is running ;)

### First steps
For every sub-project:

* Write down a list of classes/methods/concepts that will need change (there are comments below)
* Think (and discuss) how every sub-project's problems could be solved efficiently
* Write down pseudo-code of how the API should look like
* Write down pseudo-code of how the internals would look like
* Draft minimal a prototype of how you want to implement your change
* Work on a one-by-one basis

### Why this is cool
Cleaning up the interal APIs of Shogun will lead to a huge exposure to advanced software concept, and you can be sure to learn a lot about API design, algorithms, and good practices in software development.
The project will make it much easier to develop clean code within Shogun, and as such make the project more attractive for scientists to implement their work in.

### Useful resources
* [Untemplated matrix PE](https://github.com/shogun-toolbox/shogun/pull/3854)
* [Feature views PR](https://github.com/shogun-toolbox/shogun/pull/3970)

## Distributing Shogun
Installing Shogun is not the easiest task from source. Hence, we have started to add support for binary distribution of Shogun. Currently there are two ways to get on hold of libshogun, the C++ API and the Python interface:
 * [Shogun PPA](https://launchpad.net/~shogun-toolbox)
 * [Conda](https://anaconda.org/conda-forge/shogun)

This is not even covering half of the distributions and interfaces that Shogun supports. The aim of the project would be to extend the supported packaging frameworks and integrate with them.

### Mentors
 * [Viktor](Viktor%20Gal) (github: [vigsterkr](https://github.com/vigsterkr), IRC: wiking)

### Difficulty & Requirements
**Medium.**

You need know
 * various packaging methods (deb, npm, jar, pypi etc.)
 * basic DevOps skills
 * basic buildbot knowledge

### Description
As mentioned we currently only support binary packages for the C++ and Python API, that barely covers the most use-cases.

We would like to be able to integrate with all the language specific packing systems, that would allow various users of Shogun to quickly install the library and use it out of box.

Namely, we would like to integrate with the following packaging systems---the list is in priority order:
 * Maven and JigSaw for Java/JVM
 * PyPi for Python
 * CRAN for R
 * WebAssembly for JavaScript
 * NuGet for MS Windows and C#
 * RubyGems
 * NPM for JavaScript

Ideally by the end of the GSoC project there would be integration with all of these packaging framework as well as integrating it into our release pipeline (done in buildbot) so the new releases would automatically roll out everywhere.

### WebAssembly
[WebAssembly](http://webassembly.org/) is a great initiative that is a new portable, size- and load-time-efficient format suitable for compilation to the web. Using [emscripten](https://github.com/kripken/emscripten) one can take a C/C++ code and compile it into a JavaScript code that uses `asm.js`. Using this the idea is to create wasm of shogun and run it in the user's browser (or at least part of it).

There has been [experiments done](https://hacks.mozilla.org/2017/09/bootcamps-webassembly-and-computer-vision/) on OpenCV and WebAssembly, which has a similarly complex and big C/C++ codebase as Shogun. This could be a good starting point for this task.

### Useful resources
 * https://developer.mozilla.org/en-US/docs/WebAssembly/C_to_wasm

## Improving the user experience
Let's make it easier for people to **use** and **develop** Shogun.
For users, we would like to cover: user API & pipelining, parameters defaults and descriptions, exception handling, documentation & examples.
In a second step, for scientists/developers, we would like to cover: plugin architecture, internal API (e.g. linear algebra), simplification.

### Mentors
 * [Sergey](Sergey%20Lisitsyn) (github: [lisitsyn](https://github.com/lisitsyn), IRC: lisitsyn)
 * [Heiko](Heiko%20Strathmann) (github: [karlnapf](https://github.com/karlnapf), IRC: HeikoS)

### Difficulty & Requirements
**Medium.** The biggest challenge of this project is the vast scope -- you will touch a lot of Shogun's internals, both framework and ML code. Good planning required!

You need know
 * C++, Python
 * How Shogun's interfaces work, aka SWIG.
 * Shogun's new parameter framework, aka [tags](https://github.com/shogun-toolbox/shogun/blob/develop/tests/unit/base/SGObject_unittest.cc#L365)
 * Exception handling
 * Machine Learning basics
 * Other ML libraries (with good APIs)

### Details
Here are some sub-projects. We are open for more:

**NOTE**: A GSoC project will address multiple (or ideally all) of those topics.

#### Base API design
We would like to put effort into cleaning and re-designing the current **user** API.
That is, the API that is accessed through SWIG and that is exposed via our [examples](http://shogun.ml/examples). That is not the internal C++ API.
As a motivation, have a look at e.g. our [very basic learner class: `CMachine`](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/machine/Machine.h).
Observe how many methods there are, and how confusing this must seem to new users.
Your task is to simplify this. This will include: renaming existing classes and methods, adding new methods, re-factoring existing classes, maybe even adding new classes.
Most important, we want to make the new API very minimal.

Steps:
 * Have a look at these [notes](Hackathon-2017-base-api) for some initial ideas and examples.
 * Write a few complete user stories (API usage example, see the notes for some examples) for common ML cases. This requires some research: what are fundamental ML tasks that should definitely be covered, how do other libraries do it?
 * Turn your insights on how the API should look like into a summary: a class diagram for example.
 * Come up with a set of API changes in Shogun required to serve the user stories. This will include adding/renaming/removing.
 * Work incrementally, one "use-case" at a time.

Topics to cover:
 * Clean up our learner base class `CMachine`, and make it follow the de-facto standard of `fit/predict`
 * Remove all "casting" methods from Shogun, they are not needed anymore since we have tags. I.e. remove all `::obtain_from_generic` calls, remove `apply_regression, apply_binary` etc.
 * Merge `preprocessor` and `converter` as they do the same thing. Clean up their API.
 * Implement some of the operator chaining ideas from the [notes](Hackathon-2017-base-api). This is a bigger topic and will require some code logic that implements the ideas. Could take a few weeks but results in a really cool improvement.
 * Remove all `copy` methods (that create a copy of an instance), but rather implement copy constructors and rely on `clone` for deep copies.
 * Put in methods to change the interface for algorithms that support multiple APIs (see below)
 * There is way more here to do, but you get the idea :)

An example for a clean GMM API using `as_*`
```
gmm = sg.GMM()
gmm.algorithm = "split_and_merge_em"
gmm.algorithm = "em"
gmm.fit(features)
gmm.predict(features_test) # returns discrete labels, classification
gmm.as_classifier().predict(features_test) # same as above
gmm.as_distribution().predict(features_test) # returns the log-probability for each component for each data
gmm.as_distribution().as_mixture().get_component(idx) # returns a Gaussian component
gmm.as_distribution().sample(100) # returns 100 samples from the mixture
```

### First steps
Write user stories!
Those are pseudo-code examples of how the user interacts with the library, and how that should look like.
Your application needs to contain a couple of those (and ideas how to make them happen inside Shogun).
Cover all the topics you want to address (see below), and try to be as precise as possible.
See also below for more details.

### Exception handling
Currently, Shogun's exception handling is not ideal. It is just the same `ShogunException` that is thrown and in some languages it causes the program to exit. Our error messages are sometimes good (if the developer was motivated), and sometimes quite bad -- they don't tell the user what she did wrong.
This part of the project is to introduce a small set of exceptions and populate Shogun with them (e.g. `NotConverged` or `InvalidState`) so in the code the following would be possible:
```(python)
try:
   svm.train()
except shogun.NotConverged:
   ...
except shogun.InvalidState:
   ...
```
The next step is to connect them to the SWIG interfaces.
Some planing/prototyping is needed here.

### API example coverage
We would like to see **all** of Shogun's API covered in the meta examples (which also makes them be integration tested). We currently do lack examples (and cookbooks) for

 * StringFeatures
 * Fast SVMs in Shogun
 * Dimensionality reduction
 * many more ...

This project will involve writing at least 2-3 cookbooks per week (other projects need 2 examples without a cookbook), to increase coverage.


### Parameters, defaults, and documentation
Machine Learning algorithms crucially depend on well-chosen parameters.
While you can tune them automatically with Shogun (takes long though), a user sometimes simply wants to run an algorithm out of the box.
Therefore, Shogun's default parameters should be sensibly chosen by the people who know what they are doing: the developers.
Furthermore, users might be interested in what the parameters do, so we need to make it easy to read their descriptions without opening a web-browser.

In this part of the project, you will

 * Make sure (i.e. test with real-world examples) that the default parameters of Shogun are well-chosen. Compare the choices to other libraries. One that does a particularly good job in sklearn.
 * Implement a nice way to expose parameter documentation (currently done via doxygen, see the [API](http://www.shogun-toolbox.org/api)) **at runtime**. This is likely to be done via the tags framework.  We could for example see a Python script that reads parameter documents in tags and then makes sure they appear in the doxygen API. Example: `help(svm)` (we have that already, but it needs polish), `help(svm.C)`. This should also work for all target inferfaces.
 * Update `@brief` descriptions of Shogun's algorithms (some are good, some are completely missing)


### Optional
* We would like to integrate / merge all existing sources of documentation into a single one: cookbooks, API, parameter docs should all use the same content in order to improve maintainability (TODO: explain this better)

Anything else that sucks about using Shogun?
Put it in here :)

### Is this project for you?
You like thinking about API design?
You like things to be neat?
You enjoy exploring existing code-bases?
You like to have an impact on Shogun?

### Why this is cool
This project will massively improve Shogun's usability, and therefore has a potentially significant impact on the project's user-base.
You will get exposed to a lot of Shogun's internals and have a say in design decisions that will impact face of Shogun.

### Useful resources
 * Shogun [tags](https://github.com/shogun-toolbox/shogun/blob/develop/tests/unit/base/SGObject_unittest.cc#L365) and the [README](https://github.com/shogun-toolbox/shogun/wiki/README_tags)
 * [Our most basic learner class: `CMachine`](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/machine/Machine.h)
 * [Base API notes](Hackathon-2017-base-api)
 * [How to write new examples](https://github.com/shogun-toolbox/docs/blob/master/EXAMPLES.md)

## Inside that black box
This project picks up Giovanni's great initiative to ["open the black box"](https://www.youtube.com/watch?v=yNn-zwfZtHU) of Shogun implementations.
The idea is simple and appealing: when calling an expensive C++ algorithm from e.g. a Python interface, it blocks until the call finishes which can take a long time to complete. The user has no control over the program execution and therefore he has no clue on how long it will take and, more importantly, how well the model is doing.

We would like to have Shogun's iterative algorithms to be:
* emitting their progress (how much was done? how much is left?);
* emitting the internals (residuals, partial results, convergence diagnostics);
* being stoppable (resulting in partially trained models that can be deployed!);

The foundations of this have been laid last year. This year is about polishing and extending these ideas to the whole of Shogun and illustrate some cool use-cases. This project is also slightly related to the others (like the [Detox](https://github.com/shogun-toolbox/shogun/wiki/GSoC_2018_project_detox) or [Usability](https://github.com/shogun-toolbox/shogun/wiki/GSoC_2018_project_usability) projects) since the internal API for premature stopping and parameter's observers may require a bit of refactoring, to reflect the future changes of Shogun's class and interfaces.

### Mentors
 * [Heiko](Heiko%20Strathmann) (github: [karlnapf](https://github.com/karlnapf), IRC: HeikoS)
 * Giovanni (github: [geektoni](https://github.com/geektoni), IRC: geektoni)

### Difficulty & Requirements
**Easy to medium**

You need know:
 * C++;
 * Shogun (how it's internal works);
 * [RxCpp](https://github.com/Reactive-Extensions/RxCpp) (just a basic level it's enough);
 * Machine learning code basics (what are iterative algorithms);
 * [openmp](http://www.openmp.org/) (optional);

### First steps

The first thing that has to be done is create a list of algorithms that have an iterative nature. This can be done via grepping for our progress bar, the stopping mechanics (`CSignal`), algorithms that use RxCpp, and generally reading the code base (there are lots of algorithms that are iterative but don't contain any of the mentioned mechanics. Example: [`ICA`](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/converter/ica/FastICA.cpp#L133)
Secondly, for each of them we will have to do the following:
* Propose/Discuss which are the suitable actions that can be performed when pausing/stopping the method;
* For each of them implements the corresponding methods (on_pause(), on_complete(), on_next());
* Add a progress bar were possible;
* Add a meta example which shows these new features such that a prospective user can see how premature stopping works;
* Write tests where you make sure that one can use partially trained models, i.e. can call `apply/predict` after having stopped training, the instance is in a "good" state, it is serializable, etc.;

If you don't know where to start, do not worry ;) see the entrance task testing [iterative algorithms](https://github.com/shogun-toolbox/shogun/issues/3889) and the one regarding [premature stopping](https://github.com/shogun-toolbox/shogun/issues/4106).

#### Optional
* For each of the algorithms, make them emits value such that to be able to record some information and visualize them with Tensorboard;

Refine the TensorBoard integration even further. Tensorboard permits to visualize also different types of data, more specifically: images, audios and texts. Currently, Shogun supports only scalars and histograms visualization. We would like to know if we could extend the observable/observer code to output also such data from Shogun's algorithms, in a way to being able to show them through Tensorboard.

### Why this is cool
It makes Shogun so much more applicable to large problems and it improves the workflow of using Shogun. It will enhance the codebase and it will modernize our library by giving users the possibility to actually see what is going on inside their models.
You will touch a lot of ML code and therefore you will learn how commonly used algorithms work and how they are implemented in Shogun. You will also come in touch with many new technologies which build the skeleton of the project's framework (SWIG, RxCPP, Tensorflow, Tensorboard etc.) which will make your CV even cooler!

### Useful resources
* Read some Shogun code of stoppable algorithms, e.g. [`CPerceptron`](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/classifier/Perceptron.cpp), [`CCrossValidation`](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/evaluation/CrossValidation.cpp#L239), or [`CMachineEvaluation`](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/evaluation/MachineEvaluation.cpp#L143)
* The progress bar [code](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/base/progress.h)
* An [example](https://github.com/shogun-toolbox/shogun/blob/develop/src/shogun/base/SGObject.cpp#L756) on how RxCpp is employed in Shogun;
* [The developer's guide on premature stopping](premature-stopping)
* [The developer's guide on the progress bar](progress-bar)
* [The developer's guide on parameters' observers](parameter-observers)

## Flexible modelselection
Following up on one of our [very first GSoC (2011) projects](https://opensource.googleblog.com/2011/09/shogun-aims-high-with-google-summer-of.html), this project intends to clean up, unify, extend, and scale-up Shogun's modelselection and hyper-parameter tuning framework.
It is a cool mixture of modernizing existing code, using multi-threaded (and potentially distributed) concepts, and playing with black-box optimization frameworks.

### Mentors
 * [Heiko](Heiko%20Strathmann) (github: [karlnapf](https://github.com/karlnapf), IRC: HeikoS)
 * [Sergey](Sergey%20Lisitsyn) (github: [lisitsyn](https://github.com/lisitsyn), IRC: lisitsyn)

### Difficulty & Requirements
Medium to advanced. Depends on ambitions, but we are flexible on student's abilities.

You need to know about
 * Modelselection basics (x-validation, search-algorithms, implementation)
 * Shogun's modelselection framework
 * Shogun's parameter framework
 * C++
 * Optimisation frameworks like [MOE](https://github.com/Yelp/MOE) or [cma-es](http://en.wikipedia.org/wiki/CMA-ES)
 * Knowledge of other libraries' approaches ([sklearn](http://scikit-learn.org/stable/model_selection.html), [MLPack](http://www.mlpack.org/gsocblog/cross-validation-and-hyper-parameter-tuning-summary.html))

### Details

#### X-validation v2
Every learning algorithm (`CMachine` subclass) should work with x-validation ... fast!
This is completely independent of any hyper-parameter tuning.

* All model classes should be systematically tested with x-validation, see [issue](https://github.com/shogun-toolbox/shogun/issues/3732). This is similar to the [trained model](https://github.com/shogun-toolbox/shogun/blob/develop/tests/unit/base/trained_model_serialization_unittest.cc.jinja2) tests.
* Identify models that do only perform **read-only** operations on the features (this will be all models later, depending on the progress of[features-detox project](GSoC_2018_project_detox)).
* Enable multi-core x-validation using openmp, via cloning of the underlying learning machine, but with shared features (memory efficiency!).
* Carefully test the chosen models for race-conditions, memory errors, etc.
* Add algorithms on a one-by-one basis.
* Generalise code of the "trained model serialization" tests to a "trained model" tests, where multiple things can be checked for the trained models (serialization, x-validation for now).
* Make sure model-selection has a progress bar, is stoppable, continue-able, etc. See also the [black-box project](GSoC_2018_project_inside_blackbox)

#### Cleaning up after the old parameter framework
We recently changed our internal parameter framework ... or more: we are in the progress of changing it.
The new framework is cleaner, neater, and as such easier to handle.

Before we start tuning parameters in an automatic way, we need to remove the traces of the old framework.
This is a messy task that requires diving deeply into the Shogun core, but don't worry -- we will help you :)

* Remove the `m_modelselection_parameters` field from `CSGObject`
* This will break many things (most of all the current model-selection framework). Fix the problems (good initial task)
* Remove the `TParameter` construct and eventually the class `Parameter`

#### A clean API
We want to build  a better way to specify free parameters to learn, which overlaps with the [user experience project](GSoC_2018_project_usability).
The current way is to build parameter trees whose structure matches the learning machine, see e.g. [here](http://www.shogun-toolbox.org/notebook/latest/xval_modelselection.html#Model-selection-using-Grid-Search)
We would like to shop around other libraries for ideas on specifying this.

Sergey, could you put some API ideas here?

Some steps:

* Review and compare other libraries ' approaches
* Collect the most common use cases (random search, grid-search, gradient search (e.g. in our Gaussian Process framework))
* Come up with a set of clean API examples / user stories for those cases
* Draft code how to implement this API. This will include ways to annotate the spaces that parameters live in, as well as whether gradients are available.
* Implement and test systematically
* Make sure it works nicely in all target languages.

#### Black box optimisation

Bayesian optimisation and stochastic optimisation are powerful frameworks for blackbox optimisation. We aim to integrate bindings for both during the project. There is plenty of external libraries that do the algorithms for us, so this task is mostly about designing interfaces that tell Shogun to cross-validate the algorithm on the next set of parameters and reporting its performance. We aim for both [MOE](https://github.com/Yelp/MOE) and [CMA-ES](http://en.wikipedia.org/wiki/CMA-ES).


### Why this is cool
There is hardly any algorithm without free parameters.
Currently Shogun only has brute force search to tune them automatically.
While this works for SVMs, it it hopeless for anything more than 2 parameters.
Certainly, a clean and easy way to quickly tune parameters would massively boost Shogun's usability.
The project spans a huge range on topics within and outside of Shogun, including framework internals as well as cutting edge algorithms for optimisation. Super interesting even for ourselves.
Be ready to learn a lot.

### Useful resources
 * Shogun's modelselection [classes](http://shogun.ml/CModelSelection)
 * [Parameter trees](http://shogun.ml/CModelSelectionParameters)
 * [MOE](https://github.com/Yelp/MOE) are a plus
 * [CMA-ES](http://en.wikipedia.org/wiki/CMA-ES)
 * [entrance task on testing xvalidation](https://github.com/shogun-toolbox/shogun/issues/3732)

 ## Arrow Buffer as CFeatures memory backend
 Now that more and more data science project starts to use [Apache Arrow](https://arrow.apache.org/) as a memory backend
or at least has the support to export the data into an Arrow Buffer (see for example [SPARK-13534](https://issues.apache.org/jira/browse/SPARK-13534)) it would be great that some of the Shogun's `CFeatures` classes could use Arrow Buffer as a memory backend.

### Difficulty & Requirements
**Medium.**

You need know
 * C++
 * basic software engineering

### Description
Apache Arrow is a cross-language development platform for in-memory data. It specifies a standardized
language-independent columnar memory format for flat and hierarchical dataIt provides computational libraries and
zero-copy streaming messaging and interprocess communication. Languages currently supported include C, C++, Java,
JavaScript, Python, and Ruby.

Using Arrow as `CFeatures` would not only allow us for example to directly work over pandas DataFrame via pyarrow,
but in the long run, as the number of supported languages of Arrow is getting more and more, slowly and gradually we could
get rid of some of the SWIG based typemaps, which would result in a significant memory footprint reduction as well as
performance.

### Useful resources
Start with checking out the prototype in the `feature/arrow` branch.


