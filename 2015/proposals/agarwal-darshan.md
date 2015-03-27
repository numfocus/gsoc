# Enhance Amy, a Django application for managing workshops

## Abstract

Amy is a web-based application built using Django framework, for management and administration of workshops. The number of workshops run by Software Carpentry are increasing and currently the organization of workshops is done by volunteers in bits and pieces. So, Amy was started to simplify workshop management. The main focus is to make it easy for administrators to create workshops, keep track of what workshops are being arranged, who are the instructors and what they can teach, who are the learners and so on.
Currently, basic functionality for Amy is in place. 

This project deals with enhancing Amy by adding many new features to it and make it use a lot more testing.
The project consists of two main sections:
	1. Thorough unit testing of existing application and fixing any bugs that the tests turn up.
	2. Adding following features for easy management of events:
		* Integration with GitHub for authentication.
		* Integration with Eventbrite to check :
			* If the registration of the event exists
			* If the registration of the event is up and running 
	3. Integration with Mailman
		* Create a mailing lists for each workshop with instructors, helpers and hosts on it.
		* Displaying which lists a person belongs to.
		* Displaying members signed up to a list.
		* Displaying lists per workshop.
		* Allowing hosts to co-manage the mailing lists for the workshops along with admins. 
	4. Reports (with graphs visualisation):
		* Number of workshops held in the last month/year/day.
		* Number of people taught in the last month/year/day.
		* Number of instructors who have taught in the last month/year/day.
		* Number/List of instructors who have not taught in the last month/year/day.
		* Number/List of instructors who are about to teach in the next month/year/day.
		* Event with the most people/instructors in the last month/year/day.
		* Sites where most number of workshops were held in the last month/year/day.
		* Sites with the most workshops held in the last month/year/day.
		* Hosts with the most events in the last month/year/day.
		* Hosts that have not hosted events in the last month/year/day.
		* Number of hosts who have hosted their first event in the last month/year/day.
		* Number/List of events with fee waiver.
		* Number/List of events hosted by partners.
		* List of events and the corresponding fees received by it.
	5. DataTables for quick in-table search #129 (https://github.com/swcarpentry/amy/issues/129)
	6. Display map of known airports #82 (https://github.com/swcarpentry/amy/issues/82)
	7. Use calendar popup for picking dates for workshops #18 (https://github.com/swcarpentry/amy/issues/18)
	8. Improvement in UI by handling the following issues and adding other features as needed:
		* Add alphabetized "jump to" links for sites and people  (https://github.com/swcarpentry/amy/issues/189)
		* Extended navigation bar #221 (https://github.com/swcarpentry/amy/issues/221): improvising this feature by implementing full menu collapse according to screen size.
		* Autocomplete form fields #233 (https://github.com/swcarpentry/amy/issues/233)
		* Add CSS to make display less ugly #10 (https://github.com/swcarpentry/amy/issues/10)
		* Headings hierarchy #99 (https://github.com/swcarpentry/amy/issues/99)
	9. Implement backups of data on server #168 (https://github.com/swcarpentry/amy/issues/168)
	10. Keep track of sponsorships for the workshops [Event_sponsor table #74](https://github.com/swcarpentry/amy/issues/74)
	11. [Additional event attendance information (slots and applicants)? #64](https://github.com/swcarpentry/amy/issues/64)
	12. [Event details page should allow validation of Eventbrite link #19](https://github.com/swcarpentry/amy/issues/19)

## Technical Details
* For the integration of Github or any social authentication I would be using Django Social Auth following the [Django Social Auth Backend System Docs](http://django-social-auth.readthedocs.org/en/latest/backends/github.html)  for which it will first require much configuration after installing the App as mentioned  [here](http://django-social-auth.readthedocs.org/en/latest/configuration.html).
* Use EventBrite API for integration of Eventbrite by first getting Eventbrite key for Amy and following the [Eventbrite docs](http://developer.eventbrite.com/docs/)
* For integration with Mailman, I will use Mailman 3 (hoping to release in April 2015), since it exposes a REST API and will use [mailman.client](https://launchpad.net/mailman.client) (Python) to interact with that API and by following the [doc](http://mailman-cli.readthedocs.org/ (as discussed in https://github.com/swcarpentry/amy/issues/42) I would be
	* Getting the domain and creating mailing lists for each workshop as required and adding members to it.
	* Following the syntax and direct commands of mailman.client to display lists a member is signed up to and to display members subscribed to a list.
	* Changing the moderators and owners of a list accordingly for allowing hosts to co-manage the workshops.
* For the introduction of Data Tables in Amy for the display of all the tables, as discussed and implemented in [swcarpentry/amy/pull/134](https://github.com/swcarpentry/amy/pull/134), I will use the DataTables jQuery plugin, pass data as JSON and have Data Tables render only the data you can see. I would also try to use Data Tables server-side processing as suggested by W. Trevor King.
* For the implementation of Reports, to retrieve the data I will be using [Django database-abstraction API](https://docs.djangoproject.com/en/1.7/topics/db/queries/) to query the database and for visualisation in form of graphs I would like to use [Google Charts](https://developers.google.com/chart/interactive/docs/index) which uses HTML5/SVG technology to visualise the data. Though I am also open to any other technology for it.
* UI features will be added using Bootstrap3.
* For calendar popup, the calendar widget ‘AdminDateWidget’ which Django uses on it’s admin pages can be used.
* Add one more field - ‘seats’ to the Event Table to check how many seats were available.
* I would prefer adding a ‘Sponsorship’ field to the Event table, which can take multiple values instead of having a separate Event_sponsor table.

If time permits I would be addressing the other issues on  https://github.com/swcarpentry/amy/issues


## Schedule of Deliverables

### May 11th -  May 24th
Thorough Unit testing of existing modules in the application and finding out bugs if any. Create corresponding issues for it and resolve them.

### May 25th -  June 7th
Push the existing pull requests after testing it thoroughly.
Closing the corresponding issues for the pull requests which are merged.
Github Authentication Integration


### June 8th - June 21th
Eventbrite Integration
Data Tables for all table views.
Writing unit Tests for the Eventbrite and GitHub integration. 


### June 22th - July 5th
Upgrading to Mailman3 and adding the features mentioned above using REST API.
Writing unit Tests for the features added which uses Mailman 3.  
### July 6th - July 19th
Retrieving the data by querying the database for reports page.
Representation of data using graphs/charts by using Google Charts.
Writing the Unit tests for the above features.

### July 20th - August 2rd
Making required changes in the Database by addition of sponsors field and slots field in the Event Table after discussion with admins.
Writing corresponding views for the above changes.
Writing Unit tests for the above changes.
Implementing btrfs for backup of data on servers.

### August 3rd - August 16th
Making the UI better by implementing the suggested changes, making the procedure for filling the forms easier and adding new features to make the interface look good and easy to use.
Displaying Map of known Airports.

### August 17th - August 21th 19:00 UTC
Working on other issues and trying to resolve them.


## Future works
My experience till now with Software Carpentry has been great. It has been a great benefit for me to contribute to Software Carpentry. Apart from professional growth, I have improved my technical and communication skills. I have learnt many small details on how to develop in a distributed environment, which I had missed on before. I would stay in Software Carpentry as a long time contributor for my own benefits in continuing on this learning curve. In the future, I would like to add features to Amy, so that it becomes usable for instructors and hosts also.

## Open Source Development Experience
Software-Carpentry is my first and only FOSS organization at the moment. My contributions have been towards the development of Amy. I have written unit tests for the module ‘bulk add people’  and created a pull request for it:
	* [Updated test_util.py #225](https://github.com/swcarpentry/amy/pull/225),
I have found out bugs in the application and created issues on Github for the corresponding bugs:
	* [Bulk-upload: Not checking whether it is a valid csv file #231](https://github.com/swcarpentry/amy/issues/231)
	* [Bulk-upload: New User added even in case of existing user #226](https://github.com/swcarpentry/amy/issues/226)
I have resolved one of those bugs and had created a pull request for it:
	* [Case-insensitive matching of email #227](https://github.com/swcarpentry/amy/pull/227)


## Academic Experience
I am a third year Computer Science Undergraduate at IIIT Hyderabad and pursuing research in the field of Computational Linguistics. I have written a large number of small programs/applications as part of my course work. Some of my major hosted projects are:
	* [Docker Application](https://github.com/darshan95/Docker-Django-app) : We as a team of 3 members, developed a web interface using various docker remote APIs to create, delete, restart, etc. any docker container. Our application also kept record of current user containers, volume storage, network, etc. We also provided the facility of using linux terminal on our application.
	* [Cycle Run](http://buildinprogress.media.mitedu/projects/.2303/steps) : Developed an Android application at MIT Media Labs Workshop that gamifies and socialises the Real life Cycling Experience! We map the real life cycling data to an event(real life) based narrative game.
	* [Grade Portal](https://github.com/darshan95/View-Grades) : Developed a secure online portal using web2py  to view the grades and courses taken by a particular student.
	* [Readit](https://github.com/darshan95/Readit) : This Application is developed in web2py to post and rank links to online news items, similar to [reddit](http://www.reddit.com/) website. 
	* [Handwritten Digit Recognition](https://github.com/darshan95/HandWrittenDigitRecognition) : Classification and recognition of hand-written digits using mathematical morphology.
Apart from my projects, I have industrial experience too, I have done two internships in the past year. In the last summer, I worked at [mintables](http://www.mintables.com/) as a full-stack web developer. There I built an e-commerce platform using PHP, HTML5 and MYSQL. Also I have worked at Unorthodox as a member of web developer. Unorthodox is a startup which deals with image processing and computer vision. There, I worked as a web developer and developed an application in Django.

## Why this project?
Software-Carpentry is a non-profit organization which is student and research oriented. Researchers/Scientists benefit a lot from attending the workshops which are held by Software-Carpentry all over the world. Therefore, it is important that the management of these workshops should be as smooth as possible for admins. So, Enhancement of Amy is necessary for it. I have the required skill set and prerequisite knowledge required for this project. I have also set up and understood the code of Amy. Previously, I have worked on developing applications in Django and web2py which can be seen in the previous question.

## Other Schedule Information
I do not have any known commitment for the period of GSoC and will be contributing 35 hours per week.
