# Software Carpentry

## Produce a SurveyMonkey responses analyzer application

**Please ask questions as issues [here](https://github.com/numfocus/gsoc/issues).**

### Abstract

Write a web application that presents any survey results, grabbed from
[SurveyMonkey API](https://developer.surveymonkey.com/), and aggregates them by
the workshop ID.

The key features are:

* survey selection,
* results aggregation by workshop ID,
* visual presentation of all of the results (there are questions of different
  kinds: some may be shown as histograms, some may require other chart or even
  a text display),
* caching of the results (we only have about 15000 API queries a day)
* a view for workshop hosts to see the survey results,
* if time allows, integration with [AMY](https://github.com/swcarpentry/amy)
  (workshop management tool for Software-Carpentry).

### Background

For every Software-Carpentry workshop, the organization hosts attendee surveys:

1. "pre-workshop" anonymous survey that helps instructors get insight into
   attendees computing experience, and details of their machines like operating
   system,
2. "post-workshop" anonymous survey that gets attendees' feedback on the
   workshop.

There are also two surveys for instructors and one long-term survey for
workshop attendees.

These surveys are shared among all of the workshops, and answers from
specific workshops are aggregated in two ways:

1. via query parameter (`/link/to/survey?workshop_id=2016-01-24-Boston`),
2. via explicit question in the survey ("Which workshop do you attend?").

The hard part here is creating aggregated view with the results for workshop
hosts.  It takes a lot of clicks in the SurveyMonkey web interface to do this
task, but thanks to their API we can do this automatically in our application.

### Technical details

Due to the fact that [AMY](https://github.com/swcarpentry/amy) is
a [Django](http://djangoproject.com/) application, it is suggested:

* to have experience with Django and Python
* to create the project as [Django application](https://docs.djangoproject.com/en/1.9/ref/applications/#projects-and-applications)

This will allow for faster integration with AMY, if the there's time left in
the Summer.

Project's interface will probably use one of many JavaScript plotting
libraries, so it is required to research them and select one most promising.

Finally, SurveyMonkey API uses
[OAuth](https://developer.surveymonkey.com/docs/guides/oauth-guide/) for
authentication.  Prospect students should get to know this method (see below).

### Mentors

* [@pbanaszkiewicz](https://github.com/pbanaszkiewicz)
  (timezone: UTC+1h [Poland])

### Interested in making this project? Here's what you should do

If you want to show interest in making this project, we expect you to do
a demo application. The application should grab responses from your
SurveyMonkey survey and present them on the web.

You'll need:

1. a SurveyMonkey account, and one survey
2. some test responses to the survey
3. access tokens for your account
4. public GitHub repository with your application

Please make sure your account doesn't store any real information as you'll have
to share the tokens publicly.

Finally, please contact with us and let us know the link to your repository.

### TL;DR: requirements

* suggested: Django, Python
* research: best fitting JavaScript plotting library
* OAuth authentication of SurveyMonkey API requests
* a demo application to show us you're interested in the project

### Mentoring process

You'll meet with mentor every week on Skype/Hangouts. He'll review all your
changes, and suggest project design.

Weekly or biweekly you'll have to provide a blog post with updates on your
project.

### Additional perks

If you're interested in what Software-Carpentry does, we may be able to offer
you a place at online
[instructor training](http://software-carpentry.org/join/), if there is one
taking place during Google Summer of Code 2016.

### Contact

Questions: please create an issue [here](https://github.com/numfocus/gsoc/issues).
Project proposal: please create a pull request to this repository.
Other: please use our [mailing list](https://groups.google.com/a/numfocus.org/forum/#!forum/gsoc).
