#Result-aggregation server for the installation-test scripts

##Personal Details

* Name: Yashasvi Girdhar
* Email: yash.girdhar@gmail.com
* Personal Webpage : http://about.me/yashgirdhar
* Github profile : https://github.com/itsyash
* IRC nick (on freenode) : yash_
* Country of residence: India
* Timezone: IST (GMT+5:30)
* Primary language: English

##Abstract

Software Carpentry has installation-test scripts which are used by students to check if they have correctly installed the required software for a workshop.

The script basically performs a test for each software and outputs the result. In case of a failure, it prints the cause of failure and some diagnostic information on how to install the software correctly.
Until now, we do not collect these results anywhere. But there are reasons why we should be doing that. Statistics about the results will help us figure out more information about:  
* How much trouble the students are having with the installation.
* Which dependencies need more of our attention (deprecating their older versions or adapting with the new versions).

Some sample queries that the system would serve will look like:

* How many tests failed for a given workshop for students running Windows.
* What fraction of failed tests were on OS X for a specific time period.
* For a given workshop, find number of failed version tests for Git.

######Amy for the server side?  
Considering a very little overlap between the backend of Amy and this project (Amy focuses on planning workshops and this project will store the installation-test script results and manipulate them), I
think it would be good to make this aggregator an independent project.  
In future, we can always use something from Amy if we want, such as fetching the workshop requirements. We can use workshop slugs to identify workshops in the "results entries" table which will give us the
ability to work with both the datasets.  

##Technical Details

The project is mainly divided into four phases:

#### 1) A model for storing the data in a relational database

