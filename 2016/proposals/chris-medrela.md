# Manage workflow for Software/Data Carpentry instructor training

## Abstract

Software Carpentry (SWC) has a workflow for Instructor Trainings. Currently, it uses Etherpads and Google Docs spreadsheets, which slows down SWC Admins and Trainers whose time is already precious. The goal is to extend AMY (Django-base web application for SWC Admins) to manage the process of training people to be instructors, to avoid manual data copying and synchronization and make the entire process smoother.

[The appendix](#appendix) at the end of this proposal contains [description of the current workflow] and [list of issues] based on [Greg Wilson's pull request] and its discussion. Also, whenever something is written Title-cased, you can look it up in the [glossary](#glossary) in the [appendix](#appendix) at the end of this proposal. Hopefully, the glossary will keep this proposal unambiguous. It also serves as a list of all [user roles] and [resources](#resources). The latter is also based on the Greg's pull request.

[Greg Wilson's pull request]: https://github.com/swcarpentry/board/pull/81/files
[user roles]: people-or-user-roles
[description of the current workflow]: #the-current-workflow
[list of issues]: #issues

Here is [#89](https://github.com/numfocus/gsoc/issues/89) issue where I discussed this proposal. 

I believe it's really important to get feedback as soon as possible, therefore:

- Before introducing new features to all users, they'll be tested by a group of volunteering early users. That way, we don't have to test everything before showing it to first users, so we can get feedback faster and iterate quicker. If we don't get those early users, the only thing we can do is:
    - to extensively test the code for different user scenarios before exposing new functionality to all users,
    - to introduce the new system only to SWC Admins, Trainers and Trainees who serve as "early users", and then to all users.

- The early users can give feedback in an easy way, e.g. by sending an email rather than opening an issue on github.

- UI mockups will be presented and accepted by the early users.

- Amy dev instance will be available to all interested parties.

It's also important to note what will be **not** implemented:

- Advanced security control. There'll be only one flag in `Person` model that indicates if somebody is a Trainee with very limited permissions or somebody else (Admin, Trainer etc.) who can can edit everything.

- Responsive interface.

- Tracking sessions with Google Analytics or Piwik.

## New Workflow

1. **Recruiting Candidates**

    + A Candidate or a Lead Candidate of a group of Candidates uses Registration Form to create his account.

        + All Candidates will be represented in `Person` table, from the beginning of the workflow. For simplicity, users can login using their emails, so they don't have to choose username. Every Amy user can login using their email address or username (if they have an username).

        + This form contains only one field: email address. After submitting the form, Amy sends an email with link to another form, where the Candidate can choose password and enter password and username.
        
        + The Registration Form accepts emails addresses that are already present in Amy Database. 

            + If the user hasn't chosen a username yet (that happens for people who were registered by someone else), he's forwarded to the same form. 
        
            + If the user has an username, she's forwarded to Password Recovery Form.

    + The Candidate logs into Amy and uses Application Form to apply for an Instructor Training. This form is used for both individual Candidates as well as groups of Candidates.
        
        + In the case of group applications, the Lead Candidate needs to enter names and email addresses of other Candidates.

        + After submitting the form, a new account is created for each Candidate, but no password and no username are set yet. Amy detects when somebody is already present in the database (matching by email address). In that case, no record is created.

        + For simplicity, UI allows the Candidate to request only one Instructor Training.

    + The Lead Candidate can later log into Amy and edit his application using the same view.

1. **Matching Candidates to Courses**

    + Admins can view all applications along with candidates in Amy. They can also merge groups and individuals to form bigger groups. They manually match Candidates to Instructor Trainings.

        + Admins can view all applications in Applications list view.

        + Admins can view one application (along with all candidates) in Applications detailed view.

        + Admins can associate each Application with one Instructor Training. That way, they can merge groups by associating two or more Applications to one Instructor Training.

        + Applications list view let you sort applications by distance to chosen application. That way, you can easily see which groups are worth merging.

        + Admins can split any group in Applications detailed view. This works by creating a clone Application with different candidates set.

    + Admins negotiate date and place of Instructor Training with Lead Candidate via email.

    + In the end of negotiation of Instructor Training date, Admins send emails to all Candidates to confirm they want to participate in the Instructor Training.

1. **Running Courses**

    + An Instructor Training take place.

    + At Instructor Training (or after that training), all Trainees already have accounts in Amy. They can set up their passwords and usernames using Registration Form.

    + The Trainer uses Trainees list view and filters all Trainees who are not assigned to any Instructor Training. Then, he assigns his Trainees to his Instructor Training.

    + The Amy interface for Trainees will be completely different from the current one. Here is what Trainees can see after logging in:
    ![checkout-process-progress](https://cloud.githubusercontent.com/assets/1645996/13904004/afa256cc-ee91-11e5-8829-090afd95f604.png)

1. **Homework**

    + Trainees make a change in the SWC Lessons and paste the link to their pull requests into Amy. In the case of DC, they paste link to anything that describes their Lesson Change.

    + In the case of SWC, Amy analyzes the link to decide which core lesson did the trainee pick up. In the case of DC, Trainee has to manually select core lesson.

1. **Discussion Session**

    + After entering the link, the Trainee uses Amy to register for one Discussion Session.

        + In order not to overload Lesson Maintainers, they don't have to indicate in Amy if the pull request is good enough. It also means that trainees don't have to wait for the feedback from lesson maintainers, so they can register for discussion session immediately. This may raise up the percent of trainees who bother up to go through rest of checkout process.

        + By default, we assume that pull requests are good enough. However, Lesson Maintainer can indicate that the submission is not good enough in Trainees list view.

        + The Trainee can indicate that she's not available at any of the slots. She can tell when she's available in a text area. The Head of Training can list all trainees (in Trainees list view) who requested additional sessions along with whatever they entered in the text area.

        + Amy tracks which Trainees attended which Sessions. That is, there would be many-to-many relation between Trainees and Discussion Sessions.

    + After the Discussion Session, the Discussion Leader uses the detailed view of Discussion Session shown below to indicate who has passed and who failed. After that, Amy mails the trainees with instructions on what to do next. This view is also displayed when a Trainer or an Admin wants to create a new session.
    ![discussion-session-detailed-v2](https://cloud.githubusercontent.com/assets/1645996/13904131/8979568a-ee96-11e5-8f4c-068f256ac184.png)

    + This view lets the Discussion Leader customize emails:
    ![discussion-session-detailed-v2-mail](https://cloud.githubusercontent.com/assets/1645996/13904138/b115dd9e-ee96-11e5-8adf-7a1a383ed9a8.png)

    + The emails Amy sends are send in the name of the Trainer, so if somebody replies, the response will land in the Trainer mailbox.

1. **Checkout Session**

    + At this moment, trainees can register for Checkout Session.

    + After the Checkout Session, the Examiner indicates in Amy who has passed and who has failed (the interface would be similar to the detailed view of Discussion Session). Amy mails the Trainees with instructions on what to do next.

        + Note that Admins and Head of Training are not engaged at all.

1. **Welcoming to the Community**

    + Amy generates PDF certificates. Those certificates are hosted as static files on Amy (much easier than integrating Amy with github).

    + New instructors send their biography as well as photo in Amy; and are asked to join `discuss` as well as `instructors` list. After sending it, they can download their certificate from Amy.

## Schema Changes

```python

class SWCOrDCField(models.CharField):
    max_length = STR_MED
    choices = (
        ('swc', 'Software-Carpentry'),
        ('dc', 'Data-Carpentry'),
    )
    blank = False

# modification to Person class:
class Person(AbstractBaseUser, PermissionsMixin):
    # determines whether the person has access to AMY interface for admins and trainers
    is_admin = models.BooleanField(default=True)  

    # username -- no longer required
    # email -- required

    instructor_training = models.ForeignKey('InstructorTraining', null=True)
    swc_core_lesson = models.ForeignKey('Lesson', null=True, related_name='trainees',
        limit_choices_to=Q(suitable_for_training=True))
    dc_core_lesson = models.ForeignKey('Lesson', null=True, related_name='trainees',
        limit_choices_to=Q(suitable_for_training=True))
    swc_cannot_make_discussion_session = models.BooleanField(default=False)
    dc_cannot_make_discussion_session = models.BooleanField(default=False)
    cannot_make_checkout_session = models.BooleanField(default=False)
    availability = models.TextField(default="", blank=True)
    link_to_swc_lesson_change = models.URLField(default="", blank=True)
    link_to_dc_lesson_change = models.URLField(default="", blank=True)
    is_swc_lesson_change_good_enough = models.BooleanField(default=True)
    is_dc_lesson_change_good_enough = models.BooleanField(default=True)
    is_certified_instructor = models.BooleanField(default=False)
    biography = models.TextField(default="", blank=True)
    photo = models.FileField()

# The source of truth for Discussion Sessions and Checkout Sessions is Amy
# database. However, we can later switch to get them from Google Calendar. In
# that case, we need to periodically synchronize Amy database with the
# calendar.
class Session(models.Model):
    leader = models.ForeignKey('Person')
    start = models.DateTimeField()
    status = models.CharField(
        max_length=STR_MED,
        choices=(
            ('not-started', 'Did not started yet'),
            ('canceled', 'Canceled'),
            ('took-place', 'Took place')
        ),
        blank=False, default='not-started',
    )
    notes = models.TextField(default="", blank=True)

    class Meta:
        abstract = True

class DiscussionSession(Session):
    carpentry = SWCOrDCField()
    trainees = models.ManyToManyField(Person, 
        through='DiscussionSessionAttendance', 
        through_fields=('session', 'person'),
        related_name="discussion_sessions")

class CheckoutSession(Session):
    trainees = models.ManyToManyField(Person, 
        through='CheckoutSessionAttendance', 
        through_fields=('session', 'person'),
        related_name="checkout_sessions")

class SessionAttendance(models.Model):
    person = models.ForeignKey('Person')
    status = models.CharField(
        max_length=STR_MED,
        choices=(
            ('na', 'NA'),  # when the session hasn't happened yet
            ('absent', 'Absent'),
            ('failed', 'Failed'),
            ('passed', 'Passed'),
        ),
        blank=False, default='na',
    )
    mail_sent = models.BooleanField(default=False)

    class Meta:
        abstract = True

class DiscussionSessionAttendance(SessionAttendance):
    session = models.ForeignKey('DiscussionSession')

class CheckoutSessionAttendance(SessionAttendance):
    session = models.ForeignKey('CheckoutSession')

class InstructorTrainingApplication(models.Model):
    lead_candidate = models.ForeignKey(Person)
    # the lead_candidate is always present in candidates set
    candidates = models.ManyToManyField(Person, related_name='applications')
    reason = models.TextField(default="", blank=True) # why they want to become an instructor
    training = models.ForeignKey(InstructorTraining, null=True)

class InstructorTraining(models.Model):
    date = models.DateTimeField()
    country = CountryField()
    location = models.CharField(max_length=STR_LONG,
                                help_text='City, Province, or State')
    instructor = models.ForeignKey(Person)

class Lesson(models.Model):
    # `name` field is already present
    suitable_for_training = models.BooleanField(default=False)
    carpentry = SWCOrDCField()

```

## Milestones

I split this proposal into as small milestones as possible. Every milestone ends up with new version of Amy being pushed to production and introduced to all its users. This minimizes the chances of failure and let us adapt or change almost everything after every milestone -- even if it means changing the scope of this project. The milestones are:

- **Milestone 0: Migrating from SQLite to PostgreSQL**

    I'm discussing in [#716](https://github.com/swcarpentry/amy/issues/716) with Piotr Banaszkiewicz if it's really necessary, but if it is, it must be done at the beginning, as an additional "zero" milestone.

- **Milestone 1: Managing Final Demonstration Sessions** and welcoming to the community

- **Milestone 2: Managing Rest of Checkout Process** -- that is everything after an Instructor Training: Homework and Discussion Session.

- **Milestone 3: Managing Recruiting Candidates** and rest of the workflow -- that is managing everything before and during an Instructor Training: Recruiting Candidates, Matching them to Instructor Trainings and running those trainings.

## Schedule

### Community Bonding Period

- Fixing small issues to familiarize with AMY codebase.
- Choosing early users among Admins, Trainers and Trainees.
- Discussing and improving schema changes (it clearly demands improvements and more considered design).

### Milestone 1: Managing Final Demonstration Sessions

* **Week 1: May 23 - May 29**

    - Doing UI mockups. Early users give feedback on the UI.
    - Doing necessary schema changes.

* **Week 2: May 30 - June 5**

    - Implementing Checkout Session detailed view.
    - Implementing Checkout Session list view.
    - Implementing Trainees list view.
    - Implementing views of progress of Checkout Process for Trainees (one for SWC and one for DC).

* **Week 3: June 6 - June 12**

    - Implementing Registration Form and the form to set password and username.
    - Implementing Recover Password Form.
    - Setting up Amy, so it can send emails, in the name of other people (i.e. in the name of Trainers). Using `django.core.mail`.
    - Manual testing.
    - Importing existing data about early users to Amy. The data are in:
        - Checkout Registration Etherpad
        - Trainees Spreadsheet
        - Certification Repository

* **Week 4: June 13 - June 19**

    - Exposing new functionality to early users.
        - Final Demonstration Sessions for early users will be different from those sessions available for "old" users, so we don't have to synchronize sessions between Etherpad and Amy.
    - Responding to feedback and improving Amy.
    - Writing unit tests. 
        - Especially security tests to ensure that only the right people can see certain information.
        - Especially tests for the case when one trainee wants to become certified Instructor of both Carpentries.
    - Doing manual load tests to ensure that the system will keep working after introducing it to all users.

* **Week 5: June 20 - June 26 (midterm evaluation deadline)**

    - Importing existing data, so e.g. people who already attended Discussion Session are also included in the new system.
    - Exposing new functionality to all users.
    - Teaching all Trainers and Admins the new workflow.
    - Editing appropriate web pages and github repos to document the new workflow.
    - Responding to feedback from all users.

### Milestone 2: Managing Rest of Checkout Process

* **Week 6: June 27 - July 3**

    - Doing UI mockups. Early users give feedback on the UI.
    - Doing necessary schema changes.
    - Implementing Discussion Session detailed view.
    - Implementing Discussion Session list view.
    - Extending the views of progress of Checkout Process for Trainees.
    - Manual testing.
    - Importing existing data about early users to Amy. The data are in:
        - Discussion Registration Etherpad
        - Trainees Spreadsheet

* **Week 7: July 4 - July 10**

    - Exposing new functionality to early users.
        - Discussion Sessions for early users will be different from those sessions available for "old" users, so we don't have to synchronize sessions between Etherpad and Amy.
    - Responding to feedback and improving Amy.
    - Writing unit tests. 
    - Doing manual load tests to ensure that the system will keep working after introducing it to all users.
    - Importing existing data, so e.g. people who already attended Instructor Training are also included in the new system.

* **Week 8: July 11 - July 17**

    - Exposing new functionality to all users.
    - Teaching all Trainers and Admins the new workflow.
    - Editing appropriate web pages and github repos to document the new workflow.
    - Responding to feedback from all users.

### Milestone 3: Managing Recruiting Candidates

* **Week 9: July 18 - July 24**

    - Doing UI mockups. Early users give feedback on the UI.
    - Implementing Application Form.
    - Implementing filtering by Instructor Training in Trainees list view.
    - Implementing Application list view.
    - Implementing Application detailed view.
    - Doing necessary schema changes.

* **Week 10: July 25 - July 31**

    - Importing data (only related to early users). The data are in:
        - Individual Application Spreadsheet
        - Group Application Spreadsheet
        - Partners Spreadsheet
    - Manual testing.
    - Exposing new functionality to early users.

* **Week 11: August 1 - August 7**

    - Writing unit tests.
    - Responding to feedback and improving Amy.

* **Week 12: August 8 - August 14**

    - Importing all data to Amy.
    - Ensure that email addresses of Amy users are unique.
    - Exposing new functionality to all users.
    - Teaching all Trainers and Admins the new workflow.
    - Editing appropriate web pages and github repos to document the new workflow.
    - Responding to feedback from all users.

* **Week 13: August 15 - August 22 (final evaluation)**
    
    - Buffer week.
    - Week for implementing additional nice-to-have features listed below.

### Backlog

If time allows, I'd like to implement nice-to-have features like (in no particular order):

- harvesting information whether a Lesson Change is good enough from GitHub by looking at tags attached to Trainees' pull requests,

- sending emails to Trainees when new Discussion Sessions and Checkout Sessions are available,

- also notifying Discussion Leaders when Trainees want sessions of the kind they're able to offer,

- tracking whether groups (who attended instructor workshop) run their promised workshops or not,

- letting Candidates see other groups that are close to them and merge with them,

- allowing logging using Google account.

## Experience

I've written [a lot of patches] to Django (my nick is `chrismedrela` and, earlier, `krzysiumed`). This ended up in successful revamping of Django check framework during GSoC 2013. Here is [the merge commit]. During GSoC 2014 I worked at Breeze, a library for numerical processing in Scala (there is no single merge commit so I don't share any link). I've submitted this [tiny pull request] to Amy.

In 2015, I got commercial experience. I worked for a foreign client in Codibly (a software house) and I was developing distributed big data system for real time sport betting in Python. I also had a very short story with a startup in Haskell from SFX industry.

[a lot of patches]: https://code.djangoproject.com/query?owner=~krzysiumed&or&owner=~chrismedrela&col=id&col=summary&col=status&col=owner&col=type&col=component&col=version&desc=1&order=id
[the merge commit]: https://github.com/django/django/pull/2181
[tiny pull request]: https://github.com/swcarpentry/amy/pull/722

I'm a 4th year student of Automatics Control and Robotics at University of Science and Technology in Kraków/Poland.

## Why this project?

For a long time, I tutored people. It started about 5 years ago and it was about math and physics. I quickly switched to teaching programming. For a long time, I treated it as a side project. I even have [an account on codementor.io]. Last year I realized that 1-to-1 tutoring is not efficient, so I wanted to try workshops. My friend from uni, Piotr Banaszkiewicz, recommended SWC to me. So I attended Instructor Training in Kraków in the end of 2015. I've passed Final Demonstration Lesson two weeks ago (so I'm familiar with the workflow :)). Until now, I was an instructor at two workshops and I was a helper before.

Having some commercial experience I know, that it's much more important for me to work on something that is actually shipped, used, something that matters rather hyper-new technologies used in an interesting-sounding project with lack of users and feedback.

I also know Piotr Banaszkiewicz in person and I had possibility to meet Greg Wilson on Instructor Training, so I know who I will work with.

I also talked with Piotr Banaszkiewicz, so I know how it is to work on Amy as dev (Piotr really recommends it). I also believe that there are less communication issues when you can talk face-to-face with other contributors.

Usage of technologies that I know (Python, Django) is also an important factor for me.

I'm not applying for any other GSoC project.

[an account on codementor.io]: http://codementor.io/chris.medrela

# Appendix

Contact information: Christopher Medrela, `chris.medrela+softwarecarpentry [at] gmail.com`. My time zone is UTC+01:00.

## Glossary

### People or User Roles

One person may have multiple roles.

- **Instructor Trainer**, **Trainer**: someone appointed by the Software Carpentry Steering Committee to lead a Instructor Training event. There are only a few of these. Also, they volunteer to lead checkout sessions and in that case they are called **examiners**. However, we're moving to a system where the examiner *isn't* the person who ran the training course (i.e., if I train you, someone else examines you).
- **Mentor**, **Discussion Leader**: an experienced Software Carpentry or Data Carpentry instructor who has volunteered to lead some hour-long discussion sessions.  There are more of these than trainers.
- **Head of Training**, **Lead Trainer** - one person; at this moment it's Greg Wilson; he's emailed by trainees when they submit their Lesson Change; also recruit discussion leaders; also resolves discussion session scheduling issues (like when trainees cannot attend any of the available slots)
- **Trainee**: anyone that attended an Instructor Training.
- **Candidate**: anyone that want to attend an Instructor Training.
- **Instructor**: a person teaching SWC/DC workshops
- **Certified Instructor**: an Instructor who passed checkout process, that is someone who attended Instructor Training, discussion session and checkout session and passed both sessions.
- **Lead instructor** - an Instructor who leads group of instructors making a workshop. Must be a Certified Instructor.
- **Administrator**, **Admin**
- **Partner**: is an organization that has made a long-term commitment to the growth and spread of Software Carpentry. Partners may associate individual applicants.
- **Lesson Maintainer**
- **Learners**: people who attend regular SWC/DC workshop to learn programming skills
- **Helpers**
- **Host**
- **Executive Director**
- **Member of Steering Committee**

### Resources

- **Discussion Registration Etherpad**: Etherpad to register for lesson discussion session.
- **Checkout Registration Etherpad**: Etherpad to register for demo sessions and take notes during those sessions.
- **Trainees Spreadsheet**: Google Docs Spreadsheet with list of trainees. The columns are:
    - name
    - contact email
    - which training course they took part in
    - the name of the Software Carpentry lesson they're concentrating on (if any)
    - the name of the Data Carpentry lesson they're concentrating on (if any)
    - the URL of their Software Carpentry exercise submission (if any)
    - whether or not they have submitted something for Data Carpentry
    - the date and moderator of the discussion session they have taken part in (we currently store both values in one column)
    - the date on which they passed their Software Carpentry lesson demo (if any)(this date becomes the award date of the PDF certificate)
    - the date on which they passed their Data Carpentry lesson demo (if any)
- **Partners Spreadsheet**: Google Docs Spreadsheet with negotiations with partners about instructor training sessions.
- **Individual Application Form** -- a form in Amy for Candidates to apply as an individual. Temporarily this form is disabled.
- **Individual Application Spreadsheet** -- a Google spreadsheet for individual applications. The columns are:
    - name
    - contact address
    - location
    - why they want to become an instructor
- **Group Application Form** and **Group Application Spreadsheet**: Google web form and a spreadsheet for group applications (should be moved to Amy as workshop-style events). The Group Application Spreadsheet contains:
    - all names and email addresses (principal contact highlighted)
    - group location
    - description of what the group plan to do once they're trained
- **Calendar**: Software Carpentry's community calendar (a Google calendar).
- **New Instructor Form**: [Form on AMY](https://amy.software-carpentry.org/workshops/update_profile/) for people who passed exam.
- **Database**, **Amy Database**: Amy database with list of certified instructors.
- **SWC Lesson Changes**: GitHub pull requests to SWC Lessons. First step of Checkout Process.
- **DC Lesson Changes Doc**: Google Docs page for Data Carpentry. First step of Checkout Process.
- **SWC/DC Lessons**: SWC/DC lessons available on SWC website.
- **[Certification Repository](https://github.com/swcarpentry/certification)**: contains generated .pdf certificates for both SWC and DC Instructors

### Other

- **Instructor Training**, **Course**
- **Checkout Process**, **Checkout**: process of becoming Certified Instructor that takes place after Instructor Training and, at this moment, consists of three steps: 1) Homework 2) Discussion Session 3) Checkout Session
- **Homework**: Lesson Changes; first step of Checkout Process.
- **Pre-Checkout discussion session**, **Discussion Session**, **Discussion**: meeting between an Instructor Trainer and some trainees to discuss one of Software or Data Carpentry lessons.
- **Checkout Session**, **Final Demonstration Session**: meeting between a Instructor Trainer and a trainee to conclude Instructor Training.
- **Carpentry** -- either SWC or DC.
- **Application** -- individual or group application for Instructor Training

## The Current Workflow

1. **Recruiting Candidates** -- one of three ways:

    - A candidate applies directly as individual by sending Individual Application Form. Temporarily there is no way to apply as individual, because the form is disabled.

    - A group of candidates apply in physically co-located groups using Group Application Form.
        - It happened only once, but it should be a routine.
        - Size of any group must be 3-12 people.

    - A partner finds 3-12 candidates and requires Instructor Training as part of partnership agreements.

1. **Matching Candidates to Instruction Trainings**.

    1. The Head of Training, Members of the Steering Committee and/or the Executive Director:

        - looks to Group Application Spreadsheet and Individual Application Spreadsheet
        - manually decides on dates
        - invite individuals and/or groups to take part in an Instructor Training.

1. **Running Instruction Training** -- one of three ways:

    1. The Trainer and the Trainee take part in:
        1) the in-person Instruction Training that takes two days or
        2) live online Instruction Training that also takes two days or
        3) asynchronous online course run over several weeks; this format is phased out.

