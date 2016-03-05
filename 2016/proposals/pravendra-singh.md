# A survey responses visualizer application

Software Carpentry has been educating multidisciplinary researchers with the necessary computing skills to help them in their venture. It enables the same via workshops for different fields, community discussion between volunteers, and curated readings etc.

The number of people attending the workshops itself is sufficient enough to help us imagine the impact Software Carpentry Foundation has been making since its foundation.

![number-of-learners](http://software-carpentry.org/files/graphs/learners-2015-10.png)
[credits: software-carpentry.org]

## Abstract

Software Carpentry maintains a set of tools that help them manage their workshop events. For example, the project [AMY](https://github.com/numfocus/amy) is a workshop management tool for the organization.

There are *pre* and *post* workshop surveys also. They support in the proper event organization by collecting details about expected attendees for *pre-workshop* surveys and feedback about the event for *post-workshop* surveys.

The following proposal talks down an application which can "*Represent the survey responses in a comprehensible manner, assisting the workshop organizers (hosts) to plan the event according to the feedback from attendees*".

The application will be a relieve from the current cumbersome method we use for data aggregation.

It has an additional benefit here, as we (Software Carpentry) believe in openness, the resultant application can be open sourced for the great good of others who want to utilize the SurveyMonkey survey responses in an interactive fashion.

## Technical Details

The application will be powered by Django, as we also want it to work with AMY. It will use Plotly.js for visualizing different response types.

Going by the "*Object-oriented analysis and design*" approach for system design, we can categorize the application in the following sections.

###### 1. Visual representation of the survey questions

Both the type of surveys have multiple questions of varying classes. Ideally, a good visualizer should respect the inherent attributes of the dataset. That's why the application will employ suitable visualization for different types of questions.

According to this [explanation](https://github.com/numfocus/gsoc/issues/77#issuecomment-192500549), here is a list of suitable charts.

| Question Type  | Suitable Chart Type | Sample Question |
| ------------- | ------------- | ------------- |
| Multiple choice - Single answer  | Bar and Pie chart  | When are you taking this survey? |
| Multiple choice - Multiple answer  | Chord diagram  |  What is your domain of research/study? |
| Yes-No-Maybe type  | Horizontal Stacked Bar chart  |  How would you describe your ability to do the following tasks? |
| Matrix type  | Heatmap Matrix chart  | How would you rate your motivation to learn about these topics? |
| Text as an input  | Text-Cloud OR Table OR Semantic Analysis | In a few words, what is your most important reason for attending this workshop? |

###### 2. Response-Cache service

As we are already aware of the order of total responses we get from our attendees, having a "*Data Cache Layer*" is an intelligent solution. Rate Limitations exposed by SurveyMonkey API is another reason behind implementing a cache service.

The *Response-Cache Service* will be responsible for serving cached responses, and hence plots. It will leverage the *offline* mode of Plotly so that the caching service is only dependent on the API responses and idempotent in terms of generated plots.

The service will use *Heartbeat Mechanism* to invalidate cache content with respect to the API response periodically. Workshop identifiers (ID) will be used to collect and invalidate responses for a particular event.

**Interaction between SurveyMonkey API and Response-Cache Service**
```
           +-------------------+        
           |    SurveyMonkey   |        
           |         API       |        
           +--^-------------^--+        
              |  Heartbeat  |           
           +--v-------------v--+        
           |   Response Cache  |        
           |      Service      |        
           +-------------------+
```

This service consists of two elements "Cache Storage" [CS] and "Permanent Storage" [PS], using "Redis" and "RethinkDB" respectively.

**Response-Cache Service Internals**
```


          SurveyMonkey API
                 ^
                 |
                 |
           +-----|-------------------------------+        
           |     v                               |        
           |   -----                     -----   |
           |  | C S | <---------------> | P S |  |
           |   -----                     -----   |
           |                               ^     |
           +-------------------------------|-----+
                                           |
                                           |
                                           v
                                    Client Requests
```

If any change is measured in "Cache Storage" during the invalidation process, it is also replicated to the "Permanent Storage" which will be used to serve browser requests.

###### 3. Application's backend services

This section is responsible for all the Models to serve the client requests.

* Authentication system using GitHub login (optional)
* Model to retrieve data from "permanent storage" according to the selected workshop 


###### 4. Frontend services

It deals with the "Template Views" that will show the visualizations for a particular workshop, also, it will provide the interface to select a workshop.

###### 5. Documentation

I believe in the "README Driven Development", this is something I have learned from my seniors at SDSLabs. It's an ideal way of development if we are working in a team, giving the members an early viewpoint on the work.

The documentation for the application will be rich and expressive, therefore, it will be easy to extend the project for future developers.

###### 6. Test suits

Following the "Test Driven Development", test suits will be implemented along with the application.

## Schedule of Deliverables

I don't have any other work during the period of the program.

### April 22nd - May 24th

During this community bonding period, along with getting familiar with the working of the workshop event, I'll be talking to the event hosts about what kind of information they want to see in the dashboard to enhance their experience.

I would like to discuss with my mentors and finalize two things.
* Decide the final set of visualizations for every different question type, so that it will easy to just develop that part during the program.
* Understand the working and codebase of AMY to simplify its integration, once the application is developed.

### May 25th -  June 7th

* Initiate the application development by designing the schema for "Response Cache Service", for both "Cache" and "Permanent" storages.

### June 8th - June 21th

* Implement the data aggregation service (collecting responses from SurveyMonkey API) utilizing the previously designed storage schema.
* Enable the Heartbeat mechanism to implement the long-polling of API responses. 

### June 22nd - July 5th

* **Mid term evaluation**
* Implement the cache invalidation service of API responses in the "Cache Storage".

### July 6th - July 19th

* Develop the model to retrieve data from "Permanent Storage" using proper ETag headers to enable client-side cache, as explained in RFC-7232, "HTTP: conditional requests".

### July 20th - August 2nd

* Implement the authentication system (for workshop hosts)
* Implement the views to select a particular workshop, and to see its response details.

### August 3rd - August 16th

* Implement the javascript library to generate respective charts for the received data from "Permanent Storage".

### August 17th - August 21th 19:00 UTC

* **Week to scrub code, write tests, improve documentation, etc**.

Though, we will write test along with the development.
* **End term evaluation**

## Future works

To integrate the application with the project AMY, I would love to work on the project after the program ends.

I will be joining my new workplace in the late October, so it leaves me plently of time before that, to work on the same.

As I'm getting familiar with the project AMY during the community bonding period, I can directly jump in the development.

## Open Source Development Experience

I have been involved in the open source community since my freshman year at the institute, once I got the flavor of it. I have been using Git and GitHub for 3 years.

As a "Student Developer", I was part of the student organization [SDSLabs](https://sdslabs.co/), a student group that promotes technical inventions and open development on the campus.

I have worked as a "Technical Editor, Intern" with Plotly, where I have contributed to their technical computing blog, [Modern Data](http://moderndata.plot.ly). I have also implemented the open source project [octogrid](https://github.com/octogrid), and now I maintain the repository.

These are the resultant posts on the *modern.data* blog.
* [Gauge charts in R and Python](http://moderndata.plot.ly/gauge-charts-in-r-and-python/)
* [Community detection in your GitHub's following network using d3.js and igraph](http://moderndata.plot.ly/community-detection-in-your-githubs-following-network-using-d3-js-and-igraph)
* [Network Graphs in R](https://plot.ly/r/network-graphs/)

I am [@pravj](https://github.com/pravj) on GitHub where I open source all of my projects.

Here are some of the project I have worked on.

* [Doga](https://github.com/pravj/doga) - HTTP log monitoring console for Humans
* [octogrid](https://github.com/plotly/octogrid) - GitHub following network visualizer for Humans
* [slack-lens](https://github.com/sdslabs/slack-lens/tree/development) - A search backend for Slack using Elasticsearch and MySQL

I have experience developing [concurrent storage system](https://github.com/pravj/engine) using RethinkDB, I have developed it for my last project.

Both slack-lens and octogrid use GitHub's OAuth implementation.

I had received a Student Scholarship for GopherCon 2016 conference, for my open [source](https://github.com/pravj/puzzl) [projects](https://github.com/pravj/geopattern) in the Golang environment.

## Academic Experience

I am a senior Computer Science student at Indian Institute of Technology, Roorkee.
I have an interest in Data Science, Distributed Systems, and Programming Languages.

## Why this project?

I have a habit to go after and complete a thing, once I had spent a significant time on it. This same instinct made me participate in the 3rd GitHub Data Challenge with a [tiny idea](https://github.com/pravj/teamwork) I had in my mind.

Though, I didn't win it, but it pushed me into the beautiful world of data visualization. It was a proud moment to know that only 70-ish teams have submitted it.

Since then, I have worked on multiple successful projects involving data visualization.
* [Open Source Presence Infographic of Indian Startups](http://pravj.github.io/blog/open-source-presence-infographic/)
* [Jump in to ride all the Bangalore taxis, at once](http://pravj.github.io/blog/bangalore-taxis/)
* [Breaking into the Indian E-commerce](http://pravj.github.io/blog/indian-ecommerce/) [[Featured in "Tech in Asia"]](https://www.techinasia.com/talk/breaking-indian-ecommerce-space)

I have opted for this project because I feel more engaged with a software project when there are hidden insights in the underlying metadata OR it involves retrieval of data through little challenges, so that we can harness the insights for other good use.

Ultimately, the goal is to turn data into information, and information into insight.

Stephen Few has said - 
> "Numbers have an important story to tell. They rely on you to give them a clear and convincing voice."

## Appendix

##### Project Discussion
The discussion with my mentors about the project is available here, [Query about the survey visualizer project](https://github.com/numfocus/gsoc/issues/77).

##### Sample Code
[plotly/octogrid](https://github.com/plotly/octogrid): It's a visualizer ([Python package](http://pypi.python.org/pypi/octogrid)) that creates network graphs of the following-network for a given GitHub user. The package uses GitHub's OAuth authentication to cope with the rate limitation.