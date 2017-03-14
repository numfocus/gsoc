# Enhancing conda-smithy
Development of a conda-skeleton for recipe generation and automatic updating from PyPI and CRAN


## Abstract

The conda package manager provides a tool, `conda-skeleton`, that makes the creation of conda recipes based on PyPI and CRAN easy. The recipe is a directory holding metadata and the scripts needed to build the package. The conda package is then built using the conda recipe. In a similar way, conda-forge recipes are used to build conda-smithy packages. However, the standard recipe output from conda-skeleton requires some heavy editing to conform with conda-forge standards. This project entails the creation of a conda-smithy skeleton for recipe generation. This can be developed further for automating recipe generation from PyPI and CRAN with minimal user input.

## Technical Details

### Objectives
- Creating a `conda-smithy skeleton` for simplifying the build process for conda-forge packages.
- Developing tools for conda-forge recipe generation for packages similar to `conda skeleton`. 
- Simplifying and automating the package updating process by developing tools to regenerate recipes based on the latest package from various sources - PyPI, CRAN, Github.

### Background
#### conda : 
An open source cross-platform package and environment manager for installing and distributing software across multiple platoforms. Conda works on Linux, OS X and Windows and aims to make switching across platforms easy. Once a binary package is built, the distribution is uploaded to a Continuum repository so that users can easily install them in any platform.

#### conda-forge:
Conda is maintained by Continuum and in order to have an open, non-Continum based tool similar to conda, a community led project started in 2016 called `conda-forge`. It contains tools for creation of community-driven builds for any package. Packages are maintained in the open via Github, unlike conda, with binaries automatically built using free CI tools. Conda-forge has the tool `conda-smithy` for building packages and it is a work in progress. This GSoC project will deal with enhancing `conda-smithy` for automating recipe generation and better integration with various repositories - PyPI, CRAN, Github.

#### recipe
`conda` and `conda-forge` build packages by using a `recipe` which contains files with information for the build process. A recipe contains build scripts, files specifying metadata and other resources such as README, icon files, or build notes. The template of a recipe is slightly different in conda-forge than that of a conda recipe. Due to this, a heavy editing of recipes is required from the users side. 

### Plan of work
A [PR](https://github.com/conda-forge/conda-smithy/pull/480) has been opened for discussion regarding a new `conda-smithy skeleton command`. We will be closely following the `conda skeleton` tool for better integration with conda. The [skeletonize](https://github.com/conda/conda-build/blob/master/conda_build/skeletons/pypi.py#L302) functions are recipe generators for PyPI and CRAN packages which can be found in the [skeleton folder](https://github.com/conda/conda-build/tree/master/conda_build/skeletons) in conda-build. A similar command for `conda-smithy` will be developed to regenerate a conda-smithy recipe based on the latest package version. Conda-skeleton will also contain tools for automatically generating recipes from PyPI, CRAN and Github.

- Removal of the build scripts in lieu of a script line for simple Python and R Packages;
- Removal of the boilerplate comments;
- Use of a jinja variable for the package version;
- Enforce the extra metadata like the maintainers list;
- Enforce the new pypi.io URL;
- Enforce sha256 instead of md5;
- Automatically add the sha256 from warehouse.


## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

Community bonding and getting to know other developers, recommended practices and usage of conda-smithy. Reading documentation and an attempt to build a new Python package in conda-forge from scratch.

### May 29th - June 3rd

Addition of a skeleton argument to `conda-smithy` for generating basic recipe files. This will follow the code in conda's `skeletonize` functions. The difference would be in the recipe templates for conda-forge and conda. Testing for the new skeleton command.

### June 5th - June 9th

Developing the conda skeleton command for actual recipe generation from a locally store package enforcing the following:

- Removal of the boilerplate comments
- Use of a jinja variable for the package version
- Enforce the extra metadata like the maintainers list
- Enforce the new pypi.io URL
- Enforce sha256 instead of md5

### June 12th - June 16th

Extending the skeleton command for automatic adding of sha256 from warehouse, and collecting other information from PyPI (requirements). Testing this new functionality for the skeleton command for PyPI packages.

### June 19th - June 23th, **End of Phase 1**

Removal of the build scripts in lieu of a script line for simple Python. Testing of the script by building Python packages locally as well as from PyPI

### June 26 - June 30th, **Begin of Phase 2**

Once the skeleton is tested for Python packages, the next part is to do the same for R packages from CRAN.

### July 3rd - July 7th

Testing and resolving issues with the skeleton command for PyPI, CRAN and local packages. Building packages should be easy with the new conda skeleton command now for Python and R packages. Complete all testing from online sources - PyPI, CRAN.

### July 10th - July 14th

Add recipe generation from Github repositories. In principle, anybody can use the skeleton to build their forked version of a package but a distinction should be there for the official version of the repository and an user's fork.

### July 17th - July 21th, **End of Phase 2**

This part should mainly be about testing the complete skeleton tool and making it merge ready. As such, documentation, testing and code review will happen at this stage.

### July 24th - July 28th, **Begin of Phase 3**

{{ Delieverables }}

### July 31st - August 4th

{{ Delieverables }}

### August 7th - August 11th

{{ Delieverables }}

### August 14th - August 18th

{{ Delieverables }}

### August 21st - August 25th, **Final Week**

{{ Delieverables }}

### August 28th - August 29th, **Submit final work**

## Future works

#### Automatically issue PRs to the feedstocks with package version updates from PyPI.

This idea will require reading the package current version from the feedstock, checking if a new version is available and issuing a PR against the feedstock repository. It should follow the MNT PRs model used to re-render feedstocks.


## Development Experience

- Google Summer of Code 2016 student with Python Software Foundation (Dipy)
- Potential QuTiP feedstock maintainer
- Experience in Scientific Computing and Machine Learning using Python
- [CV](https://drive.google.com/file/d/0BwMeZIJ7VlY9SFNBbUJtTkdjN3c/view?usp=sharing)

## Other Experiences

{{ Experience }}

## Why this project?

{{ Why you want to do this project? }}

## Appendix

{{ Extra content }}