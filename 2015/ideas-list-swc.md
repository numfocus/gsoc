# Software Carpentry

## Add taxonomic name resolution to the EcoData Retriever to facilitate data science approaches to ecology

**Please ask questions [here](https://github.com/swcarpentry/gsoc2015/issues/1).**

### Rationale

The [EcoData Retriever](http://ecodataretriever.org) is a Python based tool
for automatically downloading, cleaning up,
and restructuring ecological data. It does the hard work of data munging so
that scientists can focus on doing science.

One of the challenges of ecological (and evolutionary) data is that the
names of species are constantly being
redefined. This makes it difficult to combine datasets to do interesting
science. By automating reconciliation of
different species names as part of the process of accessing the data in the
first place it will become much easier
to combine diverse datasets and in new and interesting ways.

### Approach

This project would extendthe EcoData Retriever using Python to access one
or more of the existing web
services that reconcile species names
(e.g., [iPlant's Taxonomic
Name Resolution Service](http://tnrs.iplantcollaborative.org/TNRSapp.html))
and automatically replacing the species scientific names in the generated
databases. Specifically this
would involve:

* Object oriented programming in Python
* Using Python to query web service APIs

### Challenges

Scientific names are stored inconsistently across datasets, so it will be
necessary to either modifying the scripts
that hold information on each dataset to indicate the location of the
species information or use an existing ontology
to automatically identify the location.

### Involved toolkits or projects

* The [EcoData Retriever](http://ecodataretriever.org)
* Python
* The APIs for the taxonomic name resolution services.
* Relational database management systems (RDBMS) including MySQL,
PostgreSQL, SQLite

### Degree of difficulty and needed skills

* Moderate Difficulty
* Knowledge of Python
* Knowledge of interacting with web services via APIs is a plus, but could
be learned during the project

### Involved developer communities

The EcoData Retriever primarily interacts via issues and pull requests on
GitHub.

### Mentors

* @ethanwhite
* @bendmorris
* @sckott

## Improving reproducibility in science by adding provenance tracking to the EcoData Retriever

**Please ask questions [here](https://github.com/swcarpentry/gsoc2015/issues/2).**

### Rationale

The [EcoData Retriever](http://ecodataretriever.org) is a Python based tool
for automatically downloading, cleaning up,
and restructuring ecological data. It does the hard work of data munging so
that scientists can focus on doing science.

Science is experiencing a reproducibility crisis in that it is becoming
clear that published results often cannot be
reproduced. Part of the challenge of reproducibility in data science style
research is that most of the steps
related to data: downloading it, cleaning it up, restructuring it, are
either done manually or using one-off scripts
and therefore this phase of the scientific process is not reproducible. The
EcoData Retriever already solves many of
these problems, but it doesn't currently keep track of exactly what has
been done and therefore fails to support
full reproducible workflows.

### Approach

This project would extend the EcoData Retriever using Python to store all
of the metadata necessary for
full reproduction in an associated SQLite database.

Specifically this would involve:

* Object oriented programming in Python
* Design an SQLite database for storing provenance information or use an
existing framework (e.g,. http://code.google.com/p/core-provenance-library/)
* Implement checks to make sure that the data is in the same form created
by the Retriever when retrieving provenance information

### Involved toolkits or projects

* The [EcoData Retriever](http://ecodataretriever.org)
* Python
* Relational database management systems (RDBMS) including MySQL,
PostgreSQL, SQLite
* Potentially an existing provenance library

### Degree of difficulty and needed skills

* Moderate Difficulty
* Knowledge of Python
* Some experience with SQL

### Involved developer communities

The EcoData Retriever primarily interacts via issues and pull requests on
GitHub.

### Mentors

* @ethanwhite
* @bendmorris

## Write a result-aggregation server for the installation-test scripts

**Please as questions [here](https://github.com/swcarpentry/gsoc2015/issues/3).**

### Background

[Software Carpentry][swc] has [installation-test scripts][swc-install] so students can check
that they've successfully installed any software required by their workshop.
However, we don't collect the results of student tests, which makes a number of
things more difficult than they need to be.  Statistics about installed versions
would make it easy to:

* Figure out how much trouble our students are having with installation (for a
  given workshop or in general).
* Make informed decisions about deprecating older versions of our various
  dependencies (see swcarpentry/bc#724, swcarpentry/sql-novice-survey#13,
  swcarpentry/git-novice#32, and [here][git-novice-pull-comment]) or modernizing our lessons to
  catch up with current systems (see swcarpentry/workshop-template#157 and
  swcarpentry/workshop-template#159).

### Approach

This project would:

* Create a model for installed packages, failed package checks, and
  diagnostic system information.
* Design an API so clients can submit the results of their
  installation-test script.
* Write a small server to serve the API, store the results in a
  relational database, and allow administators to analyze the content.
* Update the installation-test scripts to (optionally) submit their
  results to the new server.

I'm not particular about the web framework you use to write the
server, but I have the most experience with [Django][] and
[Flask][].  If you prefer a different framework, I'm fine with
anything that takes care of the boilerplate and lets you focus on
the high-level tasks.

### Challenges

Designing and implementing a simple API for storing test results,
error messages, diagnostic system information, etc.  We want a
robust, flexible system that's small and easy to maintain going
forward.

### Involved toolkits or projects

* Python, for extending the existing installation test scripts.
* A web framework like [Django][], [Flask][], [RoR][], [Express][],
  ...
* Relational database management systems (RDBMS) including MySQL,
  PostgreSQL, SQLite.

### Degree of difficulty and needed skills

* Moderate difficulty.  This will be a simple server
  application, but you'll be designing and writing it from
  scratch.
* Knowledge of Python, to write client-side code for the
  installation test scripts.
* Knowledge of a web framework and basic API design.
* Knowledge of relational databases or a wrapping library
  for storing the results.

Any of these skills could be learned during the project,
but you probably can't learn *all* of them during the
project ;).

### Mentors

* @wking

### Acknowlegements

Thanks to @xuf12 for the initial idea behind this project.

## Amy: A Web-Based Tool for Managing Workshops

**Please ask questions [here](https://github.com/swcarpentry/gsoc2015/issues/6).**

[Software Carpentry][swc] workshops are currently managed by hand
using a complex (and fragile) mix that includes SQL files stored in GitHub,
regional mailing lists,
and other bits and pieces that have been cobbled together over the last five years.
Learning how to drive all of this takes a lot of time,
and is a major barrier to partner organizations getting more involved in workshop setup,
so we have started building [Amy][amy],
a Django application for managing workshops.
Basic functionality is in place,
but many features have not yet been implemented,
and it needs a lot of improvements in usability
(not to mention a lot more testing).

### Technical Details

Amy is a straightforward [Django][Django] application;
experience with that framework (and with Python) is required.
Some of the features we would like to add require Javascript,
so familiarity with [JQuery][jquery] and other frameworks is an asset.

Amy will manage personal identifying information,
so applicants should also have a basic understanding of security engineering.
Experience with deploying and maintaining applications is also an asset.

### Mentors

* @gvwilson
* @wking
* @rgaiacs

### Acknowledgments

Thanks to everyone who has helped get [Amy][amy] this far.

# Lessons as Packages

<https://github.com/swcarpentry/gsoc2015/issues/25>

There has been a great deal of interest lately in using virtual
machine technologies such as Docker to improve the reproducibility of
computational results in scientific research.  There has been much
less discussion of whether the same end could be achieved just as
well, or better, using another technology: package managers.  This
project will explore the extent to which cross-platform, open source
package managers can aid reproducible research, both on their own and
in conjunction with other tools, by building and evaluating a
prototype of such a system.

## Technical Details

A package manager is a collection of software tools that automates the
process of installing, upgrading, configuring, and removing software
packages for a computer's operating system in a consistent manner. It
typically maintains a database of software dependencies and version
information to prevent software mismatches and missing prerequisites.

Packages are distributions of software, applications and data.  A
package typically contains metadata such as a description of its
purpose, its version, and a list of dependencies necessary for the
software to run properly.  Upon installation, this metadata is stored
in a local package database, so that the package manager can detect
and handle dependencies and conflicts between different packages
(e.g., figure out what to do when Package A needs one version of
Library X, but Package B is using an older one or needs a newer one).

The existence of packaging tools has fostered the foundation and
growth of archive sites such as CPAN (the Comprehensive Perl Archive
Network), CRAN (which serves the same role for R), and PyPI (the
Python Package Index).  Many of these sites host scientific software,
but this software is divorced from the papers that use it (which at
best are hosted on sites such as arxiv.org with links embedded in
their LaTeX or Word source, or published PDFs, pointing at a few key
libraries).  Further, much of the "last ten yards" of computational
research --- the small scripts that use those libraries to produce
specific figures and tables --- are in random locations in verison
control repositories if they are publicly available at all.

### Proposal

Programmers think of a package as being software that might be
accompanied by some text (such as a reference manual) and data (such
as configuration files or icon images).  We propose to invert this
point of view and consider a package as text (such as a research
paper) and data (the paper's raw material) that is accompanied by some
software (the scripts used to turn that data into that paper).

If a scientist has gone to the trouble of creating a manifest for a
particular research paper for conda, brew, or bower, anyone who wants
to "install" that paper --- i.e., configure a computer so that it can
reproduce the work presented in the paper --- will be able to do so by
typing something like:

    $ conda install some-identifier-for-the-paper

After this, the package user will have the same libraries, programs,
data sets (or links to data sets), style files, and other artifacts
used to produce the paper.  With a bit more work on our part, the
package user should even be able to type:

    $ conda install doi://1234.5678/v2

to set up an environment in which she can reproduce the work presented
in Version 2 of the paper with the specified DOI.

We believe this approach merits exploration because:

1.  It leverages tools that scientific programmers don't have to build
    or maintain (and which many of them already have installed).

2.  It is auditable: a package manifest is a testable description of
    what is needed to reproduce a particular piece of work.

3.  It directly addresses the twin issues of extension and remixing.

This last point bears further discussion.  Researchers often want to
reproduce a colleague's computational setup in order to do further
work on top of it, or to combine its tooling with that used in some
other paper or papers.  Virtual machines are not well suited to this:
if, for example, a researcher wants to create a new pipeline by
pushing data through the environments created by three other people,
she must either hope that their VMs come with usable web service APIs,
or use some sort of spooling system to move data between them via the
filesystem.  If, on the other hand, those three papers's computational
enviornments could be installed in one OS image using an off-the-shelf
package manager, the effort needed to remix them would probably be
substantially less.

### A Use Case

One particular use case for this system is paper review.  Many
advocates of open science believe that reviewers should re-run the
calculations used to produce the papers they are reviewing.  Setting
up a machine to do this can take anywhere from a few minutes forever,
depending on how well the computational environment is documented and
how openly available the software is.  By reducing this effort, a
system such as the one we propose could increase the frequency with
which reviewers do more than just read their colleagues's work when
reviewing it.

### Plan

Today's package managers can be used to create re-runnable versions of
papers right now.  In practice, though, creating a package manifest
for each paper would require more effort than most scientists would be
willing to put in, and the results of installation would seem odd.
(Most people wouldn't think to look for a paper in /usr/share/man.)
Off-the-shelf package managers also don't cater for "installing"
remote data, which is likely to be a common use case for researchers.

We therefore propose that a GSoC student should:

1.  Extend an open source off-the-shelf package manager in ways that
    simplify the creation of per-paper packages by the average
    scientist.

2.  Conduct a small-scale user study of that tool to determine whether
    a full-scale version might be adopted, and if so, what changes
    would be needed to increase the likelihood of that happening.

3.  Explore ways in which this approach could complement others that
    are currently being explored, particularly VM-based approaches.

A very simple proof-of-concept system exists at <https://github.com/swcarpentry/installable-lesson-demo-01>.

## Mentors

* @gvwilson
* @rgaiacs

## Appendix

[swc]: https://github.com/swcarpentry/workshop-template/tree/gh-pages/setup
[swc-install]: https://github.com/wking/swc-setup-installation-test
[git-novice-pull-comment]: https://github.com/swcarpentry/git-novice/pull/43#issuecomment-74177654
[Django]: https://www.djangoproject.com/
[Flask]: http://flask.pocoo.org/
[RoR]: http://rubyonrails.org/
[Express]: http://expressjs.com/
[github]: https://github.com/swcarpentry
[discuss]: http://software-carpentry.org/pages/join.html
[irc]: http://www.mozillascience.org/join-the-sciencelab-on-irc
[amy]: http://github.com/swcarpentry/amy
[jquery]: http://jquery.com/
