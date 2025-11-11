# Manage Workflow for Instructor Training

## Abstract

Since 1998, Software Carpentry has been teaching researchers in science, engineering, medicine, and related disciplines the computing skills they need to get more done in less time and with less pain.
Software Carpentary, uses AMY (a Django based web application) to keep track of who's qualified to teach, what workshops have run, who taught and attended. As more and more people are applying to become instructors at Software  Carpentary. It has become essential to manage the workflow for instructor training.

This project will design such a workflow and implement it by adding appropriate views to AMY..

## Proposed workflow

![HTML MOCKUP flowchart](https://cloud.githubusercontent.com/assets/15982349/13868778/90f99dc0-ecf2-11e5-9ac3-8cfd3dec3224.png)

1. Interested Trainees apply as Group or Individuals by filling forms.
![HTML MOCKUP FORMS](https://cloud.githubusercontent.com/assets/15982349/13816694/6c7a68e6-ebb5-11e5-809e-eb5bd00d7a22.png)
2. All applications are listed in the *ALL_APPLICATIONS* view. In this view the lead trainer can accept or reject an application.
Prior to accepting an application, the lead trainer has to create an *Event* for the training session, using the already implemented "Create Event" feature of AMY.
While accepting the application the lead trainer would just have to select one of already created events from the drop-down provided.
Clicking on the accept button would send an email to the applicant informing them that he/she has been accepted into the program along with information such as date, location ( this field would default to an empty string if mode is online), mode (online or in person), instructor(s) name and instructor(s) email id.
The email would also have one randomly generated password for the trainee to login in to their account (this password can be changed to anything by the trainee after the first login).
Every trainee gets a login credential, even if they belong to a group.
![HTML MOCKUP ALL_APPLICATIONS]( https://cloud.githubusercontent.com/assets/15982349/14044626/b3ae9eee-f2b8-11e5-92fc-2850b4954319.png)

3. All accepted applications, are then transferred to the *Trainee view*. In this view the lead trainer, trainers,discussion leaders, lesson maintainers can track progress of the trainees, provide feedback and update the status of every stage. They are also notified here if the Trainee has requested a discussion/checkout session.
The Feedback/Comments are public and will be visible to everyone at the administrative end - lead instructor/instructors/discussion leaders/lesson maintainers.
These Feedback/Comments would be useful for archiving, so that later on anyone would know, why a particular trainee was passed or failed at a particular stage in instructor training and by whom.
We would be using [disqus](https://www.djangopackages.com/packages/p/django-disqus/) to implement the commenting/feedback system.
Clicking on a trainee's name or the information glyph-icon would direct us to a *Trainee Details* page, where the lead instructor/instructors/discussion leaders/lesson maintainers can view all the information about the trainee (information from when the trainee filled out the application form).In this view the lead instructor/instructors/discussion leaders/lesson maintainers can also write feedback and update status for every stage.
This list can be filtered on the following criteria:
  * instructor assigned.
  * start date of training.
  * mode of training (online or in person)
  * location of the training.(mode - in person)
![HTML MOCKUP ALL_TRAINEE](https://cloud.githubusercontent.com/assets/15982349/14044715/9f25e918-f2b9-11e5-8199-e37bec408e4d.png)

4. *Sessions Tab*- One more tab would be added to AMY nav-bar titled *SESSIONS*. This view would list down all the sessions available with status - completed, ongoing, not yet started.
The instructor can view a list of all the trainees who have registered for a particular session by clicking on the session's name or the information glyph-icon which would direct them to a *Sessions Details* page.
The *Sessions Details* page would also contain other information such as type of session ( discussion or checkout),and session leader (discussion leader or trainer heading checkout session) etc.Clicking on any of the trainees, would direct us to the *Trainee Details* page.In the Sessions tab the lead instructor/instructors/discussion leaders/lesson maintainers can also create a session by clicking on the *create sessions* tab and filling a web-form.

5. *Trainee Home page* -
After getting accepted for the training program, the trainees receive a randomly generated password, which they can use to log in to AMY, a trainee's home page is different from the AMY DASHBOARD, the AMY DASHBOARD is reserved for administrative purposes only. After logging in the trainees are directed to TRAINEE_HOME. This view will have a nav-bar - **HOME**, **ABOUT**, **NOTIFICATION**, **SESSIONS** and **MESSAGES**.
  * *HOME*  shows the trainees progress, through a progress bar indicating which stages have been completed and the ones left. This view will also list down the general guidelines for instructor training, informing what is required in each stage of the training.

  * *ABOUT* , this view shows the information entered by the trainee in their initial application, they can also edit this information, and change passwords here.
  * *NOTIFICATION*,  this view allows the trainee to view all the comments/feedback from the discussion leaders and the lesson maintainers.In the format *comment: by whom: timestamp*.
  * *SESSIONS*, this view lists down all the sessions (Discussion and Checkout Sessions) available.The trainee can register for any session in this view by clicking on the *Register* button. In this view the trainee can also request a discussion session/checkout session by filling in a web-form, with fields such as TOPIC, PREFERRED TIME, and so on.If a trainee did not attend a registered Session, they can just register for another one, although their last missed session would be marked as absent.
  * *MESSAGES*, In this view the trainee can talk to any of the Instructors/Discussion Leaders/Lesson Maintainers.We will be using [ django-postman](http://django-postman.readthedocs.org/en/latest/quickstart.html) for this.These are private messages.


## Technical Details
The Software stack for this project can be divided into:

 * Backend  : [Django](https://www.djangoproject.com/)
 * Database : [SQLite](https://www.sqlite.org/)
 * Frontend : [Bootstrap](http://getbootstrap.com/),[JQuery](https://jquery.com/) for AJAX requests

### Features and Model Schemes

Trainee would be the main Model, maintaining most of the Flags for Progress and other information:

```python
class Trainee(models.Model):
    '''Model for a Trainee '''
    # listing just essential fields, more fields can and should be added when implementing
    trainee = models.ForeignKey(Person,default=None)
    group   = models.models.ForeignKey(Trainee_Group,default=None,null=True)
    training_event= models.ForeignKey(Event) # we also need to maintain individual training_event for querying and when the trainee does not apply in a group.
    session_registered=models.ForeignKey(session_regis,default=None,null=True)
    Discussion_Flag = models.BooleanField(default=False) # This flag becomes true if the trainee passes any of the discussion sessions.
    Checkout_Flag = models.BooleanField(default=False) # This flag becomes true if the trainee passes any of the checkout sessions.
    HomeWork_Flag = models.BooleanField(default=False) # This flag becomes true if any of the homework submitted is accepted by the lesson maintainers.

 ```

Maintaining Group information:

```python
''' all trainees belonging to the same group will have same Trainee_Group as a ForeignKey, as ForeignKey maintains a Many to One relationship we can easily know the trainees belong to a particular group.'''
class Trainee_Group(models.Model):
  ''' maintains group information '''

    training_event= models.ForeignKey(Event)
    primary_contact = models.ForeignKey(Person)

```
Sessions:

```python

class Sessions(models.Model):
  #listing down only a few important fields
  DISCUSS='DS'
  CHECKOUT='CS'
  SESSION_CHOICES = (
        ('DS', 'Discussion Session'),
        ('CS', 'Checkout Session')
    )
  session_type = models.CharField(max_length=STR_LONG, choices=SESSION_CHOICES, null=False, default=DISCUSS)
  instructor=models.ForeignKey(Person)
  start = models.DateField(null=True, blank=True)#start date
    end = models.DateField(null=True, blank=True)# end date

```
Managing Registered Sessions:

```python

class session_regis(models.Model):
  session= models.ForeignKey(Sessions);
  session_attendance=models.BooleanField(default=False)
  #False indicates that the trainee registered but didnot attend the session.
  #True indicates that the trainee registered and attended the session.
  success = models.BooleanField(default=False)
  #indicates if the trainee successfully passed this particular session (discussion or checkout).

```
Managing Home work:

```python

  class HomeWork(models.Model):
    trainee=models.ForeignKey(Trainee)
    link=models.URLField(max_length=200)
    success = models.BooleanField(default=False)
    #indicates if the trainee successfully passed this particular submitted homework.


```


## Schedule of Deliverables

I believe in test driven development so I will be writing tests as soon as I am done with a feature/view addition.

### Community Bonding Period

I would utilize this time to interact with the Software Carpentary Community and also familiarize myself with the code base of AMY.
I would also like to discuss with the mentors:
 *  What information is required from the applicants, so can that I can design the form fields.
 * What additional features would they like to have in AMY.

### May 25th -  June 7th

**MILESTONE 1** -
* Design the Form for Individual and Group applications
* Add the *ALL_APPLICATIONS* view in AMY.
* Add the *TRAINEE* view in AMY

### June 8th - June 21th

**MILESTONE 2** -
* Enable filtering options in *ALL_APPLICATIONS* and *TRAINEE* views, so that the lead trainer can search and filter applications.
* Add the  *Session* view in AMY
* Write Tests
* Expectation - the trainer should be able to accept trainee applications.

### June 22nd - July 5th

* **Mid term evaluation**
*  Implement the Commenting System.
*  Implement the Messaging System.
### July 6th - July 19th

**MILESTONE 3** -

* Messaging System continued.
* Commenting System continued.
* Progress Tracking System.
    Discussion Leaders and lesson maintainers, should be able to comment and evaluate the performance of the trainees.
* Enable login for trainees and Design the *TRAINEE_HOME* page.

### July 20th - August 2nd
**MILESTONE 4** -
* Progress Tracking System continued.
* Implement request session feature for trainees.

### August 3rd - August 16th

**MILESTONE 5** -

* Implement the Export Trainee as Instructor Feature in AMY.

### August 17th - August 21th 19:00 UTC

 * **Week to scrub code, write tests, improve documentation, etc.**

## Future works

Future work will include making improvements to the work-flow based on the feedback of this project. This would greatly help new trainees and trainers involved in the process of instructor training and would make the training process smoother.

## Open Source Development Experience

* Contributions to AMY:
  * https://github.com/swcarpentry/amy/pull/718
  * https://github.com/swcarpentry/amy/pull/721


* I have also contributed to another open source Django based project [KA LITE](https://github.com/learningequality/ka-lite) under the organization [Foundation for Learning Equality](https://learningequality.org/). Here are a few of my commits to the project.
  * https://github.com/learningequality/ka-lite/pull/4832
  * https://github.com/learningequality/ka-lite/pull/4893
  * https://github.com/learningequality/ka-lite/pull/4877
  * https://github.com/learningequality/ka-lite/pull/4928


## Academic Experience
I am a third year undergraduate from BITS PILANI( BIRLA INSTITUTE OF TECHNOLOGY AND SCIENCES), Hyderabad Campus majoring in Computer Science.
Some of my relevant courses were  database management, networking and object oriented design.
My skill set includes - Python,C/C++,Java,Django,MySQL, Sqlite and JavaScript.


## Why this project?

This project is a wonderful example of Software Design to tackle real life problems, where there is no exact, correct solution.
Designing a possible workflow was an enriching experience, where we had to take a number of issues into account to come up with a solution.
Working on this project, would give me more experience in Software Design and Implementation, and working on a live open source project is itself a very exciting feat.

## Appendix

### Availability

I would probably be doing a course over the summers but it would not take up much of my time.
* **Time Zone :** Indian Standard Time (IST) UTC +5:30
*  **Hours per week :** 30-35 hours

### Project Discussion
Discussion with the mentors about this project can be found [here](https://github.com/numfocus/gsoc/issues/99)

### Contact
|          |                               |
|----------|-------------------------------|
| Name     | Shubham Singh                 |
| Email Id | shubh.singh594@gmail.com      |
