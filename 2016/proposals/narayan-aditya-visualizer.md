# Survey Visualizer Application

## Abstract

Software Carpentry organizes workshops to teach computing skills and
tools to researchers. The attendees and the organizers of the
workshops fill in different surveys to allow the organization hosts
to gather insight into the attendees computing background while
the admininstrators collect long-term feedback.
These surveys, which are hosted at [SurveyMonkey], are being
analyzed using the SurveyMonkey web dashboard. Filtering the responses
by a particular workshop can be cubersome for the organization hosts
and admininstrators as it requires a lot of clicks.

The aim of this project is to build a web application to collect
and visualize survey responses for every workshop, while providing
a better experience for the admininstrators and organization hosts.

## Technical Details

The *Survey Visualizer* application stack can be divided into:

 * Backend  : [Django Rest Framework]
 * Scheduler: [Cron]
 * Database : [PostgreSQL]
 * Frontend : [Google Charts] and [Bootstrap]

### Backend - Django Application

The survey visualizer application would be made using
[Django Rest Framework] (henceforth called DRF) over [Django]
to allow easier integration with [AMY] when required.

The tentative API structure exposed by DRF is:
 * GET `/api/workshop/`  
   Lists all workshops that have surveys collecting responses.
 * GET `/api/workshop/<workshop_id>/`  
   Returns details of a particular workshop.
 * GET `/api/workshop/<workshop_id>/survey/`  
   Lists all the surveys whose responses are being collected.
   Each survey has its corresponding `survey_id`.
 * GET `/api/workshop/<workshop_id>/survey/<survey_id>`  
   Returns the responses for the particular survey, listed
   according to the questions contained in the survey.
 * POST `/api/workshop/<workshop_id>/survey/<survey_id>/refetch`  
   Instructs the Django application to fetch the responses
   that the survey has received since the last time the
   SurveyMonkey API was hit.

The underlying Django app will perform these tasks:
 * Polling SurveyMonkey API management command:
   This management command would populate the database with survey
   responses fetched from the [SurveyMonkey API].
   The number of survey responses obtained from the [get_response_counts]
   API call will be compared to the number of survey responses
   saved in the database. The remaining responses will be fetched
   using the [get_respondent_list] and [get_responses] API calls.
 * Segregating the survey responses according to the answer to the
   question: *Which workshop did you attend?* or the custom variable
   `workshop_id`.
 * Rendering and serving the templates.

### Scheduler - Cron

[Cron] will be used as a job scheduler to periodically run
the *Polling SM API* management command at fixed time intervals.
The interval duration would be dynamically determined by the
number of API calls left and the time of the day.

### Database - PostgreSQL

The Django app will use [PostgreSQL] as its database engine to
store the survey responses fetched by the backend.

### Frontend - Google Charts and Bootstrap

The user interface would use charts and graphs to visualize the
large amount of data aggregated from the survey responses.
[Google Charts] will be used to display interactive charts on
the user interface while it communicates with the API defined
before.

The surveys contain a wide range of questions with multiple and
single option choices. Google Charts provides us with a very large
collection of chart types to experiment with.
The ones that would be most effective in conveying the information
include:
 * Pie Charts: Pie Charts can be used for questions with limited
   number of answer choices (usually binary).
 * Bar Charts: Bar Charts will be used to compare numeric value of
   a wide range of questions against one another.
 * Table: Text inputs are tough to visualize and tables provide us
   an easy way to list them.

A [sample application] involving the use of Bar Charts to compare
the number of responses for the various answers to the questions
listed in the [sample survey] can be viewed [here].

## Schedule of Deliverables

### Community Bonding Period
During this period, the finer technical details and decisions will
be taken in consultation with the mentors. These include:
 * The charts to be used to visualize each question in the survey.
 * The database schema that would allow aggregation and segregation
   of survey responses according to workshops.

### May 25th -  June 7th
Start implementation of the *Polling SM API* management command
to fetch survey responses from SurveyMonkey API.

### June 8th - June 21th
Write tests and complete the implementation of the above
management command.

### June 22th - July 5th
* Setting up the cron job to schedule polling of the SurveyMonkey
API at definite time intervals.
* **Mid term evaluation**

### July 6th - July 19th
* Implement segregation of survey responses by `workshop_id`.
* Implement the API to be exposed by DRF.

### July 20th - August 2nd
* Work on the UI.
* Implement charts and graphs that use the API to aggregate
  survey responses.

### August 3rd - August 16th
Work on integrating the survey visualizer application with AMY.

### August 17th - August 21th 19:00 UTC
Document the project and the API.
Cover all parts of the code with tests.

## Future works

I would continue working on the project, making improvements and
fixing bugs as reported on the issue tracker. I will continue to
support AMY and make contributions to support the integration.

## Open Source Development Experience

I have contributed to a few open-source projects during the summers
of 2015:
* [AMY]: I have contributed 6 commits to AMY during the period March
  to July, 2015. These included basic bug fixes and changes to
  the form template logic.
* [DjangoCMS Cascade]: I was responsible for the overhaul of the
  testing framework to allow parallel tests of the codebase over
  different version of Python interpretor and dependencies. I also
  helped in the upgrading the codebase to be compatible with Django 1.8.
* [DjangoCMS-WOW]: Published an addon for the DjangoCMS ecosystem to the
  Python Package Index allowing developers to use the Javascript library,
  WOW with DjangoCMS.

I have also contributed to web projects that were undertaken by
the student community.

## Academic Experience

I am a third year undergraduate student at [IIT Kharagpur], studying
Electronics and Electrical Communication. I have undertaken courses in
control theory, internet architecture, operations research and other
varied interests.

## Why this project?

I have worked on Django for two years and communicated with different
APIs such as Google and Parse.  I have experimented and studied the
SurveyMonkey API during the proposal phase and I am aware of the
difficulties that I would face during the project.
Moreover, my contribution to AMY would allow me to get to work with
the integration in no time.

[SurveyMonkey]: http://surveymonkey.com/
[Django]: http://djangoproject.com/
[Django Rest Framework]: http://www.django-rest-framework.org/
[Cron]: https://wiki.archlinux.org/index.php/cron
[PostgreSQL]: http://www.postgresql.org/
[Google Charts]: https://developers.google.com/chart/
[Bootstrap]: http://getbootstrap.com/
[AMY]: https://github.com/swcarpentry/amy
[SurveyMonkey API]: https://developer.surveymonkey.com/
[sample application]: https://github.com/narayanaditya95/survey-visualiser
[sample survey]: https://www.surveymonkey.com/r/HSKQPQQ
[here]: https://infinite-falls-5979.herokuapp.com/
[get_response_counts]: https://developer.surveymonkey.com/docs/methods/get_response_counts/
[get_respondent_list]: https://developer.surveymonkey.com/docs/methods/get_respondent_list/
[get_responses]: https://developer.surveymonkey.com/docs/methods/get_responses/
[DjangoCMS Cascade]: https://github.com/jrief/djangocms-cascade
[DjangoCMS-WOW]: https://pypi.python.org/pypi/djangocms-wow
[IIT Kharagpur]: http://iitkgp.ac.in/
