# EcoData Retriever

## Add taxonomic name resolution to the EcoData Retriever to facilitate data science approaches to ecology

**Please ask questions [here](https://github.com/numfocus/gsoc/issues).**

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

This project would extend the EcoData Retriever using Python to access one
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
* @henrysenyondo
* @sckott

## Improving reproducibility in science by adding provenance tracking to the EcoData Retriever

**Please ask questions [here](https://github.com/numfocus/gsoc/issues).**

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
* @henrysenyondo


## Upgrade to datapackage.json standard

**Please ask questions [here](https://github.com/numfocus/gsoc/issues).**

### Rationale

The [EcoData Retriever](http://ecodataretriever.org) is a Python based tool for
automatically downloading, cleaning up, and restructuring ecological data. It
does the hard work of data munging so that scientists can focus on doing
science.

One of the ways the Retriever makes it easy to add new datasets is by allowing
datasets to be added using simple plain text descriptions of the data that are
then parsed into Python. There is an emerging standard for this kind of metadata
called
[datapackage.json](http://dataprotocols.org/data-packages/#descriptor-datapackagejson).

### Approach

This project would upgrade the EcoData Retriever to use the datapackage.json
standard. Specifically this would involve:

* Understanding the metadata standard
* Extending the standard to include metadata that is required by the EcoData
  Retriever that is not part of the standard
* Converting the Python parser to take JSON as input instead of current YAML
  like scripts
* Converting existing scripts to the JSON standard
* (stretch goal) developing a simple web or command line tool for generating
  JSON for new datasets

### Challenges

The EcoData Retriever scripts contain information on how to clean and modify
data that is not currently part of the datapackage.json standard. Designing how
to include this information in a way that is easy to read by both humans and
computers will require good design work.

### Involved toolkits or projects

* The [EcoData Retriever](http://ecodataretriever.org)
* Python
* JSON
* [datapackage.json](http://dataprotocols.org/data-packages/#descriptor-datapackagejson)

### Degree of difficulty and needed skills

* Moderate Difficulty
* Knowledge of Python
* Knowledge of JSON

### Involved developer communities

The EcoData Retriever primarily interacts via issues and pull requests on
GitHub.

### Mentors

* @ethanwhite
* @henrysenyondo


## Add Python 3 support

**Please ask questions [here](https://github.com/numfocus/gsoc/issues).**

### Rationale

The [EcoData Retriever](http://ecodataretriever.org) is a Python based tool for
automatically downloading, cleaning up, and restructuring ecological data. It
does the hard work of data munging so that scientists can focus on doing
science.

As the scientific Python community increasingly transitions to Python 3,
supporting the newest version of Python will be important.

### Approach

* Using [2to3](https://docs.python.org/2/library/2to3.html) as a starting point
update the code that can support both both 2.7+ and Python 3.4+

### Challenges

Getting code to run on both Python 2 and Python 3 with the same code base can be
tricky.

### Involved toolkits or projects

* The [EcoData Retriever](http://ecodataretriever.org)
* Python

### Degree of difficulty and needed skills

* Relatively easy
* Knowledge of Python

### Involved developer communities

The EcoData Retriever primarily interacts via issues and pull requests on
GitHub.

### Mentors

* @ethanwhite
* @henrysenyondo
