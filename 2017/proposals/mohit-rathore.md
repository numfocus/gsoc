# Port tests to Pytest [#884](https://github.com/MDAnalysis/mdanalysis/issues/884)

## Abstract

| **Difficulty** | **Involves**  | **Potential Mentors** |
| ------------- | --------------|------------ |
| Moderate | Python, nose, pytest, coverage, travis | [@mnmelo](http://www.github.com/mnmelo) [@richardjgowers](http://www.github.com/richardjgowers) [@jbarnoud](http://www.github.com/jbarnoud) [@kain88-de](http://www.github.com/kain88-de) |

TDD or test driven development is the backbone of any modelling library.  Recently nose has ceased its maintenance and suggests porting to nose2 or pytest [[1]](http://nose.readthedocs.io/en/latest/#note-to-users). Along with mdanalysis some libraries like ckan [[2]](https://github.com/ckan/ckan/issues/3310), azurectl [[3]](https://github.com/SUSE/azurectl/issues/102) have already realized that this porting is necessary. While some very reputed libraries like [numpy](https://github.com/numpy/numpy) have been using nosetest for all of their tests, it is really hard for them to migrate to pytest. Unlike numpy, mdanalysis has less number of tests so porting is comparitively easy. This project should be considered top priority because mdanalysis aims to increase its code coverage to over 90%. Porting to pytest before increasing the coverage will prevent devs from investing redundant effort in writing tests in compliance to nosetest.


## Technical Details

* Testing : [pytest](http://doc.pytest.org/en/latest/) is a mature full-featured Python testing tool that helps you write better programs. There is a good comparision between different testing frameworks here [[4]](https://koodaamo.wordpress.com/2013/11/29/comparison-of-py-test-and-nose-for-python-testing/ ) [[5]](https://www.slant.co/topics/2621/versus/~pytest_vs_nose_vs_unittest).
* Coding  : use travis-ci automated build system, follow PEP8  
* Testing CI : use travis-ci, coveralls for coverage  
* Stream Editor : preferably [sed](https://www.gnu.org/software/sed/manual/sed.html#Overview) or [replace](http://hpux.connect.org.uk/hppd/hpux/Users/replace-2.24/) to perform basic text transformation in the initial phase of development.
* Time benchmarking tool - [pytest-benchmark]([pytest-benchmark](https://pytest-benchmark.readthedocs.io/en/latest/)  is a plugin for pytest to finely benchmark the time taken by each test.
*
* Devs at [azurectl](https://github.com/SUSE/azurectl) have already  [ported to pytest](https://github.com/SUSE/azurectl/commit/b071a582bf59d227f87b7148af698174cba31730)


The prime problems I faced were:
* With the `setUp` and `tearDown` functions. If the class did not inherit `TestCase` these functions were not executed so we have two options:  
1 - to either
use `setup_method` and `teardown_method` instead  
2 - inherit `TestCase` from `numpy.testing` module.

* As pointed out in [#884](https://github.com/MDAnalysis/mdanalysis/issues/884) `yield` is deprecated will be deprecated in pytest 4.0. The workaround (also suggested in [#884](https://github.com/MDAnalysis/mdanalysis/issues/884)) is to use `@pytest.mark.parametrize`

* The pytest skips tests with `__init__` method, the suggested workaround is to change the [standard test discovery](http://docs.pytest.org/en/latest/example/pythoncollection.html).
* Also other test discovery options were not compatible with `nosetests`. Hence changing [standard test discovery](http://docs.pytest.org/en/latest/example/pythoncollection.html) is mandatory.

## Schedule of Deliverables
### May 1st - May 28th, **Community Bonding Period**

* Before diving into the coding part I will analyze (or resolve) some issues beforehand - primarily [#597](https://github.com/MDAnalysis/mdanalysis/issues/597), [#1191](https://github.com/MDAnalysis/mdanalysis/issues/1191) and the subordinating issues to get a better grip on the subject. Also I will follow the course suggested in [#884](https://github.com/MDAnalysis/mdanalysis/issues/884).
* Discuss (and figure out) a list of nosetests features which are incompatible with pytest.
* As suggested by [@kain88-de](http://www.github.com/kain88-de) in this phase I will modify the existing [wiki-page](https://github.com/MDAnalysis/mdanalysis/wiki/Porting-tests-to-Pytest) by gathering the following information:
> * the plugins we have and how they could be ported to py.test?  
> * how many tests pass now with pytests?  
> * what fails?  
> * what idoms with examples in our testsuite fail?  
> * where can we replace nose with pytest idoms?

* To keep a track of my work I will be posting a weekly blog on my blogging website
https://markroxor.wordpress.com/ or my jekyll based blog at http://markroxor.github.io/blog/ . Here I will highlight all the bugs/issues I will be facing during GSoC.

In the coming phase we will thoroughly collect the relevant information required to proceed, along with minor code-refactoring.

### May 29th - June 3rd
* Make a list of tests that won't run under pytest.

### June 5th - June 16th
* Refactor the code to make current tests compatible with pytest.
* Configure travis-ci and other test execution scripts.
* Submit a PR.

### June 19th - June 23th **End of Phase 1**
* Port the currently used plugins.
* If any extra information is available, update the [wiki-page](https://github.com/MDAnalysis/mdanalysis/wiki/Porting-tests-to-Pytest).
* Submit a PR.

### June 26 - June 30th, **Begin of Phase 2**
In this phase we'll try to replace nose with pytest's idioms.
* Replace nose with pytest's idioms for [`coordinates`](https://github.com/MDAnalysis/mdanalysis/tree/develop/testsuite/MDAnalysisTests/coordinates)
* Submit a PR.

### July 3rd - July 7th
* Replace nose with pytest's idioms for [`analysis`](https://github.com/MDAnalysis/mdanalysis/tree/develop/testsuite/MDAnalysisTests/analysis)
* Submit a PR.

### July 10th - July 14th
* Replace nose with pytest's idioms for [`topology`](https://github.com/MDAnalysis/mdanalysis/tree/develop/testsuite/MDAnalysisTests/topology)
* Replace nose with pytest's idioms for the root [`MDAnalysisTests`](https://github.com/MDAnalysis/mdanalysis/tree/develop/testsuite/MDAnalysisTests/) folder
* Submit a PR.

### July 17th - July 21th, **End of Phase 2**
* Replace nose with pytest's idioms for [`core`](https://github.com/MDAnalysis/mdanalysis/tree/develop/testsuite/MDAnalysisTests/core)

* Replace nose with pytest's idioms for [`auxilary`](https://github.com/MDAnalysis/mdanalysis/tree/develop/testsuite/MDAnalysisTests/auxiliary)

* Replace nose with pytest's idioms for [`formats`](https://github.com/MDAnalysis/mdanalysis/tree/develop/testsuite/MDAnalysisTests/formats)

* Replace nose with pytest's idioms for  [`lib`](https://github.com/MDAnalysis/mdanalysis/tree/develop/testsuite/MDAnalysisTests/lib)

* Submit a PR.

### July 24th - July 28th, **Begin of Phase 3**
* Write tests for all the code which was not already covered by nosetest.

### July 31st - August 11th
* Try to reduce the time taken by tests by finding slow tests using [pytest-benchmark](https://pytest-benchmark.readthedocs.io/en/latest/) and optimize them.
* Submit a PR.

### August 14th - August 18th
* Complete any overdue tasks left.
* __Secondary project__ - If time permits, increase code coverage by completing [project 2](https://github.com/MDAnalysis/mdanalysis/projects/2).
* Submit a PR.

### August 21st - August 25th, **Final Week**
* Go through the code all over again to remove any redundancies. Clean the code.  
* Manage docstrings.

### August 28th - August 29th, **Submit final work**
* Get the final pull request merged.

## Future works
After GSoC ends I am willing to continue my work on the mdanalysis's [github projects](https://github.com/MDAnalysis/mdanalysis/projects) and on the gsoc project [Add new MD-Formats](https://github.com/MDAnalysis/mdanalysis/wiki/GSoC-2017-Project-Ideas#add-new-md-formats), once I get more acquainted with the library during gsoc.  

# Q&A
> Why are you interested in working with us?

I want to be in constant touch with the open-source community. Porting tests for _mdanalysis_ is the easiest way for me to get acquainted with the library code and hence I will be able to contribute further.

> Have you used MDAnalysis for your research already?

No, I have not used MDAnalysis for research but I believe that this is irrelevent since the project I have chosen can be implemented regardless.

> Do you have any exams during GSoC or plan a vacation during the summer?

No, my exams wrap up in the month of April. I'll be available for the entire summer.


> Do you have any experience programming?

I have a strong development profile. Few relevant projects I am involved in are -
* I am owner of a reputed organisation [WAlnut](https://github.com/WalnutiQ/wAlnut) working towards artificial general intelligence.
* member of the incubator program hosted by [Gensim](https://github.com/RaRe-Technologies/gensim/) - a topic modelling library.
* member of the organisation [Numenta](https://github.com/numenta/) - Numenta Platform for Intelligent Computing is an implementation of Hierarchical Temporal Memory (HTM)
* owner of [Roboism](https://github.com/roboism), a source code junction for the projects used by [Roboism](roboism.pythonanywhere.com), the robotics club of IIT(ISM) Dhanbad.

### Contribution to Open-Source Projects
I have many open-source contributions. A few of which are:

[gensim](https://github.com/RaRe-Technologies/gensim) -
I am among the top 15 contributors of the repository.

Major/minor PRs
[MRG]

https://github.com/RaRe-Technologies/gensim/pull/1036

https://github.com/RaRe-Technologies/gensim/pull/990

https://github.com/RaRe-Technologies/gensim/pull/989

https://github.com/RaRe-Technologies/gensim/pull/968

https://github.com/RaRe-Technologies/gensim/pull/964

https://github.com/RaRe-Technologies/gensim/pull/957

https://github.com/RaRe-Technologies/gensim/pull/949

https://github.com/RaRe-Technologies/gensim/pull/946

https://github.com/RaRe-Technologies/gensim/pull/925

https://github.com/RaRe-Technologies/gensim/pull/924

https://github.com/RaRe-Technologies/gensim/pull/895


[WIP]

https://github.com/RaRe-Technologies/gensim/pull/1091

https://github.com/RaRe-Technologies/gensim/pull/1033

https://github.com/RaRe-Technologies/gensim/pull/1086


[nupic](https://github.com/numenta/nupic)

minor PRs

[MRG]

https://github.com/numenta/nupic/pull/3426

https://github.com/numenta/nupic/pull/3421

{{ Development Experience }}

## Other Experiences
In my sophomore year I have worked as a junior research assistant with Dr. Deivalakshmi (NIT Trichy) in the field of image enhancement using Dynamic Stochaistic Resonance and currently I am working Dr. Dinbhandhu Bhandari (HIT Kolkata) in the field of natural language processing using word2vec.

## Why this project?
Ever since I first started open-sourcing I have been writing and running tests
for my code parallely with the modelling part. I have been using nose tests for
[gensim](https://github.com/RaRe-Technologies/gensim) and [Walnut](https://github.com/WalnutiQ/wAlnut). With `nosetests` being obsolete I will gradually supercede it by nosetest 2 or pytests. This project will indirectly
help me with the conversion.  


## Appendix

# About Me

## Username and Contact Information

**Name**            :   Mohit Rathore

**University**      :   [Indian Institute of Technology (ISM), Dhanbad](http://iitismdhanbad.ac.in)

**Email**           :   mohit@ece.ism.ac.in

**Github Username** :   [markroxor](https://github.com/markroxor)

**Web Presence** : Google "_markroxor_"

## Relevant links
1 - http://nose.readthedocs.io/en/latest/#note-to-users  
2 - https://github.com/ckan/ckan/issues/3310  
3 - https://github.com/SUSE/azurectl/issues/102   
4 - https://koodaamo.wordpress.com/2013/11/29/comparison-of-py-test-and-nose-for-python-testing/  
5 - https://www.slant.co/topics/2621/versus/~pytest_vs_nose_vs_unittest