1. **Homework**

    1. The Trainee picks one core lesson from SWC or DC or both (depending whether they want to SWC or DC Certified Instructor).
    1. If the Trainee wants to become SWC Certified Instructor, the Trainee submit pull requests with a small change to Software Carpentry Lessons.
    1. If the Trainee wants to become DC Certified Instructor, then the Trainee proposes a change to DC Lessons in DC Lesson Changes Doc.
    1. The Trainee emails the Head of Training with the link to the Lesson Change.
    1. The Head of Training updates Trainees Spreadsheet.
    1. The Head of Training mails the Trainee instructions on how to sign up for a discussion session on the lesson discussion Etherpad.
    1. The SWC Lesson Maintainers are supposed to merge the pull requests, but currently the load on them is unsustainable.

1. **Discussion Session**

    1. The Trainee registers for Discussion Session on Discussion Registration Etherpad. The Session must be about the core lesson that the trainee picked up in the previous step (Homework).
    1. The Trainees and Mentor take part in Discussion Session.
    1. The Mentor emails the Head of Training.
    1. The Head of Training updates Trainees Spreadsheet: column "the date and moderator of the discussion session they have taken part in".
    1. The Head of Training mails Trainees who passed Discussion Session telling them to register for Checkout Session.

1. **Checkout Session**

    1. The Trainee registers for Checkout Session on Checkout Registration Etherpad.
    1. The Trainees and Mentor take part in Checkout Session. They take notes in the Checkout Registration Etherpad.