We start with deciding what information is useful and how should we store it. I have already built a first draft of the [schema](#schema) (added at the end of this proposal) with feedback from the mentor.
I would use SQLite database for storing the data.

####  2) API to collect the results

This step involves designing an API on the server, so that students can submit their results through the installation-test scripts. The API would collect the results and store them in the relational database, conforming with the schema.
I would be using [Flask](http://flask.pocoo.org/) and [SQLAlchemy](http://www.sqlalchemy.org/) for this API. Flask is quite lightweight and flexible, and is powerful enough to provide us all what is needed by this project. Regarding SQLAlchemy, I haven't seen a more simple yet powerful SQL toolkit and ORM. The above is a tried and tested combination for API implementations.

#### 3) Modifying the installation-test scripts to send data to the server.
Here, we send the required data to the server using the API. I have understood the script and submitted a [PR](https://github.com/wking/swc-setup-installation-test/pull/4) as well that adds a comment explaining the implementation details.

#### 4) API to perform queries on the data (includes building an interface for the Software Carpentry admins)

This phase mainly involves three steps:

* Designing an API to perform queries on the stored data.
* Building an interface for the target users to use the API, and
* Adding a data visualisation library for better viewing of the results.

For constructing the queries, I plan on writing a custom UI in [AngularJS](https://angularjs.org/). 
We want a simple and minimalistic interface that's easy to maintain in the long run. Angular conforms with all this requirements and is easy to learn.

For data visualisation, I am still doing my research on various JavaScript libraries, but at present I am inclined towards [D3.js](http://d3js.org/) due to its wide acceptance and powerful features.

##Schedule of Deliverables

I plan to complete the backend before the mid-term evaluation, so the rest of the term will be used for frontend and testing.

##### 27th April to 23rd May : Community bonding period

I would use this period for learning more about the technologies that I would be using and understanding how Software Carpentry works. I think it’s important that I believe in its vision and have an inspiration to stay connected with it even after the project is completed. I do not want to limit my involvement to just three months.
I have been learning the technologies that I mentioned above and I think there is enough time before the project starts to get a good hold of them.

Moving on, this period is also important for us to decide what information is useful. I have already discussed this while building the schema and will update if needed.


##### 1) Build the schema: 23rd May to 26th May

I have already built a draft of the [schema](#schema) with some feedback, so it shouldn’t take much time.


##### 2) API to send data to the server : 27th May to 5th June

* decide what all we need to send and how to gather it.
* decide how to send. Most probably using urllib2 so that no extra dependency is introduced

##### 3) API to receive the data : 6th June to 14th june

* receiving the data on the server
* storing it in database according to the schema decided
* Writing documentation for the API.

##### 4) API for performing queries : 14th June to 28th June

* performing queries on the database
* start with basic queries
* this is the time to test the schema for its speed and efficiency. We may need to change the schema here if it doesn’t perform well with the queries.
After this, we move on to getting feedback on the API, work on the feedback, write some tests and clean up the code.

This is where our decision to separate the frontend and backend will benefit us.
We won’t move to frontend, unless we have the complete backend ready.

I have kept almost 1 week free here, so that we have some time on our hands in case any issues come up. Other than that, I would use this time to write documentation about the API for performing queries.

###Start the frontend here:

##### 5) Decisions to take before starting : 4th July to 10th July
Decide upon:

* what all functionalities/features we want to provide to the user
* how do we want to show the data to the user

##### 6) Frontend interface : 11th July to 26th July

Initially, show the results of the queries in a simple way.
Build incrementally upon it until we reach what has been decided.

##### 6) Library for better visualisation of the data : 27th july to 9th August

* Add data visualisation library.

##### 7) Test run of application : 10th August to 16th August

* test run of the application by the users
* working on bugs that are observed in the test run

I would use the remaining 10 days to :  
1) make the application deployable  
2) write remaining documentation  
3) finally deliver a clean, structured and documented code for the project.

####Other commitments
I will be able to devote the required time to the project during the entire duration as I have my university vacations during that period.

##Future work

I believe in `you own the code you write` philosophy and will take it upon me to maintain the product even after the project timeline. Moreover, I would try to remain in touch with other developers here and work on other projects as well.
I think software carpentry is doing a wonderful job. I believe in its mission and would like to get involved with it for a longer time.


##Open Source Development Experience
I have been involved with the open source world for quite some time now. Being a Google Summer of Code intern for the past two years, I have a very good understanding of how the open source organisations work.

In the year 2013, I worked for Benetech, a non-profit company based in US. I worked on a product that has been developed to help the vision-disabled people to read ebooks.
It was the first time I worked with an organisation on such a large scale. It was an enthralling experience for me to see my code being used by people around the globe.
That was the reason I chose Mozilla for my next year internship (2014) . I worked on adding the Text to Speech API to Firefox, which was not present till then.

Here are the links to my projects. The code has been shipped at Benetech and ready to ship at Mozilla.  
[Benetech project](http://www.google-melange.com/gsoc/project/details/google/gsoc2013/itsyash/5885170347409408)  
[Mozilla project](http://www.google-melange.com/gsoc/project/details/google/gsoc2014/itsyash/5870670537818112)

I have also played with gnome-shell source code and fixed a couple of bugs there. Apart from that, as an active
member of the Open source development Group of my University, I have been involved in organising various events in
the campus.


###Other Professional Experience

Research and Development Intern with Works Applications in Tokyo (2014 summer) . I built a web application that
serves as a search engine on the Test Automation Repository. The entire application was built from scratch in two
months. The product was deployed at the end of the internship. Technologies used were Java Web API's and
Bootstrap.


##Academic Experience

I am currently pursuing my B.Tech in Computer Science and Engineering at IIIT Hyderabad, India.

To enhance my knowledge on my interests, I have taken up courses such as Machine Learning, Information Retrieval
and Extraction, Software Engineering, Database Systems and Internals of Application servers.
Some of my projects are :

* A scalable and efficient search engine on 40 GB Wikipedia corpus.
* Predict interestingness of an article using sentiment analysis of tweets.
* Discover patterns in the given data to find various fusion methods for the purpose of building a machine learning model out of it.
* A standalone application for a restaurant that included designing a database schema for the restaurant and performing queries on it.


##Why this project?

Firstly, the reason of choosing Software Carpentry for my summer project is that I strongly believe in the
vision of the organisation and wanted a way to get involved with it. I think being a Google Summer of Code intern
this year is one of the best ways to accomplish that.
It would be a great learning experience for me to observe how the organisation works and I wish to remain
connected with it even after my project.

Now, the reason for choosing this project is that I am interested in implmenting what is required in the project
and I think I have the required experience for it. This project attracts me because it provides me a way to use my
skill set at an organisational level and if completed successfully, it would be a very helpful tool for Software
Carpentry in future.

I am confident that if I am offered this opportunity, I will be able to contribute positively. I wish to be in a
long and creative relationship with Software Carpentry, and this project would just be a start to it.

###Schema

I have assumed that the information related to a particular workshop may also be required.
If that’s not the case, I have designed the schema in such a way that it can easily be changed.


####Table 1: Result Entries

| id  | system_info  | upload_timing | workshop_id |
| --- | ------------ | ------------- | ----------- |

system_info can be expanded into two columns if we want (such as "Operating system" and "Version").

####Table 2: Package Requirements

| workshop_id | package_name |
| ----------- | ------------ |

This is for listing all the packages for a given workshop. We can also do this using table 1 and table 3, but I think this is more easier and efficient. We can always remove it if we think it's not needed.

####Table 3: Package Test Entries

| uid  | pname  | required_version  | installed_version  | diagnostic_information |
| ---- | ------ | ----------------- | ------------------ | ---------------------- |

uid-> foreign key to primary key(id) of Result Entries


Here are some types of sample queries that the system can handle.

1. What fraction of last month’s submitting hosts had Git < 1.8.

2. How many tests failed in a particular workshop.

3. How many tests failed in a particular workshop on Windows.

4. What fraction of failed tests were on OS X for a specific time period.

5. For a particular workshop, find number of failed version test for Git.

6. For a particular workshop, information about all the packages.

7. Total failed/passed checks for the past 10 workshops OR for the workshops held between a specific time period.

8. Similarly, total failed version tests on pytest package in the last 10 months.

9. Find the required versions of all packages of a specific workshop id.


So basically, for any query that asks for failed/passed tests or version tests, we should be able to add :

* a time range
* a particular operating system
* a particular workshop, and
* a particular package

