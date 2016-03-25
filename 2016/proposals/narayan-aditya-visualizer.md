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

[SurveyMonkey]: http://surveymonkey.com/

The aim of this project is to build a web application to collect
and visualize survey responses for every workshop using the
[SurveyMonkey developer API], while providing
a better experience for the administrators and organization hosts.

[SurveyMonkey Developer API]: https://developer.surveymonkey.com/

## Technical Details

The *Survey Visualizer* application stack can be divided into:

 * Backend  : [Django]
 * Scheduler: [Cron]
 * Database : [Django ORM]
 * Frontend : [Charts.js] and [Bootstrap]

[Django]: http://djangoproject.com/
[Django ORM]: https://docs.djangoproject.com/en/1.9/topics/db/models/
[Cron]: https://wiki.archlinux.org/index.php/cron
[Bootstrap]: http://getbootstrap.com/
[Charts.js]: http://www.chartjs.org

### Backend - Django Application

The survey visualizer application would be made using Django
to allow easier integration with [AMY] when required.

[AMY]: https://github.com/swcarpentry/amy

#### SurveyMonkey Django Application

The SurveyMonkey pluggable Django application would serve as
an intermediary between the SurveyMonkey API and the Django
ORM.

The SurveyMonkey app would consist of five Models:

* **Survey**: The *Survey* Model will store details about the surveys
  contained in the SurveyMonkey account.
   ```python
   class Survey(models.Model):
       """Represents a single survey"""
       LANGUAGES = (
           (1, 'English'),
           #...
       )

       objects = SurveyManager()

       survey_id       = models.IntegerField(unique=True)
       language_id     = models.PositiveIntegerField(choices=LANGUAGES)
       title           = models.CharField(max_length=100)
       nickname        = models.CharField(max_length=100)
       analysis_url    = models.URLField()
       preview_url     = models.URLField()
       date_created    = models.DateTimeField()
       date_modified   = models.DateTimeField()
       num_responses   = models.PositiveIntegerField()
   ```
   The surveys will be auto-populated by the [get_survey_details] API
   endpoint, which can be invoked through the `populate` method on its
   Model Manager.
   ```python
   class SurveyManager(models.Manager):
       def populate(self):
           # Hit the get_survey_details API endpoint
           # Populate the Survey, Question and Answer tables
           pass
   ```

[get_survey_details]: https://developer.surveymonkey.com/docs/methods/get_survey_details/

* **Respondent**: The *Respondent* Model stores basic user details
  of all respondents for a particular survey.
  ```python
  class Respondent(models.Model):
      RESPONDENT_STATUS = (
          (0, 'Incomplete'),
          (1, 'Complete'),
          (2, 'Disqualified'),
      )

      objects = RespondentManager()

      respondent_id   = models.IntegerField(unique=True)
      first_name      = models.CharField(max_length=100)
      last_name       = models.CharField(max_length=100)
      email           = models.EmailField()
      status          = models.IntegerField(choices=RESPONDENT_STATUS)
      survey          = models.ForeignKey('Survey')
  ```
  The respondents are fetched and auto-populated by the
  [get_respondent_list] API endpoint, which can be invoked through
  the `populate` method on its Model Manager.
  ```python
  class RespondentManager(models.Manager):
      def populate(self):
          # Hit the get_respondent_list API endpoint
          # Populate the Respondent table
          pass
  ```

[get_respondent_list]: https://developer.surveymonkey.com/docs/methods/get_respondent_list/

* **Question**: A survey contains a list of questions that are answered
  by the survey respondents. All questions have a family and subtype
  that define their type. The question types and formatting options
  have been defined at SurveyMonkey’s [Question types documentation].
  ```python
  class Question(models.Model):
    """Represents a single question"""

    question_id     = models.IntegerField(unique=True)
    heading         = models.CharField(max_length=100)
    survey          = models.ForeignKey('Survey')
    family_type     = models.CharField(max_length=15)
    subtype         = models.CharField(max_length=15)
  ```
  The questions are auto-populated by the `populate` method of
  the Survey model, which hits the [get_survey_details] API
  endpoint.

* **Answer**: Each question in the *Question* table has certain
  answers that the respondent can select or give as an input.
  Each answer has a type assigned to it, as listed at the
  [Data Types documentation].
  ```python
  class Answer(models.Model):
      """Represents one of the possible answers to a question"""
      ANSWER_TYPES = (
          ('row', 'one row of the question'),
          ('col', 'one column of the question'),
          ('other', '"other" answer'),
          # ...
      )

      answer_id       = models.IntegerField(unique=True)
      position        = models.IntegerField(null=True)
      text            = models.CharField(max_length=100)
      answer_type     = models.CharField(
          choices=ANSWER_TYPES,
          max_length=10,
          blank=True
      )
      visible         = models.NullBooleanField()
      weight          = models.IntegerField(null=True)
      apply_all_row   = models.NullBooleanField()
      is_answer       = models.NullBooleanField()
      question        = models.ForeignKey('Question', related_name='answers')
  ```
  The answers are auto-populated by the `populate` method of
  the Survey model, which hits the [get_survey_details] API
  endpoint.

[Data Types documentation]: http://help.surveymonkey.com/articles/en_US/kb/Available-question-types-and-formatting-options

* **Response**: Each row in the *Response* table associates a respondent
  with the question that was asked and the answer(s) that
  the respondent selected or inputted. A question can have answers
  associated as `row`, `col` or `col_choice` options; or contain a text
  response to an open-ended question (such as a textbox).
  ```python
  class Response(models.Model):
      """Represents the response to a particular question"""
      question        = models.ForeignKey('Question', related_name='responses')
      respondent      = models.ForeignKey('Respondent', related_name='responses')
      row             = models.ForeignKey('Answer', related_name='row', null=True)
      col             = models.ForeignKey('Answer', related_name='col', null=True)
      col_choice      = models.ForeignKey('Answer', related_name='col_choice', null=True)
      text            = models.CharField(max_length=100, blank=True)
  ```

