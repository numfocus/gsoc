# Manage workflow for instructor training

## Abstract

Computing Skills have now become essential more so in the field of Science, but many a time we find researchers struggling with code or tools to get things done.
Software Carpentary is a step in this direction.
Since 1998, Software Carpentry has been teaching researchers in science, engineering, medicine, and related disciplines the computing skills they need to get more done in less time and with less pain.
Software Carpentary , uses AMY ( a django based web application) to to keep track of who's qualified to teach, what workshops have run, who taught and attended. As more and more people are applying to become instructors at Software Carpentary. It has become essential to manage the workflow for instructor training.

This project aims at designing such a workflow and implementing it by adding appropriate views to AMY.

## Proposed workflow

![HTML MOCKUP flowchart](https://cloud.githubusercontent.com/assets/15982349/13770094/aa23d670-eaaa-11e5-81a8-312f860ecd7b.png)

1. Interested Trainees apply as Group or Individuals by filling forms.
![HTML MOCKUP FORMS](https://cloud.githubusercontent.com/assets/15982349/13816694/6c7a68e6-ebb5-11e5-809e-eb5bd00d7a22.png)
2. All applications are listed in the *ALL_APPLICATIONS* view. In this view the lead trainer can accept or reject an application.
The lead trainer has to compose one sample email by filling a web-form before accepting an application, this email will contain information such as
date, location, mode(online or in person), instructor name and instructor email id. This information is also stored in the database for the accepted applicant to be queried on later.
Accepting an application will send an email, to the applicant that he/she has been accepted into the program with other details about the actual training along with one randomly generated password to login in to their account (this password can be changed to anything by the trainee after the first login).
The sample email has to be written once and can be sent to all the people who have been accepted for a particular training session.
For another training session the lead trainer would just have to fill the web-form again, overwriting previous values.
![HTML MOCKUP ALL_APPLICATIONS]( https://cloud.githubusercontent.com/assets/15982349/13816688/66a3e7e4-ebb5-11e5-83f1-5086ddddac13.png)

3. All accepted applications, are then transferred to the Trainee view. In this view the lead trainer, trainers,discussion leaders, lesson maintainers can track progress of the trainees, provide feedback and update the status of every stage. They are also notified here if the Trainee has requested a discussion session.
This list can be filtered on the following criteria:
  * instructor assigned.
  * start date of training.
  * location of the training.

  When the training is completed and the trainee has been certified, the lead instructor can export the trainee as instructor, to move their accounts to the Person table, and remove them from the Trainee table.
![HTML MOCKUP ALL_TRAINEE](https://cloud.githubusercontent.com/assets/15982349/13816909/3401c4a4-ebb6-11e5-9799-0eb21ccd5eea.png)
4. *Trainee Home page* -
After getting accepted for the training program, the trainees recieve a randomly generated password, which they can use to log in to AMY, a trainee's home page is different from the AMY DASHBOARD, the AMY DASHBOARD is reservered for administrative purposes only. After logging in the trainees are directed to TRAINEE_HOME. This view will have a nav-bar - **HOME** , **ABOUT** and **NOTIFICATION**.
  * *HOME*  shows the trainees progress, through a progress bar indicating which stages have been completed and the ones left. In this view the trainee can also request a discussion session by filling in a webform, with fields such as TOPIC, PREFERRED TIME, and so on.
This view will also list down the general guidelines for instructor training, informing what is required in each stage of the training.
  * *ABOUT* , this view shows the information entered by the trainee in their initial application, they can also edit this information, and change passwords here.
  * *NOTIFICATION*,  this view allows the trainee to view all the comments/feedback from the discussion leaders and the lesson maintainers.In the format *comment: by whom: timestamp*.


## Technical Details
The Software stack for this project can be divided into:

 * Backend  : [Django](https://www.djangoproject.com/)
 * Database : [SQLite](https://www.sqlite.org/)
 * Frontend : [Bootstrap](http://getbootstrap.com/),[JQuery](https://jquery.com/) for AJAX requests

### Features and Model Schemes

Trainee would be the main Model, maintaining most of the Flags for Progress and other information:

```python
lass Trainee(AbstractBaseUser):
    '''Model for a Trainee '''
    # listing just essential fields, more fields can and should be added when implementing
    COURSE_CHOICES = (
        ('1', 'SWC'),
        ('2', 'DC'),
        )
    personal    = models.CharField(max_length=STR_LONG)
    middle      = models.CharField(max_length=STR_LONG, null=True, blank=True)
    family      = models.CharField(max_length=STR_LONG)
    email       = models.CharField(max_length=STR_LONG, unique=True, null=True, blank=True)
    course      = models.CharField(max_length=STR_LONG, choices=COURSE_CHOICES, null=False, default='1')
    group       = models.ForeignKey(Trainee_Group,default=None,null=True)
    Attendance_Flag = models.BooleanField(default=False) # maintains attendance record
    Discussion_Flag = models.BooleanField(default=False) # maintains PASS or FAIL
    HomeWork_Flag = models.BooleanField(default=False) # maintains PASS or FAIL

    username    = models.CharField(
        max_length=STR_MED, unique=True,
        validators=[RegexValidator(r'^[\w\-_]+$', flags=re.A)],
    )
 ```

Maintaining Group information:

```python
class Trainee_Group(models.Model):

    instructor_assigned=models.ForeignKey(Person)
    primary_contact_personal    = models.CharField(max_length=STR_LONG)
    primary_contact_middle      = models.CharField(max_length=STR_LONG, null=True, blank=True)
    primary_contact_family      = models.CharField(max_length=STR_LONG)
    primary_contact_email       = models.CharField(max_length=STR_LONG, unique=True, null=True, blank=True)
    training_course             = models.CharField(max_length=STR_LONG)
```
Commenting System:
Since comments/feedback are only meant to be unidirectional (i.e from the discussion_leaders, lesson_maintainers to the trainee) a simple Commenting Model would suffice and there is no need for third party apps.

```python

class D_Comments(models.Model):
    ''' commenting system
    storing comments by the discussion leaders
    '''
    Discussion_Comment =models.TextField(default="", blank=True)
    Discussion_leader = models.ForeignKey(Person,default=None,null=True)
    trainee = models.ForeignKey(Trainee,default=None,null=True) # Many to one relationship
    created = models.DateTimeField(auto_now_add=True)

class H_Comments(models.Model):
    ''' commenting System
    storing comments by the lesson maintainers
    '''
    Homework_Comment = models.TextField(default="", blank=True)
    lesson_maintainer = models.ForeignKey(Person,default=None,null=True)
    trainee =  models.ForeignKey(Trainee,default=None,null=True) #Many to one relationship
    created = models.DateTimeField(auto_now_add=True)

```


## Schedule of Deliverables

I believe in test drive development so I will be writing tests as soon as I am done with a feature/view addition.

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
* Write Tests
* Expectation - the trainer should be able to accept trainee applications.

### June 22nd - July 5th

* **Mid term evaluation**
*  Implement the Commenting System.
### July 6th - July 19th

**MILESTONE 3** -

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

Future work will include making improvements to the workflow based on the feedback of this project. This would greatly help new trainees and trainers involved in the process of instructor training and would make the training process smoother.

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