1. **Welcoming to the Community**

    1. An Admin generates PDF certificate using command-line script in [the certification repository](https://github.com/swcarpentry/certification).
    1. The Admin mails the trainees with instructions to:
        - join the `discuss` and `instructors` mailing lists
        - fill in New Instructor Form in Amy to tell us where the trainee is, what he's comfortable teaching, etc., and
        - send us a short biography and photo to add to our website.
    1. The Trainee replies to the Admin.
    1. The Admin edits the biography and the photo (this must be done manually since they always require formatting) and publish it to the website.
    1. The Admin welcomes bunch of Trainees on the blog.

## Issues

1. **Issues with the current workflow, that is issues that cannot be solved by just porting to Amy:**

    1.  Only 2/3 people who attended instructor workshop become instructors.

        - Solution: Investigate why people get stuck: Change the workflow to follow up people who don't bother to go through discussion and exam and ask them why it is like that.

        - Post-facto response rates to surveys are usually terrible, so we need a little leadership (that is pings and accountability to a real human) to motivate people.

    1.  Online courses have poorer follow-through rates.

        - Those courses will be phased out, so we don't need to solve it.

    1.  Load on lessons maintainers is unsustainable.

    1.  There are too much new exercises and changes to content of lessons.

        -   Although some people disagree, e.g. Aleksandra Pawlik:

            > What I observe if I try to use these challenges is that people check out.. In particular, the more advanced students lose interest. With hands-on exercises you can always have additional features which people need to do - this keeps advanced busy while beginners still are completing the basic task.
            > The disconnect in the exercises with the sample dataset used for lessons gets people confused.
            > Almost every single workshop in the feedback people ask for more hands-on exercises, related to the sample dataset, even to do them when they go home.

    1.  No blog post welcoming new instructors haven't been done in a while due to lack of time.

    1.  Lack of consistency about whether the person mentioned on certifications is the instructor (the person who ran their training course) or the person who oversaw their final demo.

    1.  De-personalize the nays from discussion leads. (requested by `kariniag`)

        -   Solved by `BillMills`:

            > This is creating a lot of overhead for Greg and Jonah that I think is a bit overcomplicated. I totally feel you that saying no to students is uncomfortable - but I think this is just a framing problem. Don't think of it as 'saying no', but rather as advice on how to improve. In no circumstances would I ever tell someone 'no, I don't think you're ready to be an instructor' - instead I would say 'I really liked X about your performance today, but next week I'd like to see Y; do that and you will be an absolutely awesome instructor! :D :D :D'

1. **Issues caused by the current solution, that is, using Google Docs and Etherpads instead of Amy:**

    1. Hard to prevent mistaken overwrites/deletions/etc.

    1. Hard to track changes (who's completed what since we last looked)

        - E.g. figuring out which people in the queue for instructor training have been through workshops.

    1. Information is scattered across too many places and it's hard to get an overview of what's going on.

    1. Manual copying and synchronizing of information makes load on trainers unsustainable.

        - E.g. synchronizing identities between Group Application Spreadsheet and AMY.

    1. Searching for updates makes load on trainers unsustainable.

    1. Using one Etherpad both to manage sign-up for demo sessions and to take notes during those sessions is confusing.

        - Solution: one Etherpad for registration and one reused Etherpad for each Discussion Leader.

1. **Nice-to-have features**

    1. A form for an individual Candidate to apply for Instructor Training.

        - There is such form in Amy, but it's disabled.
        - The application must include names and contact information for everyone who intends to take part.
        - Candidates must also commit to organizing and running at least one workshop within a few months of training.
        - Could be similar to [the form used to request SWC Workshop](https://amy.software-carpentry.org/workshops/swc/request/)
        - No need for username/password -- the Candidate doesn't need to log in until he's accepted.

    1. A form for group of candidates to apply for Instructor Training.

        - There is such form in Google Forms.

    1. Tracking stuff like gender and race of Amy wanna-be instructors.

    1. Determining whether individual applicants are associated with our partners. (The applicants may not know that their organization is a partner.)

        - Solution: Add drop-down or link to list of members in the application webform for instructors.

    1. Helping individual training applicants find one another to form groups, so they can apply in physically co-located groups. At this moment, they are manually clustered by their nearest airport code.

    1. Importing group applications to Amy ([#419](https://github.com/swcarpentry/amy/issues/419#issuecomment-195781588))

    1. Notifying people in the registration for discussion sessions:

        - Trainees are not notified when new sessions of the kind they want appear.
        - Discussion leaders are not notified when trainees want sessions of the kind they're able to offer.
        - Admins are not notified if session have taken place and who got through etc.

    1. Tracking both a Software Carpentry and a Data Carpentry discussion session, if the participants were present and if they passed, separately for each Carpentry.

    1. Tracking the name of instructor and person who did final test. Necessary to include the instructor (who did the most of work) on certificates.

    1. Systematically tracking whether groups (who attended instructor workshop) run their promised workshops or not.

    1. Making complex queries like "instructors with a badge within D miles of [airport code] who have taught < N workshops in the last M months who are comfortable teaching topic X". Or at least be able to do github-like queries.

        - `gvwilson` thinks that the search-for-instructors functionality in Amy right now serves admins' needs,
        - searching instructor by airport is an acceptable workaround for `BillMills`.

    1. Tracking sessions with Google Analytics or Piwik. That'd allow us to know:

        - what features are most used;
        - if a sudden upgrade makes angry 1 person or 20 people.

    1. Migrating from SQLite to PostgreSQL (see issue [#716])

[#716]: https://github.com/swcarpentry/amy/issues/716