The database schema for the SurveyMonkey application would look like:

![Database schema](./narayan-aditya-visualizer.png)

This schema allows us to filter survey responses across different
relationships:
* Responses submitted by a particular respondent  
  `Responses.objects.filter(respondent=respondent_obj)`  
* Responses filtered by answer to a particular question  
  `Responses.objects.filter(question__heading='Please select the workshop you attended').filter(row__text='Workshop A')`

#### Django Project

The Django project will perform these tasks:
* Exposing the `populate` method of the Model Managers of the above
  models through a management command.
* Rendering and serving the views and templates.

### Scheduler - Cron

[Cron] will be used as a job scheduler to periodically run
the command `Responses.objects.populate()` at fixed
time intervals (tentatively twice a day).

*Why Cron?*
 * Cron is the easiest job scheduling solution available to us
   and is available across all Unix environments.
 * > Premature optimization is the root of all evil

   -- DonaldKnuth


### Frontend - Charts.js and Bootstrap

The user interface would use charts and graphs to visualize the
large amount of data aggregated from the survey responses.
[Charts.js] will be used to display interactive charts on
the user interface.

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

 [extensive chart gallery]: http://www.chartjs.org/docs
 [Pie Charts]: http://www.chartjs.org/docs/#doughnut-pie-chart
 [Bar Charts]: http://www.chartjs.org/docs/#bar-chart
 [Tables]: http://getbootstrap.com/css/#tables

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
 * Improvements to the proposed database schema to cater to use
   cases not considered yet.
 * Switching to the recently released [SurveyMonkey
  Developer API v3].

[SurveyMonkey Developer API v3]: https://developer.surveymonkey.com/api/v3/

### May 25th -  June 7th
* Write tests for the SurveyMonkey Django application.
* Start implementation of  the SurveyMonkey Django application.

### June 8th - June 21th
* Test and complete the implementation and documentation
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
* Work on integrating the survey visualizer application with AMY.

### August 17th - August 21th 19:00 UTC
* Document the project and the SurveyMonkey application.
* Cover all parts of the code with tests.

## Future works

I would continue working on the project, making improvements and
fixing bugs as reported on the issue tracker. I will continue to
support AMY and make contributions to support the integration.

## Open Source Development Experience

I have contributed to a few open-source projects during the summers
of 2015:
* [AMY]:  
  I have contributed 6 commits to AMY during the period March
  to July, 2015. These included basic bug fixes and changes to
  the form template logic.

[AMY]: https://github.com/swcarpentry/amy

* [DjangoCMS Cascade]:  
  I was responsible for the overhaul of the
  testing framework to allow parallel tests of the codebase over
  different version of Python interpretor and dependencies. I also
  helped in the upgrading the codebase to be compatible with Django 1.8.

[DjangoCMS Cascade]: https://github.com/jrief/djangocms-cascade

* [DjangoCMS-WOW]:  
  Published an addon for the DjangoCMS ecosystem to the
  Python Package Index allowing developers to use the Javascript library,
  WOW with DjangoCMS.

[DjangoCMS-WOW]: https://pypi.python.org/pypi/djangocms-wow

I have also contributed to web projects that were undertaken by
the student community.

## Academic Experience

I am a third year undergraduate student at [IIT Kharagpur], studying
Electronics and Electrical Communication. I have undertaken courses in
control theory, internet architecture, operations research and other
varied interests.

[IIT Kharagpur]: http://iitkgp.ac.in/

## Why this project?

I have worked on Django for two years and communicated with different
APIs such as Google and Parse.  I have experimented and studied the
SurveyMonkey API during the proposal phase and I am aware of the
difficulties that I would face during the project.
Some of them include:
* Upgradation to [SurveyMonkey Developer API v3]:  
  SurveyMonkey released the third version of their developer API
  on 14th March, 2016. Its the greatest and the most feature-rich API
  release that all new features will be built/benchmarked upon.  
  With the introduction of [webhooks] support, the SurveyMonkey Django
  application can, without the use of a job scheduler, populate the
  Models with callbacks from the API.

[SurveyMonkey Developer API v3]: https://developer.surveymonkey.com/api/v3/
[webhooks]: https://developer.surveymonkey.com/api/v3/?python#webhooks

* Need for a graceful exit:  
  The management command that invokes the `populate` method of Model
  Managers  must handle errors gracefully as Cron jobs fail silently.

Moreover, my contribution to AMY would allow me to get to work with
the integration in no time.

## Appendix:

A [sample application] involving the use of Bar Charts to compare
the number of responses for the various answers to the questions
listed in the [sample survey] can be viewed at [Heroku].

[sample application]: https://github.com/narayanaditya95/survey-visualiser
[sample survey]: https://www.surveymonkey.com/r/HSKQPQQ
[Heroku]: https://infinite-falls-5979.herokuapp.com/

The conversation between the mentors can be viewed at
[numfocus/gsoc/issues/91].

[numfocus/gsoc/issues/91]: https://github.com/numfocus/gsoc/issues/91

## Availability

**Time Zone**:      UTC +0530  
**Minimum hours per week**: 40 hours

My university classes begin on 18th July, 2016. I will still be able
to devote 30-40 hours per week till the end of the coding period.

## Contact

**Name**:   Aditya Narayan  
**Email**:  narayanaditya95@gmail.com
