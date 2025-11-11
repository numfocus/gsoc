#### Software Carpentry
# Survey Visualizer Application

## Abstract
Software Carpentry hosts a series of workshops, each if which is associated with five surveys. The surveys are conducted on SurveyMonkey. Viewing the results on SurveyMonkey takes a lot of clicks, and hence an application is needed which can aggregate the results from the various surveys.

A Django-based application will be linked to Software Carpentry's SurveyMonkey account. The workshop organiser (user) is expected to provide the application with either:    
1. the complete URL of survey **OR** 
2. the workshop ID in an input field.

If the user provides the URL, data of the survey is displayed in the required format. In case a workshop ID is directly provided, all surveys associated with the workshop are listed. The user selects the survey and the data is presented.

The presentation of results for each question will depend on the question type. A basic idea on the presentation:  

 Question Type   |      Possible way of presenting the answers     
----------|-------------
 Single choice (*Radio* / *Menu*) |  Bar graph / Pie chart 
 Multiple choice |  Bar graph / Pie chart 
 Matrix Menu | Multiple Bar graphs / Stacked Bar graph / Grouped Bar chart 
 Matrix table | A Bar graph for each record / Matrix "chart
 Rating-type Question | Bar graph / Pie chart 	

In later stages, the application will be integrated with [AMY](https://github.com/swcarpentry/amy), a Django-based workshop administration application for Software Carpentry. 

## Technical Details
The application will require a one-time setup where in the application will be linked to the SurveyMonkey account of Software Carpentry. 

The technology stack for the application will consist of:
 * *Backend* : **Django**
 * *Database* : **PostgreSQL** / **SQLite**
 * *Frontend* : **Bootstrap** along with **Plotly.js**

#### **Backend**: Django
As stated earlier, the application will be developed in Django. Since the application will be associated with a single SurveyMonkey account, the access token shall be taken from the SurveyMonkey API console. This access token will be used to authorize the access to the account data.

As the SurveyMonkey account of Software Carpentry has 5 surveys, the URL feed by the user will be used to get the *survey_id* and *workshop_id*

The application will make a sequence of API calls:

1. Check if the data for the given *survey_id* and *workshop_id* exists in the database: 
 * If it exists, an API request will be hit at the **[/get_respondent_list]** with *survey_id* sent as parameter. The survey data is filtered using *workshop_id*. We check if any new results have been added since last time the data was fetched. For this purpose, we store a *last_access* field in the database, which stores the date of the last data fetch and send this variable as parameter to fetch only the new results.

2. In case there is no existing entry for the given *survey_id* and *workshop* or new data has been added:
 * 	An POST request will be hit at **[/get_survey_details]** with *survey_id* sent as parameter to fetch data associated with the survey (except the responses). The data is then filtered for the given *workshop_id*.
 * Creation of dictionaries to ease the calculation and the process of creating the final JSON object. For e.g., *answer_count*, that relates answer_id to count or *question_answer* to relate *question_id* to it's *answer_id*s.
 * 	Again an API request will be sent to **[/get_respondent_list]** to obtain the list of all respondents that participated in the survey (*respondent_ids*). This will be required later to fetch responses to the survey.
 * 	An API request to **[/get_responses]** will return the JSON objects which consists of ids of all answers marked for a single question for all respondents.
 * For every *respondent_id* in *respondent_ids*, we iterate over all *question_id* and for a given question, the *count* is calculated against each *answer_id*.
 * The calculated result is saved as a JSON object and stored in the database.

The detailed structure of the models used:
```python

class Question(models.Model):
	TYPE = ( 
		('single-radio', 'Single Choice [RADIO]'),
		('single-menu', 'Single Choice [MENU]'),
		('mutliple', 'Multiple Choice'),
		('matrix-menu', 'Matrix Choice [MENU]'),
		('matrix-table', 'Matrix Choice [TABLE]'),
		('text-field', 'Text Field'),
		('rating', 'Rating'),
	)
	CHART = (
		('bar', 'Bar Gaph'),
		('pie', 'Pie Chart'),
		('multiple', 'Multiple Bar Graphs'),
		('matrix', 'Matrix Chart'),
		('text', 'Text Field'),
	)
	id = models.IntegerField(default = 0)
	text = models.CharField(max_length = 512)
	type = models.CharField(choices = TYPE)
	chart = models.CharField(choices = CHART)

class Answer(models.Model):
	id = models.IntegerField(default = 0)
	question = models.ForeignKey(Question)
	text = models.CharField(max_length = 32)

class Organiser(models.Model):
	name = models.CharField(max_length = 128)

class Workshop(models.Model):
	id = models.CharField(max_length = 256)
	host = models.ForeignKey(Organiser)
	date = models.DateField()

class WorkshopSurvey(models.Model):
	all_survey_ids = [112, 123, 235, 358, 5813]		# random values chosen for demonstration purpose
	SURVEYS = ( 									# linking type of survey to the survey_ids 
			(all_survey_ids[0], 'PreWorkshop Survey'), 
			(all_survey_ids[1], 'PostWorkshop Survey'),
			(all_survey_ids[2], 'Organizer Survey 1'),
			(all_survey_ids[3], 'Organizer Survey 2'),
			(all_survey_ids[4], 'Longterm Survey'),
		)
	workshop = models.ForeignKey(Workshop)
	survey_id = models.IntegerField(choices = SURVEYS)
	participants = models.CharField(max_length = 10000)
	url = models.CharField(max_length = 512)
	results = HstoreField() 						# models.CharField() in case of SQLite
	last_access = models.DateField()
```	  

The *results* field for a WorkshopSurvey object will store JSON in the following format 
```python
{	
	'index' :	
		{
			'id': 		question_id,
			'answer':	[
							{
								'id': 		answer_id
								'count':	answer_count['answer_id']	 	 #answer_count shall be user-defined dictionary as described above
							},
						]
		},
}
```

##### Database Schema
![Database Schema](https://lh3.googleusercontent.com/c1DJI58ZVreLs2aU8n28RcKORXwt2y3ZkyadzYImNswuRI85z-7zdD4nlDKXeETyOiKsugVgpmXn7cxot4M5vJJNILiRFe_4gqPWBmNt5BvLpkNgM64v9gRLlA0X7AeFoYNmdv17I-7tOHEnRJuA5EQKxvK4XhotJg755rmwxGXccDhjNxUPjGepcjQHUy-S52w8e0ct077YMhhpYSbcxaucNnWlcVb1JQHsp7xo9i5AH0k_B6wzvnOtn4OHd2YSGTz2t-lqOywkMRYAtVbcncWOAIB5W1oqLyB9_ANCMnb25fIyGiS75ZE0ZLYv6q5WO6wXCL6R37Inq1xhyZdWqqjwPziBm0LCa_MAZ_3CJTF5jhtXog8PFR4yp6rCdzKnO2glVRDZTAI-peHpsxCMsm6N67FI7E7-FNUvN3ynusKCXe84VfdS4_tGM9cHHlmJcsUUYQIbqpwycLSyIUTbPB4Eb5aZKlRfHV946pS4Yc2QcxwUCcGOqK3NUI1r6uZt--8DKi0E9DVR45rwvdVXN7l9DuB-mcIlpWpiSM2GclYM4FvlcLVsPDKxneAtN9U=w896-h443-no)
The arrows represent *Many-to-One* ,i.e. **Foreign Key** relationship

##### Explaination
In order to (i) make querying easy & fast and (ii) store results in an optimal data structure, I have proposed the above design. Most of the queries and filters would be on variable like *workshop_id*, *survey_id*, *date* or *host*, we can easily search the *Workshop* & *WorkshopSurvey* tables for any such query. The *Organiser* is maintained as a seperate table, as we may need to associate more data with a host.   
The *WorkshopSurvey* table will store the calculated JSON (in case of PostgreSQL) or the JSON dump (in case of SQLite) in the *results* field. The field will store a list of JSON objects each storing a *question_id* associated with a list of *answer*s (a dictionary). When the *results* are fetched, we query the *Question* & *Answer* tables for the *id*s and obtain the necessary data (question-text, question-type or answer-text).  
This schema would work for both PostgreSQL as well as SQLite. 

##### UML Sequence Diagram
![Sequence Diagram](https://lh3.googleusercontent.com/ly5wZV2HjIlDMSuPA6evW-a-mdphQ2X4ZAdJWyfCxhNU3GbMPNauPSMf2gEeBYQ-JqA6h8BZ02-dYad-hnwubrO5hTir4Y-4aEd0FNHIPDPKorhbcY6hB_azuiVnwcgMCwR1oYN9XmSwFw1YRoxYMQ3Bz-p_yzQik4m55kimcMyiSjhoW-NYFtjBrksbvg4wIkFQ7YfBIUNbW3ezaOILfRqMZsCU6YWOR-rlMuozt1VmUyCM8hnvYrMlgjqX8xdKEitSBYWGp0quY0RccgB03IiIP3w4uvy5h8nc7dp49D9Whb39msXY84e_TC2jaYKeGJHfDJujpGf0mtS7-gZts2f3J2UXnaI7_4ney6qXqXwpbRYME4zLWeCL4cX81KbXJmk_IqDQSCBePQdO-0ZwcKIh0CscmQf4gaQIpF1Mw-yehHuKhP99VJ0G282N9xnCAytkl-mC93AZxB_NOWAgkJEB7cny_dBbulksz4C565mUzXMduhEEh8x6ax503EamMOtt-PGGdprPNbBOKn9KfxAmmCs4RK9ct0u3yq5B9vHNCATP0f16NYL8WZVGwN8=w777-h677-no)

#### **Database**: PostgreSQL / SQLite
With the release of v9.4, extensive support for JSON has been added to PostgreSQL, making it the perfect solution for our idea. PostgreSQL can easily handle large amounts of data and thus, speed performance and fetching of newer data wouldn't be an issue.  
However, as per the discussions with the mentors, I have been made aware that SQLite is currently being employed in AMY. With the discussed database schema, we can store the JSON as a string in SQLite.  

#### **Frontend**: Bootstrap + Plotly.js
The reason for narrowing down the choices out of various charting libraries to a single one is the fact that this library enables developers to design responsive, customizable charts and adds functionalities to the already large feature set of *D3.js*, on which it is based. Further, (i) it is completely free & open-source (ii) *Plotly playground*: developers can run write the code online & modify using GUI (thus, modify, enhance & test charts well before using them) *and* (iii) it is easier to use.

## Schedule of Deliverables

### Community Bonding Period
This time will be used to perform complete *requirement analysis* of the project and  to better know the Software Carpentry community - the users as well as the developers. During this period, the discussions will be held with mentors, as part of the analysis phase, to finalize the minute details including:
* presentation layout for each question-type and answer-type
* database schema
* way to present the choices (of 5 surveys) when the workshop ID is provided 


### May 25th -  June 7th
*Begin coding.*  
As I have implemented OAuth during the discussion period and have good experience of coding in Django, I will develop the backend (along with frontend - except the charts - i.e. all models, templates, urls and basic views will be coded) and use SurveyMonkey OAuth to authenticate the application with Software Carpentry's SurveyMonkey account.

### June 8th - June 21st
*Using the API.*  
The API usage will be implemented to aggregate the survey data. During the discussion phase, I had developed scripts to use the SurveyMonkey API and realised some issues (discussed below). Hence, some time (~ 2 days) would be spent on this aspect. 
By the end of this period, the survey data for a given workshop shall be processed and stored.

##### **MID TERM EVALUATION**  
*Milestone #1:*  
Show working Python backend: authorization, fetching and processing of data.

### June 22nd - July 5th
*Developing the frontend.*    
*Begin documentation.*
The charts and graphs will be implemented, using the processed data & the progress so far will be documented along with the necessary examples.

### July 6th - July 19th
*Testing & (continue) documentation.*    
The application should be completely working by this time. Thus this period will be used to thoroughly test the application for various bugs in backend and frontend. Tests cases used in this phase will be documented, along with the system response. Also, if any improvements are realised, will be worked upon in this time.

### July 20th - August 2nd
*Working with AMY. [1]*  
Since, the application will be fully functional by this time, the next 4 weeks will be spent on studying AMY and integrating the application with it.
	
### August 3rd - August 16th
*Working with AMY. [2]*  
Besides integration, I shall continue working on documentation towards the end of this phase. 	

### August 17th - August 21st 19:00 UTC
*Wrapping up.*  
The documentation will be finalized. Code will be beautified along with removal of redundant code.

##### **FINAL EVALUATION**
*Milestone #2:*
* Show a working, launch-ready project.
* Show a working unit of test suite.

## Future works
Continue working on this project, along with AMY. I would continue to contribute, test and fix bugs that will be reported. I will work on improvements in AMY and also contribute to future projects of Software Carpentry.
	
## Open Source Development Experience
I'm new to open source. This project would be my first opportunity to contribute to an open-source organization. I found the idea quite exciting - especially since I have already worked on few large-scale Django projects. However, I have developed **Media Management Service**, a Django-based backend service that handles media files and requests. It's features include media storage, file recovery, file transformation, file compression and user defined rule-based authentication. Further, it is integrated with Django Forms and [Django File Manager](https://github.com/IMGIITRoorkee/Django-filemanager) (An open-source GUI File manager developed by [Information Management Group, IIT Roorkee](https://www.facebook.com/IMGIITRoorkee/)). As soon as the documentation gets complete, I plan to release this service module as open source. 

## Academic Experience
I'm a second year undergraduate student at Indian Institute of Technology Roorkee (IIT R), majoring in Computer Science & Engineering. I have strong programming experience in Python, PHP, SQL and C++. As a part of academic curriculum, I have taken courses in *Software Engineering*, *Object Oriented Analysis & Design*, *Data Structures & Algorithms*, which have equipped me with skills to design, develop and test applications of industry standards.  
Besides software development, I'm passionate about cryptography, information security, machine learning and theoretical computer science.

## Why this project?
Being motivated to contribute to an open-source organization and having worked on Django for more than a year, I believe this project would be a perfect start to contribute to OSS. As a member of [Information Management Group](http://img.channeli.in/works/), I have worked on many Django-based applications and have primarily worked on developing	Media Management Service and the Institute Placement Portal. The online competition portal that I developed for demonstration purpose can be found [here](https://github.com/meet-vora/euclid). This project seems very exciting to work on, given my background. Further, I have been able to use the SurveyMonkey API as well as Plotly during the discussion period and have well understood the idea. I have also realised the trouble one could get into, while using the SurveyMonkey API - querying with inclusion of custom variable *workshop_id* and OAuth, and have discussed the other details and issues [here](https://github.com/numfocus/gsoc/issues/98). Thus, I believe that I am well equipped with the skills, excitement and motivation to work on this project.  

## Other Details
#### Summer Schedule
I have no other commitments scheduled for the summer and would devote at least 40 hours a week, and more if required.
The ongoing semester will be completed by 6th of May. The next semester is scheduled to begin on 20th of July, 2016. 

#### Contact
Email: [meetvora@outlook.com](mailto:meetvora@outlook.com) / [v.h.c.meetirth@gmail.com](mailto:v.h.c.meetirth@gmail.com)  
Mobile: +91 XXX XXX XXXX