Improving reproducibility in science by adding provenance tracking to the EcoData Retriever


Personal Details


Name:- Prabh Simran Singh Baweja
Senior Year, B.Tech in Computer Science and Engineering, IIIT Hyderabad, India.
Email Id:- prabhsimransingh.baweja@gmail.com


Abstract


The adoption of provenance among computational scientists is low, because most existing systems require them to adopt a particular tool set in order to benefit from their functionality, such as the requirement to use a particular programming language, operating system, or a workflow engine. The EcoData Retriever is a Python based tool for automatically downloading, cleaning up, and restructuring ecological data. Most of the steps related to data acquisition and manipulation are either done manually, or using one-off scripts. Due to this, the process is highly arbitrary and not reproducible. The EcoData Retriever is unable to keep track of the work that has been completed, and therefore fails to support revision and reproduction of workflows. Thus, the need to integrate a provenance library or build a module to track the history of data is pressing. The aim of this project is to accomplish the same, making the EcoData Retriever more efficient and reliable for scientists.


Technical Details


The project could be divided in three parts :-


1. Analyze the feasibility of existing provenance libraries versus an in-house solution


The first step towards the project is to determine a provenance solution that suits our requirements and could be efficiently ported to work with EcoData Retriever. Some of the options are as follows:


1. Core Provenance Library: Core Provenance Library is designed to run on a variety of platforms, work with multiple programming languages, and be able to use a several different database backends. An application could use the library's API to disclose its provenance by creating provenance objects and disclosing data and control flow between the objects. The library would take care of persistently storing the provenance, detecting and breaking the cycles, and providing an interface to query and visualize the collected provenance.
2. Dublin Core : The Dublin Core Schema is a small set of vocabulary terms that can be used to describe web resources (video, images, web pages, etc.), as well as physical resources such as books or CDs, and objects like artworks. 
3. If none of the above libraries are deemed suitable for the project, we would rely on designing an in-house module to support provenance in EcoData Retriever. The requirements for the same as highlighted below:
1) Build a module to create an object with the metadata required for maintaining revisions.
2) Build a module that associates a directed acyclic graph structure for each such object, and computes “deltas” for each change recorded in the file, along with a time-stamp, and the version of Retriever used.  There are several popular algorithms to compute “deltas” (or changes), achieving high data compression and efficiency in retrieval. Some of them are XDelta (http://xdelta.org/) and OpenVCDiff (https://github.com/google/open-vcdiff)
3) Each node in the DAG is stored with the corresponding changes/deltas, and other metadata is stored in a JSON schema for future retrieval.
4) The last step is to build a module, to revert to the corresponding compression algorithm, when required and thus, support full reproduction of old analyses even if the data/code changes in the future.

After the thorough analysis and evaluation of these options, with the valuable inputs from the mentor and community, it would be possible to move to the next part of the project.

      2.   Integration of provenance library/module
        
        At this stage, we would have a clear picture of what needs to be implemented. 


A).  If we are able to find a provenance library that can be integrated, then it’s various features need to be taken care of while integration. We can use indexing techniques to store the pointers to the files that have been downloaded. The file pointers to original files, can be sorted according to their names to reduce the time of search. The modules to be built are:
1) Attach/detach from the database backend
2) Create and Search for provenance objects
3) Manage the control flow/pipeline. This will help support full reproduction.
4) Add custom properties

B.)   The second option is to implement it from scratch, as described in the above section. In this case, we would develop the compression/delta module, file indexing module, search/lookup and create module, and Pipeline Manager module.

       3.    Data retrieval and processing pipeline
        
        The basic docker that has been built will be useful for checking out the original code as required. Given the availability of correct version of EcoData Retriever and the signature of the methods run on the original data (that is given by our provenance module), we will be able to retrieve a full workflow and also possibly visualize the changes in each step, if required.


Schedule of Deliverables


Before 22nd April
   1. Study the code base of EcoData Retriever, in particular talk to the members of the community to recognize the existing one-off scripts and kinds of manual changes being currently made.  
   2. Intensive feasibility study of provenance libraries and module. 
   3. Discussion about (2) with the mentors.
23rd April - 22nd May (Community Bonding Period)
   1. Discussion about (2) with the mentors. 
   2. Finalizing the library. 
   3. If library not found, and we go ahead with our own implementation, Design a schema (UML and Flow diagrams) of the solution proposed above. Discuss and arrive at the architecture with help of mentors. 
25th May - 7th June (Coding/Integration)
   1. Start to work on integration of the library with the Retriever.
   2. Implement the indexing technique.
   3. Basic features of the provenance library need to be integrated. (Probably, Modules (1) and (2) above)
   4. If library not found, write a basic archiving algorithm, and build the DAG, metadata and surrounding modules for the same.
