# Manage Workflow for Instructor Training

## Abstract

Software Carpentry has been training scientists with computing skills since the last 18 years, and the number of instructors has been growing rapidly since its inception. More and more people are applying for instructor training programme which makes it difficult to manage the whole process manually. To help ease the problem, this project helps admins manage the whole workflow of instructor training starting from application to final checkout. This would help orgainizing the currently scattered workflow, getting rid of scattered emails and what not.

## Proposed Workflow

The whole system to manage instructor training is supposed to have two interfaces:

1. The Trainee Admin - An administrative interface - which is used by admins to manage trainees at each stage, and

2. The Trainee Dashboard - the one used by trainees to keep track of their progress.

The process starts with applicants applying for instructor training programmes

### 1. The Application Form

The application process for becoming an instructor starts here. People aspiring to become instructors shall start by filling up this application form.

Trainees apply in one of the following ways:

  1. **Apply individually**:

  This would be a normal application, requesting the relevant details from the applicant.

  **Helping individuals form groups**

   * Apart from the regular details, the applicant would also be asked if they are willing to pair up.
   * If yes, they would be presented with a list of individuals or groups willing to accept new members. The applicant can then request to pair up with the individual/group of their choice.

  **Determine alredy associated applicants**

  The applicant will be presented with a list of partner organizations which will be used to determine whether they are already associated with a partner organization

  2. **Apply in a group**

  Applicants can also apply in groups, making it easier to manage a bunch of people together. This would have the following fields:

   * Individual details of every member, as in [1] above
   * Group Leader
   * Whether or not they are willing to expand the group


### 2. Managing Trainees - The Trainee Admin

  The Trainee Admin will be used to manage trainees at each stage of training. Access will be based on permissions (to be discussed during GSoC), so that the right people get to change and monitor the trainee status at each stage. For now, an *admin* is the relevant person handling a particular stage for an applicant. The admin workflow is outlined here:

  * All the applications will be visible to *admins* on a dashboard.
  * The *admins* start by reviewing applications for instructor training that were filled in the form above.
  * They can then shortlist applicants and match them to courses. Multiple applications can be updated at once for ease.
  * Once this is done, the applicants are sent an invitation to attend the selected course and also to join the Trainee Dashboard.

  >Trainees log in, and attend the course

  * The *admin* updates course completion status

  >Trainees submit homework link

  * The *admin* reviews the homework, and provide necessary feedback. Once they are satisfied, they update homework completion status.

  >Trainee provides preferred time for discussion session (optional)

  * The *admin* fixes up a discussion session

  >Trainees attend discussion session

  * The *admin* evaluates the discussion session, providing necessary feedback. If satisfied, updates the discussion complete status

  >Trainee provides preferred time for discussion session (optional)

  * The *admin* fixes up a checkout session

  >Trainees attend checkout session

  * The *admin* evaluates the checkout session, providing necessary feedback. If satisfied, updates the checkout complete status
  * When this is done, the applicant now becomes a trainee, and they can be managed from the AMY instructor dashboard.


### 3. Trainee Dashboard

  Every applicant that gets selected will be provided with an access to the trainee dashboard. This will be a one stop solution to all their queries. Most importantly, they'll know what has been done and what to do next. Here, the trainee can track their progress, interact with instructors at any stage, and find out the next steps need to be done. This makes things simpler for everyone.

  * **Know alloted course**

  As soon as the trainee logs in, they'll be shown with the course they are matched with. When they complete the course, they'll be taken to the next step - homework submission.

  * **Homework Submission**

  Trainees can submit the link to their pull request or Google Doc. This way it'll be easier to track submissions.

  * **Discussion sessions**

  The trainees will have an option to select a time slot out of the available ones. This makes matching easier.

  * **Checkout session**

  If the trainee passes the discussion session, they'll again be able to give preferred choices for checkout sessions.

  * **Training complete**

  Once the training is complete, the trainees can download certificates, and move to the AMY instructor dashboard

### Salient Features

  **In house messaging and feedback**

  At any point of time, the trainee and the relevant instructors will be able to communicate from within the portal. This solves the hassle of unorganised emails.

  **Preferrential selection of discussion and checkout sessions**

  This makes the process flexible, and comfortable.

  **Email notifications**

  Email notifications would be sent in case of important events such as an upcoming discussion session, so that the applicant doesn't miss any deadlines

### Flowchart

Here's a graphic that summarizes the process

