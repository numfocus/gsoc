# Result aggregation server for installation-scripts

## Abstract

Software is indispensable for any modern scientist irrespective of their field
of work. However, many scientists lack or have limited exposure to tools and
practices that would allow them to write more reliable and maintainable code
with less effort.

Software Carpentry is a collective effort of volunteer instructors, computer
scientists and technology-enthusiasts with an aim of advancing research in
scientific disciplines by teaching necessary computing skills.

Software Carpentry works in this direction by organizing workshops, lessons or
simply through discussions. 

An important part of the workshop is to ensure that participants have installed
all the necessary software. Software Carpentry has
[installation-test](https://github.com/wking/swc-setup-installation-test)
scripts that enables one to check if they have successfully installed the
software required by their workshop. As an improvement to the these scripts, the
proposal lays out following objectives:

* Implement a server to store the results of installation-testing scripts (i.e.
platform people use, which software dependency worked and which did not, error
logs)
* Enhance the installation-test scripts by adding an option for users to collect
 results and send them to a server.

## Technical Details

The technical details of the project can be organized into following:

* Collectable information   : machine information, report
* Backend                   : design of REST API, framework, database, schema
* Frontend                  : import data, HTML reports, visualizations
* installation-test scripts : update the scripts to collect info and send request
* Documentation             : use travis-ci automated build system, follow pep8
* Testing                   : use travis-ci, coveralls for coverage

An additional objective of this section is to keep the design sweet and simple
(KISS).

### Collectable information

In this section, we discuss the parameters that can be collected by the SwC’s
installation-test scripts to help us analyze and improve installation setup for
future workshops.

The goal is to collect anonymized, non-identifiable machine information and
status report of the installation-test scripts.

#### Machine information

* Platform (or System)
* Architecture (or Machine)
* Node (Hostname), Processor
* OS (or Distribution), Release version, Name

Most of this information can be collected using the `platform` Python module.
A snippet of useful commands is shown below.

```
import platform
platform.uname() # cross platform
platform.win32_ver() # windows
platform.mac_ver() # mac
platform.linux_distribution() # linux
```

#### Status report

* Software check status (pass/fail) along with relevant messages
* Version number of the found (installed) softwares
* Any error that may have occurred in the script itself
* Process information
* Timestamp
* (Optional) User email


The current installation-test script implementation already collects the list of
*successful* and *failed* checks along with version information and error
message as available.

### Server

The server will collect the diagnostic report submitted by installation-scripts
when they are run on the user machine.

A simple design of the setup is discussed below:

#### API

The following steps are involved in the process:

* Installation-script sends a `POST` request to the server endpoint (for example, https://installation.software-carpentry.org/report).
  > Using Flask web-development framework for Python. A robust, flexible
  > choice that is easy to develop and maintain. Scalability is not an issue for
  > our use case.

* Server verifies the data, open database connection, updates the record.
  > As suggested in the ideas sections, SQLite will be a low-barrier, easier to
  > maintain database, appropriate for our needs.

* Send a status response back to the client. The response can contain feedback,
  suggestions or perhaps link to some survey.

* Ability to export diagnostic data into JSON or CSV format.
  > It’ll allow us to separate the user interface from backend creating a modular
  > setup.

#### Database

Based on the current nature of collectable information, write-volume and
functionality required SQLite seems to be a fit choice. In this regard, the
database schema is an important component that determines how we are going to
store the collected data in the database.

The following illustrates a typical schema that might be used:

```
TABLE machine_info {
  id PRIMARY KEY AUTO,
  system VARCHAR,
  hostname VARCHAR,
  release VARCHAR,
  version VARCHAR,
  machine VARCHAR,
  processor VARCHAR,
  email VARCHAR,
  time VARCHAR,
  exit_status INT
}


TABLE software_version {
  id PRIMARY KEY AUTO,
  software VARCHAR,
  installed BOOL,
  version VARCHAR,
  log VARCHAR
}
```

Since we are obtaining the platform information using Python `platform` module
which is same across all platforms, the same database schema can accommodate all
systems.

A typical example of the database entry then may look like:

```
(TABLE machine_info)

id | system  | hostname    | release             | version                                               | machine  | processor | email       | time                      | exit_status
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
1  | ‘Linux’ | ‘ghostbook’ | ‘3.19.0-47-generic’ | ‘#53~14.04.1-Ubuntu SMP Mon Jan 18 16:09:14 UTC 2016’ | ‘x86_64’ | ‘x86_64’  | foo@bar.com | ‘Tue Mar 8 00:52:35 2016’ | 0

(TABLE software_version)

id | software | installed | version | log
-----------------------------------------
1  | python   | True      | 3.5.1   | ‘’
1  | git      | True      | 1.9.1   | ‘’
1  | hg       | False     | ‘’      | ‘’
1  | pip      | True      | 7.9     | ‘A newer version of pip is available.’
```

Complex queries can be performed by `JOINING` the two tables and querying for
corresponding field of interest.

#### Concerns
Both tables share the unique identity given to a particular report.  There is
a possibility that same user may submit a report twice possibly because there
were some errors or something else causing duplication.

There might be a couple of ways to tackle this:

1. One way to avoid it would be to generate a unique `machine-id` and use
   that as a primary identifier key. This key will act as foreign key for key
   for other table and maintain consistency.
2. Just execute a SELECT statement on table `machine-info` and see if that
   information is already record. If yes, then update the records in
   `software_version` table. Essentially, all the attributes of `machine-info`
   table may constitute a `UNIQUE_KEY` constraint.

### User interface

* An easily navigable HTML report of diagnostic data.

* Add simple in-browser visualizations.
  > These visualizations can be served via backend using Python library like
  > Bokeh or rendered on the client using D3 (or equivalent) library. I prefer the
  > latter approach.

### Testing

The client-side needs to work reliably across different systems, mainly Linux,
Windows and OSX machines.

By default, I will develop and test the scripts on my Linux system. For Windows
and OSX, however, I will have to take help of mentors/community members or my
friends.

### Updating installation scripts

Modify the installation-test scripts to generate a diagnostic report containing
machine information and status report as described before. The script should:

* catch abrupt halts (`SIGINT`, for example) and report it to the server.
* should create a local copy of report log if submission fails.

## Schedule of Deliverables

### April 22nd - May 24nd

Community bonding period. Communicate with members of Software Carpentry
organization and learn about workshop organization, lesson preparation and other
details.

The close interaction will help me prepare necessary agenda for a Software
Carpentry workshop that I am planning to organize locally (India) and eventually
start an India chapter.

### May 25th -  June 7th

Set up repository. Automated coverage and integration test building using
[coveralls](https://coveralls.io) and [tavis-ci](https://travis-ci.org/). Will
follow a test-driven development style.

Also, finalize the collectable information, decide database and investigate its
worse case behavior, design schema, freeze component options.

### June 8th - June 21th

Setup server. Implement report collection, and database store, update and
retrieval operations.

Basic user-interface.

Write a secondary demo script that posts to server.

### June 22th - July 5th

**Mid-term evaluation**

Update installation-test scripts. Test rigorously.

### July 6th - July 19th

Add API endpoints to obtain report data in JSON format (whether other formats
required, to be decided later).

### July 20th - August 2rd

Upgrade user-interface. Design a responsive, navigable HTML report. Create
mock sketches and decide a good way to show information in coherent, easy to
understand manner. 

Allow downloading data (using the endpoint developed week before).

### August 3rd - August 16th

Implement a few client-side visualization. Or alternatively, write scripts to
analyse the data, helping us answer a few questions like:

* *Which dependency is most likely to be missing?*
* *Which step is most problematic for users?*
* *Which systems are used most commonly in a workshop?*
* *Whether the lessons/tutorials appropriately cover those cases?*

### August 17th - August 21th 19:00 UTC

**Week to scrub code, write tests, improve documentation, etc.**

**End term evaluations**

A few weeks in between act as buffer to compensate for unfortunate delays.

## Future works

The analysis obtained from the data will help Software Carpentry to revise and
update their lessons and workshop guides, streamline installation process and
manage software revisions.

The report generated through this tool might as well be linked to (anonymous)
survey responses. It might help us answer some really interesting questions
like:

* *Do some platform users find it more difficult to manage through the workshop
  than the others?*

## Open Source Development Experience

Collaborator in many open-source projects. A few that I am personally fond of:

* [Sequenceserver](https://github.com/wurmlab/sequenceserver):  > 115+ commits,
    bioinformatics software, developed in Ruby, Frontend and backend design

* [Wikipedia Gender Indicators](https://github.com/hargup/WIGI-website): Wikimedia supported, developer and maintainer,
statistical analysis, visualization, used Python and JavaScript. See it in [action](http://wigi.wmflabs.org).

* [Afra](https://github.com/yeban/Afra): JavaScript, Commits to improve the user
    interface.

Please see all my contributions at [GitHub](https://github.com/vivekiitkgp).
I also blog occasionally at [shorts](https://vivekiitkgp.github.io).

## Academic Experience

I am a Biology enthusiast with a wide spectrum of interests in computing. I am
inclined towards developing or improving software tools in the field of
computational biology.

## Why this project?

Having closely worked with the scientific community as a collaborator and
researcher myself, I have realized that there is a vast gap in how things on
done on either side. Scientists, often due to their limited exposure to research
computing, are forced to adopt tools or practices that are inefficient, time
consuming or simply outdated.

This is also the philosophy that let me to participate in developing
Sequenceserver, where we re-engineered user experience and optimized workflow;
something which is rarely seen in a bioinformatics software.

Software Carpentry, however, is an excellent effort in a direction to remedy the
problem. The specific choice of project is motivated from some of the analysis
questions that I have laid out before in my proposal. I believe this project
will us answer a few of those questions and reduce the gap between disciplines.

## Appendix

Relevant interaction with mentor and community can be found at
[swc-setup-instllation-test](https://github.com/wking/swc-setup-installation-test/issues/12)
and [numfocus/gsoc](https://github.com/numfocus/gsoc/issues/79).

### Sample code

A demo consisting of a client and web server, where the client sends platform
(e.g. operating system) details to the server, and servers stores that in the
database.

[Swc-demo](https://github.com/vivekiitkgp/swc-demo)

### Contact

Vivek Rai

vivekraiiitkgp@gmail.com

Indian Standard Time (+5.30 UTC)

vivekrai@freenode (IRC)

### Checklist

- [x] I contacted NumFOCUS mentors before writing my proposal
- [x] I showed you my contribution (it can be any form: proof-of-concept project idea, some sample code or just a link to your commits from other project)
- [x] I linked to my sample contribution from the proposal
- [x] I linked to my opened issues in numfocus/gsoc repository from the proposal
