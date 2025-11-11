# Enhancing conda-smithy
Development of a conda-skeleton for recipe generation and automatic update of recipes from PyPI and CRAN


## Abstract

The conda package manager provides a tool, `conda-skeleton`, that makes the creation of conda recipes based on PyPI and CRAN easy. The recipe is a directory holding metadata and the scripts needed to build the package. The conda package is then built using the conda recipe. However, the standard recipe output from conda-skeleton requires some heavy editing to conform with conda-forge standards. This project entails the creation of a conda-smithy skeleton for recipe generation. This can be developed further for automating recipe generation from PyPI and CRAN with minimal user input.

## Technical Details

### Objectives
- Develop `conda-smithy skeleton` for simplifying the build process for conda-forge packages.
- Developing tools for conda-forge recipe generation for packages similar to `conda skeleton`. 
- Simplifying and automating the package updating process by developing tools to regenerate recipes based on the latest package from various sources - PyPI, CRAN, Github.

### Background

#### conda : 
An open source cross-platform package and environment manager for installing and distributing software across multiple platforms. Conda works on Linux, OS X and Windows and aims to make switching across platforms easy. Once a binary package is built, the distribution is uploaded to a Continuum repository so that users can easily install them in any platform. Conda is designed to manage packages and dependencies for software written in any language but originating from the PyData community it works well with Python packages. 

#### conda-forge:
The official Continum package cannot scale to the large user base of open source libraries which led to the formation of `conda-forge` in 2016. It contains tools for creation of community-driven builds for any package. Packages are maintained in the open via Github, unlike conda, with binaries automatically built using various CI tools. Conda-forge has the tool `conda-smithy` for building packages and it is a work in progress. This GSoC project will deal with enhancing `conda-smithy` for automating recipe generation and better integration with various repositories - PyPI, CRAN, Github.

#### The problem:
`conda` and `conda-forge` build packages by using a `recipe` which contains files with information for the build process. A recipe contains build scripts, files specifying metadata and other resources such as README, icon files, or build notes. The template of a recipe is slightly different in conda-forge than that of a conda recipe. Due to this, a heavy editing of recipes is required from the users side. This project will aim at minimising that and creating tools for recipe generation and update.

