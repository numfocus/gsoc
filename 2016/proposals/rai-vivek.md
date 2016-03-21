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

An additional objective of this section is to focus on a simple design.

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

```python
import platform
platform.uname() # cross platform
platform.win32_ver() # windows
platform.mac_ver() # mac
platform.linux_distribution() # linux
```

#### Status report

* Software check status (pass/fail) along with relevant messages
* Version number of the found (installed) software
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

* Server verifies the data, opens database connection, updates the record.
  > As suggested in the ideas sections, SQLite will be a low-barrier, easier to
  > maintain database, appropriate for our needs. We will, however, use
  > SQLAlchemy Python ORM and avoid dealing with raw SQL queries improving both
  > readability and maintenance of code.

* Server responds with an OK status.

* Ability to export diagnostic data into JSON or CSV format.
  > It’ll allow us to separate the user interface from backend creating a modular
  > setup.

#### Database

Based on the current nature of collectable information, write-volume and
functionality required SQLite seems to be a fit choice. In this regard, the
database schema is an important component that determines how we are going to
store the collected data in the database. Following from
[discussion](https://github.com/numfocus/gsoc/pull/94#issuecomment-196363139)
[on GitHub](https://github.com/numfocus/gsoc/pull/94#issuecomment-196410391),
we decided in favor of using
[SQLAlchemy](http://flask-sqlalchemy.pocoo.org/latest/) Python ORM library
instead of using raw SQL queries to managed database operations.

The following illustrates a typical database model that can be defined:

```python
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///test.db'
db = SQLAlchemy(app)

class PackageInfo(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    package = db.Column(db.String(120))
    installed = db.Column(db.Boolean)
    version = db.Column(db.String(120))
    log = db.Column(db.String(1000))
    machine_id = db.Column(db.Integer, db.ForeignKey('machine_info.id'))

    def __repr__(self):
        return ("<PackageInfo(id={}, software={}, installed={}, "
                "version={}, log={}, machine_id={})>").format(self.id,
                                                              self.package,
                                                              self.installed,
                                                              self.version,
                                                              self.log,
                                                              self.machine_id)


class MachineInfo(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    workshopid = db.Column(db.String(120))
    system = db.Column(db.String(80))
    hostname = db.Column(db.String(120))
    release = db.Column(db.String(120))
    version = db.Column(db.String(120))
    machine = db.Column(db.String(120))
    processor = db.Column(db.String(120))
    email = db.Column(db.String(120))
    time = db.Column(db.DateTime)
    exit_status = db.Column(db.Integer)
    packages = db.relationship('PackageInfo', order_by=PackageInfo.id,
                               backref='machine_info')

    def __repr__(self):
        return ("<MachineInfo(id={}, workshopid={}, system={}, hostname={}, "
                "release={}, version={}, machine={}, processor={}, email={}, "
                "time={}, exit_status={})>").format(self.id,
                                                    self.system,
                                                    self.hostname,
                                                    self.release,
                                                    self.version,
                                                    self.machine,
                                                    self.processor,
                                                    self.email,
                                                    self.time,
                                                    self.exit_status)
```


Since we are obtaining the platform information using Python `platform` module
which is same across all platforms, the same database schema can accommodate all
systems.

A typical example of the database entry then may look like:

```python

m_1 = MachineInfo(workshopid='ugc@edu.in', system='Linux', hostname='ghostbook',
                  release='3.19.0-47-generic',
                  version="#53i~14.04.1-Ubuntu SMP Mon Jan 18 16:09:14 UTC 2016",
                  machine='x86_64', processor='x86_64', email='foo@bar.com',
                  time=datetime.utcnow(), exit_status=0)

m_2 = MachineInfo(workshopid='train@example.com', system='Darwin', hostname='MacBook-Air',
                  version='15.0.0',
                  release='Darwin Kernel Version 15.0.0: .. ',
                  machine='x86_64', processor='i386', email='bar@foo.com',
                  time=datetime.utcnow(), exit_status=0)

pip = PackageInfo(package='pip', installed=1, version='7.9', log='Upgrade', machine_id=1)
git = PackageInfo(package='git', installed=1, version='1.9', log='', machine_id=1)
hg = PackageInfo(package='hg', installed=0, version='', log='', machine_id=2)
python = PackageInfo(package='Python', installed=1, version='3.5.1', log='', machine_id=2)

db.session.add(git)
db.session.add(pip)
db.session.add(hg)
db.session.add(python)

db.session.add(m_1)
db.session.add(m_2)
db.session.commit()
```

Once the model is defined, queries [complex
queries](http://flask-sqlalchemy.pocoo.org/2.1/queries/) can be performed to
add, remove or update records in the database as per requirement.

A few examples of schema models are shown below:

```python
>>> pip = PackageInfo(package='pip', installed=1, version='7.9', log='Upgrade', machine_id=1)

>>> pip
<PackageInfo(id=1, software=pip, installed=True, version=7.9, log=Upgrade, machine_id=1)>

>>> # add more packages, machines

>>> MachineInfo.query.all()
[<MachineInfo(id=1, workshopid="ugc@edu.net", system=Linux, ..., exit_status=0)>,
 <MachineInfo(id=2, workshopid="example@ready.net", system=Darwin, ..., exit_status=0)>]

>>> MachineInfo.query.first().packages
[<PackageInfo(id=1, software=git, installed=True, version=1.9, log=, machine_id=1)>,
 <PackageInfo(id=2, software=pip, installed=True, version=7.9, log=Upgrade, machine_id=1)>]

```

#### Database migration

As the project progresses, database schema may require significant alteration
and will necessitate the use of a schema migration tool. Alembic is a database
migration tool for usage with SQLAlchemy database toolkit of Python that we’d
like to use. The Flask ecosystem also has a handy migration plugin
([Flask-migrate](https://flask-migrate.readthedocs.org/en/latest/)) built on top
of it.

We’ll revisit the requirement and setup of proper tools for database migration
after mid-term evaluation after receiving feedback from workshop deployments.

#### Concerns

There is a possibility that users may execute the test scripts several times,
submitting the report to the server and leading to duplication.

There might be a couple of ways to tackle this:

1. One way to avoid it would be to generate a unique `machine-id` and use
   that as a primary identifier key. This key will act as foreign key for key
   for other table and maintain consistency.
2. Just execute a SELECT statement on table `machine_info` and see if that
   information is already record. If yes, then update the records in
   `package_info` table. Essentially, all the attributes of `machine-info`
   table may constitute a `UNIQUE_KEY` constraint.
3. Handle it through a client side modification in installation-test script.

These will be [discussed
later](https://github.com/numfocus/gsoc/pull/94/files#r55733858) in due course
as the project evolves.

### User interface

* An easily navigable HTML report of diagnostic data.
  > A Twitter Bootstrap front-end with jQuery.

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

* add a new command line parameter which denotes whether the user *consents* to
  sending the report to an external server.
* catch abrupt halts (`SIGINT`, for example) and report it to the server.
  > If CLI switch for sending report is not specified, the script should fail
  > with an exit message requesting the user to rerun with flag enabled.

* create a local copy of report log if submission fails. The file can be
  directly saved in JSON format.

#### Installation manual

Following from the
[discussion](https://github.com/numfocus/gsoc/pull/94#discussion_r55920929), we
seek to write a installation manual of all the software currently checked by the
installation-test scripts. These instructions or installation-recipes will help 
create a consistent ecosystem, improve setup experience and save time.

## Schedule of deliverables

**Roughly by mid-term evaluation period, we hope to deploy final changes on the
client side i.e. installation-test scripts along with a basic server-database
framework.**

The concurrent feedback from the workshops will allow us to incorporate feedback
in the second half of the coding period. So that by the end of the period, we
already have a well-tested and polished code.

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

Update installation-test scripts. Write unit-tests for untested code and test
rigorously. Specially, the code that generates report, makes submission and
catches error.

**Deploy test scripts and installation manual to workshops. Request feedback.**

### July 6th - July 19th

Add API endpoints to obtain report data in JSON format (whether other formats
required, to be decided later).

Incorporate feedback from workshop deployments. Setup database migration tool as
necessary.

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

* [Wikipedia Gender Indicators](https://github.com/hargup/WIGI-website):
  Wikimedia supported, developer and maintainer,
statistical analysis, visualization, used Python and JavaScript. See it in
[action](http://wigi.wmflabs.org).

* [Afra](https://github.com/yeban/Afra): JavaScript, Commits to improve the user
    interface.

These projects incorporated relevant development experience (unit testing,
coverage, code quality etc.) useful for this project.

Please see all my contributions at [GitHub](https://github.com/vivekiitkgp).
I also blog occasionally at [shorts](https://vivekiitkgp.github.io).

## Academic Experience

I am a Biology enthusiast with a wide spectrum of interests in computing. I am
inclined towards developing or improving software tools in the field of
computational biology.

Currently, I am enrolled as a pre-final undergraduate student at *Indian
Institute of Technology Kharagpur*, studying Biotechnology and Biochemical
Engineering.

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

### Other plans

May to mid of July is available as summer vacation during which I expect to
remain right on timeline (30 hrs/week). Normal school hours resume after
mid-July during which my hours/week may reduce (20 hrs/week).

### Checklist

- [x] I contacted NumFOCUS mentors before writing my proposal
- [x] I showed you my contribution (it can be any form: proof-of-concept project idea, some sample code or just a link to your commits from other project)
- [x] I linked to my sample contribution from the proposal
- [x] I linked to my opened issues in numfocus/gsoc repository from the proposal