![Image](https://docs.google.com/drawings/d/11PIidJrIwYlGbbb5PvyWXpa6r8lG0OVQ7ZJGbefU6VU/pub?w=920&h=1006)


## Technical Details

**Backend**

The portal will use **Django** for obvious reasons.

**Database**

As the portal would be a subapp within the AMY project, it will use the same database as AMY. But since the portal, and AMY, in general would be handling a higher load, `PostgreSQL` would be the preferred database. An issue is already raised [here](https://github.com/swcarpentry/amy/issues/716). If this gets addressed, we'll use PostgreSQL. If not, the system can also run on SQLite during development phase.

**Design Framework**

The design is something I want to pay attention to. The trainee dashboard, as well as the admin will be deeply integrated with every stage of instructor training. Since this is something that is to be used regularly, the UI has to be elegant, which vanilla Bootstrap is not.

I propose to either use another theme, or a different framework, such as `MaterializeCSS` for the Trainee Dashboard. Amy can still continue to use Bootstrap for consistency.

**Emails and Notifications**

The portal will use Django's default `django.core.mail` module for sending emails and notifications.

**Social Login**

The portal would allow the use of social accounts, such as Google and GitHub, to login to the Trainee Dashboard. This would be done easily by using `Python Social Auth`

**Messaging**

`Django-Postman` is a well tested package suited for our needs. This can be used for messages


## Schedule of Deliverables

### May 25th -  June 7th

Discuss upon delicacies:

* Discuss the structure of models
* Decide the permissions hierarchy
* Finalize the design framework
* Finalize on the dashboard interface
* Implement the application form

### June 8th - June 21th

* Work on Trainee Admin
* Implement the `Review Applications` functionality
* Work on automatic email notifications

### June 22nd - July 5th

* Complete admin reveiws for homework, discussion and checkout sessions
* Implement the trainee dashboard

### July 6th - July 19th

* Implement social authentication
* Implement the messaging system
* Write tests for application form and Trainee Dashboard

### July 20th - August 2nd

* Write tests for Trainee Admin
* First draft ready for review
* Review *admin*'s feedback

### August 3rd - August 16th

* Work on reviews from *admins*
* Final optimizations
* Wind up writing tests
* Find and fix critical bugs
* Write documentation

### August 17th - August 21th 19:00 UTC

Buffer for final wrap up, fix if something remains.

## Future works

The system has enormous scope of expansion, the major ones being:

1. Integrate discussion session within the portal, which is like a Google Doc open within the portal
2. Incorporate video calling with screen sharing abilities from within the portal to integrate checkout sessions.


## Open Source Development Experience

  As far as 'Open Source' is concerned, I've only contributed to AMY and related projects. I have:

  * [reported a bug](https://github.com/swcarpentry/amy/pull/720), which returned all persons instead of none if no duplicates were found, and also provided a [fix](https://github.com/swcarpentry/amy/pull/720) for it, which has been merged.
  * working on [expanding AMY to incorporate certificate generation](https://github.com/swcarpentry/amy/pull/724). The pull request is under discussion
  * helped [eliminate the use of Inkscape](https://github.com/swcarpentry/certification/pull/12) by replacing it with CairoSVG in the certification repository
  * helped [tweak some styles](https://github.com/gvwilson/python-novice-gapminder/pull/1) in the [python-novice-gapminder](https://github.com/swcarpentry/python-novice-gapminder) repository

## Academic Experience

I am an undergraduate from BITS Pilani pursuing majors in Electrical and Electronics Engineering at BITS Pilani. I'm currently in my second year and have taken a variety of interesting courses such as Control Systems, Microprocessors Programming, Digital Design, OOP and Machine Learning.
I'm also interested in web development, and I develop websites of our college fests. Some projects undertaken during college are:
* The backend of websites of our college fest, built on Django: [Oasis](http://bits-oasis.org/), [BOSM](http://bits-bosm.org) and [APOGEE](http://bits-apogee.org)
* An event management system, to keep track of the progress of teams in an event as a part of APOGEE 2016
* An in house registrations portal, to assign IDs, billing and allot accomodation to outstation participants

## Why this project?

I've developed Django applications in the past, and I wanted to improve upon my skills by means of working with experienced developers for some real world applications. And after some contributions to AMY, I seem to understand it well, and I'm also comfortable enough with AMY. This gives me more reasons to work on it! Also, I can promise quality work, and hope that this becomes a wonderful experience for all of us.

Some of my previous contributions are:

**Oasis 2015** ([GitHub](https://github.com/dvm-bitspilani/oasis-2015-backend)) ([bits-oasis.org](http://bits-oasis.org))

Extended and revamped [regsoft](https://github.com/dvm-bitspilani/oasis-2015-backend/tree/master/regsoft), the on-campus registration software, and the portal for participant registrations.

**Apogee 2016** ([GitHub](https://github.com/dvm-bitspilani/apogee-2016)) ([bits-apogee.org](http://bits-apogee.org))

Developed [EMS](https://github.com/dvm-bitspilani/apogee-2016/tree/master/ems), an Event Management System to keep track of participation in all events of the fest, and the backend for [Lacuna](https://github.com/dvm-bitspilani/apogee-2016/tree/master/lacuna), a puzzle based game.

**BOSM 2015** ([GitHub](https://github.com/dvm-bitspilani/BITS-BOSM-2015)) ([bits-bosm.org](http://bits-bosm.org))

Developed an [admin interface for PCR](https://github.com/dvm-bitspilani/BITS-BOSM-2015/tree/master/bosm2015/pcradmin), the [portal to manage participant registrations](https://github.com/dvm-bitspilani/BITS-BOSM-2015/tree/master/bosm2015/registration) and update event data dynamically on the website.