### Plan of work
A [PR](https://github.com/conda-forge/conda-smithy/pull/480) has been opened for discussion regarding a new `conda-smithy skeleton command`. We will be closely following the `conda skeleton` tool for better integration with conda. The [skeletonize](https://github.com/conda/conda-build/blob/master/conda_build/skeletons/pypi.py#L302) functions are recipe generators for PyPI and CRAN packages which can be found in the [skeleton folder](https://github.com/conda/conda-build/tree/master/conda_build/skeletons) in conda-build. A similar command for `conda-smithy` will be developed to regenerate a conda-smithy recipe based on the latest package version. Conda-skeleton will also contain tools for automatically generating recipes from PyPI, CRAN and Github.


## Schedule of Deliverables

### May 1st - May 28th, **Community Bonding Period**

Community bonding and getting to know other developers, recommended practices and usage of conda-forge. Reading documentation and an attempt to build a new Python package in conda-forge from scratch.

### May 29th - June 3rd

Addition of a skeleton argument to `conda-smithy` for generating basic recipe files. This will follow the code in conda's `skeletonize` functions. The difference would be in the recipe templates for conda-forge and conda. 

### June 5th - June 9th

Developing the conda skeleton command for actual recipe generation from a locally stored package enforcing the following:

- Removal of the boilerplate comments
- Use of a jinja variable for the package version
- Enforce the extra metadata like the maintainers list
- Enforce the new pypi.io URL

### June 12th - June 16th

Enforce sha256 instead of md5 in the recipe and automate the process of generating the hash from online sources. 

### June 19th - June 23rd, **End of Phase 1**

Automate the process of collecting other information from PyPI (requirements) and updating the recipe file. Testing the new functionality for the skeleton command for PyPI packages.

### June 26th - June 30th, **Begin of Phase 2**

Removal of the build scripts in lieu of a script line for simple Python. Testing of the script by building Python packages locally as well as from PyPI

### July 3rd - July 7th

Once the skeleton is tested for Python packages, the next part is to do the same for R packages from CRAN. At first, look at the existing tools and methods for recipe generation for R packages and try to build a simple R package in conda-forge.


### July 10th - July 14th

Setup a simple recipe generation function for CRAN packages locally and check CRAN apis for retriving information from CRAN

### July 17th - July 21st, **End of Phase 2**

Develop the CRAN recipe generation function to get package information from CRAN.

### July 24th - July 28th, **Begin of Phase 3**

Testing of the CRAN recipe generation function. Build R packages locally and from CRAN.

### July 31st - August 4th

Testing and resolving issues with the skeleton command for PyPI, CRAN and local packages. 

### August 7th - August 11th

This part should mainly be about testing the complete skeleton tool and making it merge ready. As such, documentation, testing and code review will happen at this stage.

### August 14th - August 18th

Building packages should be easy with the new conda skeleton command now for Python and R packages. Complete all testing from online sources - PyPI, CRAN. Complete final documentation and blog for the project.

### August 21st - August 25th, **Final Week**

Resolve any issues with the tool along with the start of an implementation for Github integration. This can be carrier forward as a future goal.

### August 28th - August 29th, **Submit final work**

## Future works

#### Github integration and automatic recipe generation

Once the implementation for a `conda-smithy` recipe generation method from PyPI and CRAN is in place, it can be extended for recipe generation from Github repositories. In principle, anybody can use the skeleton to build their packages and host them with a command `conda-smithy github "www.github.com/user/repo.git"`

#### Automatically issue PRs to the feedstocks with package version updates from PyPI.

Another extension could be adding a feature to issue PRs whenever a package is updated in PyPI so that a new recipe and build process is started in the feed-stock repository. This will simplify the process of updating conda-forge recipes. It will require reading the package current version from the feedstock, checking if a new version is available and issuing a PR against the feedstock repository. It should follow the MNT PRs model used to re-render feedstocks.


## Development Experience

- Developed a python module for Magnetic Resonance Image (MRI) reconstruction based on the Intra-voxel Incoherent Motion model (Le Bihan, 84) as part of [Google Summer of Code 2016 (blog)](http://sahmed95.blogspot.in) with [Dipy](http://nipy.org/dipy/). The module is currently merged as part of Dipy and the development can be tracked in this [PR](https://github.com/nipy/dipy/pull/1058).

- Worked on the development of a python module for a topological representation of quantum circuits. The [PR](https://github.com/qutip/qutip/pull/603) is currently under review and will be added to QuTiP - a quantum toolbox in Python.

- QuTiP feedstock maintainer

- Experience in Scientific Computing and Machine Learning using Python. 

- Simulated a Hodgkin Huxley inspired model for electrical signaling in bacterial bio-films using Python during a winter internship at the [Institute of Mathematical Sciences, Chennai](https://www.imsc.res.in/) as part of the Computational Biology group.

- [CV](https://drive.google.com/file/d/0BwMeZIJ7VlY9SFNBbUJtTkdjN3c/view?usp=sharing)

## Other Experiences

- Mime artist and part of a 30 member group of performers with experience in silent acts, shadow acts, UV and LED shows. As a coordinator, involved with direction, logistics and management of the team for [shows](https://www.youtube.com/watch?v=6gVuEoj38RU) in front of audiences of more than 2000 people.

## Why this project?

As a Physics student, I got into programming while studying numerical analysis and methods to solve differential equations. As I wrote code for my own projects, I seldom cared about documentation or testing. This often resulted in my code having bugs which were resolved with quick fixes and made it difficult to be used by others. In order to overcome this, I turned to open-source. I was lucky to get selected as a Google Summer of Code 2016 student with Dipy. While working on the development of a new Python module for Dipy, I learnt the importance of writing robust and tested code. Software development in the real world is challenging and the next step after developing code is distribution. Several issues come up when you youre trying to package and distribute code for use on various platforms and conda is a step forward in tackling those issues. After an experience with developing code, now I wish to learn how distribution works and hence my interest in this project. In addition, the community of users and developers in conda-forge come from various areas and I would like to be an active member of this community. Google Summer of Code with conda-forge will give me a structured platform to work on a real world problem, blog about it so that others can learn and at the same time gain an experience in software distribution and packaging.


## Appendix

### References

[1] Conda recipe documentation : http://conda-forge.github.io/docs/recipe.html#

[2] Conda: Myths and Misconceptions: https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/