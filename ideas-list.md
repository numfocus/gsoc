Here are the projects we offer to mentor this summer:

# Add taxonomic name resolution to the EcoData Retriever to facilitate data science approaches to ecology

## Rationale

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

## Approach

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

## Challenges

Scientific names are stored inconsistently across datasets, so it will be
necessary to either modifying the scripts
that hold information on each dataset to indicate the location of the
species information or use an existing ontology
to automatically identify the location.

## Involved toolkits or projects

* The [EcoData Retriever](http://ecodataretriever.org)
* Python
* The APIs for the taxonomic name resolution services.
* Relational database management systems (RDBMS) including MySQL,
PostgreSQL, SQLite

## Degree of difficulty and needed skills

* Moderate Difficulty
* Knowledge of Python
* Knowledge of interacting with web services via APIs is a plus, but could
be learned during the project

## Involved developer communities

The EcoData Retriever primarily interacts via issues and pull requests on
GitHub.

## Mentors

* @ethanwhite
* @bendmorris
* @sckott

# Improving reproducibility in science by adding provenance tracking to the EcoData Retriever

## Rationale

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

## Approach

This project would extend the EcoData Retriever using Python to store all
of the metadata necessary for
full reproduction in an associated SQLite database.

Specifically this would involve:

* Object oriented programming in Python
* Design an SQLite database for storing provenance information or use an
existing framework (e.g,. http://code.google.com/p/core-provenance-library/)
* Implement checks to make sure that the data is in the same form created
by the Retriever when retrieving provenance information

## Involved toolkits or projects

* The [EcoData Retriever](http://ecodataretriever.org)
* Python
* Relational database management systems (RDBMS) including MySQL,
PostgreSQL, SQLite
* Potentially an existing provenance library

## Degree of difficulty and needed skills

* Moderate Difficulty
* Knowledge of Python
* Some experience with SQL

## Involved developer communities

The EcoData Retriever primarily interacts via issues and pull requests on
GitHub.

## Mentors

* @ethanwhite
* @bendmorris

# Write a result-aggregation server for the installation-test scripts

## Background

[Software Carpentry has][1] [installation-test scripts][2] so students can check
that they've successfully installed any software required by their workshop.
However, we don't collect the results of student tests, which makes a number of
things more difficult than they need to be.  Statistics about installed versions
would make it easy to:

* Figure out how much trouble our students are having with installation (for a
  given workshop or in general).
* Make informed decisions about deprecating older versions of our various
  dependencies (see swcarpentry/bc#724, swcarpentry/sql-novice-survey#13,
  swcarpentry/git-novice#32, and [here][3]) or modernizing our lessons to
  catch up with current systems (see swcarpentry/workshop-template#157 and
  swcarpentry/workshop-template#159).

## Approach

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

## Challenges

Designing and implementing a simple API for storing test results,
error messages, diagnostic system information, etc.  We want a
robust, flexible system that's small and easy to maintain going
forward.

## Involved toolkits or projects

* Python, for extending the existing installation test scripts.
* A web framework like [Django][], [Flask][], [RoR][], [Express][],
  ...
* Relational database management systems (RDBMS) including MySQL,
  PostgreSQL, SQLite.

## Degree of difficulty and needed skills

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

## Involved developer communities

The Software Carpentry community primarily interacts via
[issues and pull requests on
GitHub][github] and the [`discuss@` mailing
list][discuss].  There's also [an IRC channel][irc].

## Mentors

* @wking

## Acknowlegements

Thanks to @xuf12 for the initial idea behind this project.

[1]: https://github.com/swcarpentry/workshop-template/tree/gh-pages/setup
[2]: https://github.com/wking/swc-setup-installation-test
[3]: https://github.com/swcarpentry/git-novice/pull/43#issuecomment-74177654
[Django]: https://www.djangoproject.com/
[Flask]: http://flask.pocoo.org/
[RoR]: http://rubyonrails.org/
[Express]: http://expressjs.com/
[github]: https://github.com/swcarpentry
[discuss]: http://software-carpentry.org/pages/join.html
[irc]: http://www.mozillascience.org/join-the-sciencelab-on-irc
