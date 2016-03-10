# Survey Visualizer Application

## Abstract

Software Carpentry organizes workshops to teach computing skills and
tools to researchers. The attendees and the organizers of the
workshops fill in different surveys to allow the organization hosts
to gather insight into the attendees computing background while
the administrators collect long-term feedback.
These surveys, which are hosted at [SurveyMonkey], are being
analyzed using the SurveyMonkey web dashboard. Filtering the responses
by a particular workshop can be cumbersome for the organization hosts
and administrators as it requires a lot of clicks.

The aim of this project is to build a web application to collect
and visualize survey responses for every workshop, while providing
a better experience for the administrators and organization hosts.

## Technical Details

The *Survey Visualizer* application stack can be divided into:

 * Backend  : [Django]
 * Scheduler: [Cron]
 * Database : [PostgreSQL]
 * Frontend : [Charts.js] and [Bootstrap]

### Backend - Django Application

The survey visualizer application would be made using [Django]
to allow easier integration with [AMY] when required.

#### SurveyMonkey Django Application

The SurveyMonkey pluggable Django application would serve as
an intermediary between the SurveyMonkey API and the Django
ORM.

The usage of the app will look similar to the following snippet:

```python
from surveymonkey.models import Response
from workshop.models import Workshop


#Fetch all responses from the database
responses = Response.objects.all()
#Fetch responses filtered by the last workshop
last_workshop_responses = Response.objects.filter(
    workshop=Workshop.objects.latest()
)
# Fetch responses gathered within the last week
weekly_responses = Response.objects.filter(
    date_created=(datetime.date.today()-datetime.timedelta(days=7))
)
#Populate database with responses from SurveyMonkey API
new_responses = Response.objects.populate()
```

#### Django Project

The Django project will perform these tasks:
 * Exposing the `Response.object.populate()` directive through a
   management command.
 * Rendering and serving the views and templates.

### Scheduler - Cron

[Cron] will be used as a job scheduler to periodically run
the command `Responses.objects.populate()` at fixed
time intervals.
The interval duration would be dynamically determined by the
number of API calls left and the time of the day.

*Why Cron?*
 * Cron is the easiest job scheduling solution available to us
   and is available across all Unix environments.
 * > Premature optimization is the root of all evil

   -- DonaldKnuth

### Database - PostgreSQL

The Django app will use PostgreSQL or SQLite as its
database engine in production and SQLite database engine during
development to store the survey responses fetched by the
SurveyMonkey Django application.

### Frontend - Charts.js and Bootstrap

The user interface would use charts and graphs to visualize the
large amount of data aggregated from the survey responses.
[Charts.js] will be used to display interactive realtime charts on
the user interface while it communicates with the API defined
before.

The surveys contain a wide range of questions with multiple and
single option choices. Charts.js provides us with a very large
collection of chart types to experiment with.
The ones that would be most effective in conveying the information
include:
 * [Pie Charts]: Pie Charts can be used for questions with limited
   number of answer choices (usually binary).
 * [Bar Charts]: Bar Charts will be used to compare numeric value of
   a wide range of questions against one another.
 * [Tables]: Text inputs are tough to visualize and tables provide us
   an easy way to list them.

*Why Charts.js?*
 * Charts.js is the most popular JavaScript charting library on
   GitHub that fulfills our use case.
 * Charts.js is super lightweight as compared to other charting
   libraries.

## Schedule of Deliverables

### Community Bonding Period
During this period, the finer technical details and decisions will
be taken in consultation with the mentors. These include:
 * The charts to be used to visualize each question in the survey.
 * The database schema that would allow aggregation and segregation
   of survey responses according to workshops.

### May 25th -  June 7th
Implement the SurveyMonkey Django application.

### June 8th - June 21th
Write tests and complete the implementation and documentation
of the SurveyMonkey Django application.

### June 22th - July 5th
* Setting up the cron job to schedule polling of the SurveyMonkey
API at definite time intervals.
* **Mid term evaluation**

### July 6th - July 19th
* Implement per-workshop and aggregated views.
* Work on the UI.

### July 20th - August 2nd
* Implement charts and graphs for each question contained in a
  survey.

### August 3rd - August 16th
Work on integrating the survey visualizer application with AMY.

### August 17th - August 21th 19:00 UTC
* Document the project and the SurveyMonkey app.
* Cover all parts of the code with tests.

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
Some of them include:
 *  SurveyMonkey API:
    With reference to the [conversation] in the issue thread,
    `workshop_id` custom variable will be listed as a question
    in the [get_responses] API response. This should allow easier
    filtering as compared to explicitly asking the question
    *Which workshop did you attend?*.
    However, any workshops listed as a custom variable or as an
    answer to the above question would have different
    `answer_id` in different surveys. Thus, the many-to-many
    relationship between surveys and workshops would require an
    additional [through field] to track the `answer_id`s.
 *  Need for a graceful exit:
    The management command that polls the SurveyMonkey API must handle
    errors gracefully as Cron jobs fail silently.
Moreover, my contribution to AMY would allow me to get to work with
the integration in no time.

## Appendix:

A [sample application] involving the use of Bar Charts to compare
the number of responses for the various answers to the questions
listed in the [sample survey] can be viewed [here].

The conversation between the mentors can be viewed at
[numfocus/gsoc/issues/91].

## Contact

Name:       Aditya Narayan
Email:      narayanaditya95@gmail.com
Timezone:   UTC +0530


[SurveyMonkey]: http://surveymonkey.com/
[Django]: http://djangoproject.com/
[Django Rest Framework]: http://www.django-rest-framework.org/
[Cron]: https://wiki.archlinux.org/index.php/cron
[PostgreSQL]: http://www.postgresql.org/
[PostgreSQL-specific features]: https://docs.djangoproject.com/en/1.9/ref/contrib/postgres/fields/
[Bootstrap]: http://getbootstrap.com/
[Charts.js]: http://www.chartjs.org
[extensive chart gallery]: http://www.chartjs.org/docs
[Pie Charts]: http://www.chartjs.org/docs/#doughnut-pie-chart
[Bar Charts]: http://www.chartjs.org/docs/#bar-chart
[Tables]: http://getbootstrap.com/css/#tables
[AMY]: https://github.com/swcarpentry/amy
[migration]: https://github.com/swcarpentry/amy/issues/716
[SurveyMonkey API]: https://developer.surveymonkey.com/
[sample application]: https://github.com/narayanaditya95/survey-visualiser
[sample survey]: https://www.surveymonkey.com/r/HSKQPQQ
[here]: https://infinite-falls-5979.herokuapp.com/
[get_response_counts]: https://developer.surveymonkey.com/docs/methods/get_response_counts/
[get_respondent_list]: https://developer.surveymonkey.com/docs/methods/get_respondent_list/
[get_responses]: https://developer.surveymonkey.com/docs/methods/get_responses/
[conversation]: https://github.com/numfocus/gsoc/issues/91#issuecomment-194137475
[through field]: https://docs.djangoproject.com/en/1.9/ref/models/fields/#django.db.models.ManyToManyField.through
[DjangoCMS Cascade]: https://github.com/jrief/djangocms-cascade
[DjangoCMS-WOW]: https://pypi.python.org/pypi/djangocms-wow
[IIT Kharagpur]: http://iitkgp.ac.in/
[numfocus/gsoc/issues/91]: https://github.com/numfocus/gsoc/issues/91