8th June - 21th June (Testing)
   1. Testing the basic features implemented in previous week. 
   2. Bugs to be detected and rectified in the early stages of development of the code.


22nd June : Mid Term Evaluation
23rd June - 5th July (Coding/Integration)
   1. Integrating the advanced and custom features of the provenance library.
   2. If implementing from scratch, develop the algorithm for computing “delta”, and adding JSON file support.
6th July - 12th July (Testing)
   1. Testing above features, identify bugs, and fix them. The rest of the week will be used as a buffer to make sure there is no lag.
12th July - 20th July (Coding)
   1. Build the Data Pipeline and integrate the PipelineManager module to support full reproduction of old analyses even if the data/code changes in the future.
 20th July - 26th July (Integration Testing)
   1. Implementing various indexing techniques to store pointers to the files that have performed various tasks.
   2. Completing the integration part of the library.
27th July - 2nd August (Testing)
   1. Rigorous testing of the code with different scripts. 
   2. Checking if the library has been integrated accurately or the algorithm functions properly.
   3. Checking if the indexing techniques work without any errors. 
2nd August - 5th August (Buffer Period)
   1. Correction of errors, if any.
   2. Improvements and Refactoring of the code after careful scrutiny, resulting in smaller time and space complexity.
   3. Performing various tests to confirm 100% accuracy.
5th August - 16th August (Rerun the Retriver)
   1. Storing all the data vis-à-vis a particular version in the docker.
   2. Release all the code related to a particular version as and when required.
17th August - 21st August (Testing)
   1. Check carefully if everything you have written works perfectly.
   2. In case of errors, solve those bugs.


Future Work

The deadlines I have set for myself will be sufficient to complete this project. In the future, this project can be extended to build a visualization tool for changes made by EcoData Retriever. A GUI to assemble and execute a pipeline could also be built over the above modules.

I am equally eager to contribute to the other projects that EcoData Retriever is working on.  I wish to be in a long and creative relationship with Software Carpentry, through constant communication with the mentors, and this project would be a start.






Open Source Development Experience

I have been an active contributor to the open source community for a couple of years. I developed a few games for CodeCombat which have now been played by around a million users. 
   1. http://codecombat.com/play/level/sword-loop.
   2. https://github.com/codecombat/codecombat/issues/399
Apart from that, I am an ardent member of the Open Source Development Group in my university.




Academic Experience

I am a senior undergraduate at International Institute of Information  Technology, Hyderabad, India majoring in Computer Science and Engineering. 
Some of the courses that I have taken are:- Information Retrieval and Extraction, Structured System Design and Analysis (Project based), IT Workshop, Database systems, Cloud Computing, Software Engineering, Data Structures, Algorithms, Statistical Methods in Artificial Intelligence.

Some relevant projects:- 
   1. Wikipedia Search Engine
        Developed an efficient search engine on Wikipedia (42 GB). Search results obtained in less than 1 sec. Languages Used:- Python, C++. (~1k LOC)
      2)  Database Query Execution
              Execute a single user DBMS, from scratch, that can execute SQL CRUD Queries using DBMS page structures and relations.
      3) Community Detection in Social Networks
        Action and Content Based Community Detection on 58k active Flickr users. Used modularity maximization over unique features to incorporate user similarity as edge weights.
      4) Payment Gateway Interface
Analyzed requirements, designed schema and created a database using MySQL for an
online payment gateway. A query interface was also integrated using Web2py.










Why this project?


I’ve always been an ardent supporter of Open source and frequently make my academic projects and pastime hacks publicly available. This project, in particular, attracts me because it is aimed at developing a tool that would help accelerate research output for scientists who use this tool. I understand the motivation behind the project, and the importance of versioning, having learnt it the hard way myself.

Given my skills and previous projects related to indexing, experience with version control and familiarity with APIs and file systems, I believe I’m a strong match for this project. In the past, I’ve dealt with file systems in low level languages and understand their working. I’ve very comfortable with integrating APIs with existing web services, having done so for my Wikipedia project. I’m confident, having read more about existing provenance libraries and their underlying algorithms, that I’d be able to deliver this project. I hope to make a modest contribution to the scientific community by helping scientists reproduce existing research, in a more effective way.

Contributions
Check for and use system proxies for downloading files
https://github.com/weecology/retriever/pull/278.
Previous exchange of Ideas:
https://github.com/numfocus/gsoc/issues/2




References
https://code.google.com/archive/p/core-provenance-library/
http://dublincore.org/