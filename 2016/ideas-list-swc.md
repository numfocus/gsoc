# Software Carpentry

## A survey responses visualizer application

**Please ask questions as issues [here][gsoc-issues].**

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
* if time allows, integration with [AMY][]   (workshop management tool for
  [Software Carpentry][]).

### Background

For every [Software Carpentry][] workshop, the organization hosts attendee
surveys:

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

Due to the fact that [AMY][] is a [Django][] application, it is suggested:

* to have experience with [Django][] and [Python][]
* to create the project as [Django application](https://docs.djangoproject.com/en/1.9/ref/applications/#projects-and-applications)

This will allow for faster integration with AMY, if the there's time left in
the Summer.

Project's interface will probably use one of many JavaScript plotting
libraries, so it is required to research them and select one most promising.

Finally, SurveyMonkey API uses
[OAuth](https://developer.surveymonkey.com/docs/guides/oauth-guide/) for
authentication.  Prospect students should get to know this method (see below).

### Degree of difficulty and needed skills

Difficulty: moderate. This will be a single application, but you'll be
designing and writing it from scratch.

Skills required to complete the project (you can apply without some of them):

* Knowledge of [Python][] and basics of [Django][].
* Basic knowledge of how to use JavaScript plotting library of your choice
* Knowledge of [Git][] for storing your work on [GitHub][]
* Knowledge of SurveyMonkey OAuth.

Any of these skills could be learned during the project and I will help you
with them, but you probably can't learn *all* of them during GSOC.

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

Finally, please contact us and let us know the link to your repository.

### Mentoring process

You'll meet with mentor every week on Skype/Hangouts. He'll review all your
changes, and suggest project design.

Weekly or biweekly you'll have to provide a blog post with updates on your
project.

### Additional perks

If you're interested in what [Software Carpentry][] does, we may be able to
offer you a place at online
[instructor training](http://software-carpentry.org/join/), if there is one
taking place during Google Summer of Code 2016.

### Contact

Questions: please create an issue [here][gsoc-issues].

Project proposal: please create a pull request to this repository.

Other: please use our [mailing list][].

  [Software Carpentry]: https://software-carpentry.org/
  [gsoc-issues]: https://github.com/numfocus/gsoc/issues
  [swc]: https://github.com/swcarpentry/workshop-template/tree/gh-pages/setup
  [swc-install]: https://github.com/wking/swc-setup-installation-test
  [Python]: https://www.python.org/
  [Django]: https://www.djangoproject.com/
  [AMY]: https://github.com/swcarpentry/amy
  [Flask]: http://flask.pocoo.org/
  [SQLite]: https://www.sqlite.org/
  [python-requests]: http://docs.python-requests.org/en/master/
  [Git]: http://git-scm.com/
  [GitHub]: https://github.com/
  [mailing list]: https://groups.google.com/a/numfocus.org/forum/#!forum/gsoc
