##Providing sparse data structures as an implementation detail of the core and enhancing their functionality

---

###Table of Contents


 1. Abstract
 2. Project Goals
 3. Technical Details
 4. Milestones / Schedule of Deliverables
 5. Future Works
 6. Open Source Development Experience
 7. Academic Experience
 8. Why this project
 9. About Me
 
---

###Abstract

Currently sparse data structures provided by pandas exist in the separate `sparse` module. This package has few users and is easy to overlook when developing new features due to its separation from the `core` module. I propose to change sparse into an implementation detail of the core data structures so that a user does not need to import different packages or classes if they want to use sparse functionality. 

As it stands now, sparse data structures are meant to be compatible with the general data structures they represent. Making sparse integrated with the `core` would make it easier to deal with compatibility and require less changes from users who want to change their code to use sparse.

The `sparse` module also has many issues that have yet to be resolved, as can be seen [here](https://github.com/pydata/pandas/issues?q=is%3Aopen+is%3Aissue+label%3ASparse). The errors make sparse more difficult to use and require workarounds. On top of this there are enhancements to sparse that have been requested on Github. Once the refactoring is finished, I plan to fix some of the more relevant bugs and implement the enhancements, in order to provide a wider range of features to sparse data structures. 

--- 

###Project Goals

The project goals are as follows:


* Refactor sparse to be an implementation detail:
	- Add the functionality to the `core` data structures and retire the `sparse` module (probably keeping it around for legacy reasons)
	- Write tests to ensure that no functionality has been lost. This will involve refactoring and using `sparse/test` tests, and moving them to the `core` tests. Also I will most likely need to write my own tests if there is anything that the previous ones don't cover.
	
* Fix some existing bugs: 
	- Before working on enhancements, it makes sense to fix existing bugs.
	- Work on [issues](https://github.com/pydata/pandas/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+label 		 		%3ASparse+label%3ABug) labelled 'Bug', to generally improve functionality and fix errors.
	- Bugs to fix on Github: [here](https://github.com/pydata/pandas/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+label%3ASparse+label%3ABug) -  these will likely change by the start of gsoc.
			
* Implement enhancements:
 
  - Enhancements to implement: [here](https://github.com/pydata/pandas/issues?utf8=✓&q=is%3Aopen+is%3Aissue+label%3ASparse+label%3AEnhancement), if there is not enough time to for them all, I will choose a subset to implement. 
  - This section will involve writing tests for the enhancements that will be implemented.




--- 

###Technical Details

The first stage of the project will be merging the `sparse` data structures into the core ones. This will involve adding options to the data structures indicating that the object should be implemented sparsely i.e.:

`df = pd.DataFrame({'A': [1,2,np.nan,np.nan], 'B': [np.nan,np.nan,7,np.nan]}, sparse=True)`

The example above does not explicitly state which value should be ignored as np.nan is the default value for this [as noted in the docs](http://pandas.pydata.org/pandas-docs/stable/sparse.html). As with the sparse classes the user will be able to specify what the ignored value should be, the `sparse_fill_value` option will be used for this. 

Also there should be a method to convert an existing object into a sparse/dense implementation i.e:

`df.sparsify(sparse_fill_value=0)` or `df.densify()`

Some of the functionality will be placed in internals classes such as `SparseBlock` and `BlockManager` which are used internally by the data structures such as `DataFrame`, `Panel`, or `Series`. These internal classes are defined in the `pandas/core/internals.py` module, and a lot of the implementation will be done there as well as in the data structures themselves.


At the moment the `sparse` module has its own tests, during the refactoring period of the project, the tests will be changed to operate on the new implementation and moved to `core/tests`.

When it comes to implementing the enhancements, as a general rule any new feature I will implement will have added tests, if the existing ones do not already cover it.


--- 

###Milestones / Schedule of Deliverables


During the community bonding period I will do further research on the Pandas code base, as well as refine my understanding of the necessary enhancements.


####Milestone 1:

 **Make sparse an implementation detail**
 
  Start Date: *25 May*<br/>
  End Date: *5 July*
 

- I will still have exams at university until 6th June therefore I will not work at my fastest during that period. I do have quite a few days between exams however, and therefore will have time to work on the project.

 	**Schedule:**
 	
 	- May 25th - June 7th:
 		- Add functionality to the internals classes, in order to support sparse behaviour.
 	- June 8th - June 21st:
 		- Implement sparse data structures using the new functionality in the `internals`
 	- June 22nd - July 5th:
 		- Submit mid-term evaluation before 27th
 		- Refactor and create tests for data structures using sparse

####Milestone 2:

 **Fix existing bugs**
 
 Start Date: *6 July*<br/>
 End Date: *19 July*

- Some of the bugs listed on Github will remain an issue after the conversion, this period will be dedicated to fixing them.
- The work here will involve reproducing the bugs, tracking them down and coding the fixes for them. This section should be a lot shorter than the other two as there will be less planning/design choices involved.  
 
 	**Schedule:** 
 	
 	- July 6th - July 19th:
 		- Select the most relevant bugs from the list and fix them.
 	
 
####Milestone 3:
 
 **Implement Enhancements**
 
 Start Date: *20 July*<br/>
 End Date: *21 August*

 
 - Once sparse has been refactored and bugs have been fixed, I can focus on making enhancements and 
 
 	**Schedule:**
 	
 	- July 20th - August 2nd:
 		- Select enhancements to implement and start implementing them. For every enhancement, will write tests
 	- August 3rd - August 16th:
 		- Continue working on adding enhancements and testing them.
 		- Begin writing mentor evaluation. 
 	- August 17th - August 21st:
 		- Finish and submit mentor evaluation.
 	
---

###Future Works

After the project I would like to evaluate the success of refactoring sparse, and contribute to Pandas in other areas. I would look through any user feedback Pandas gets, and try to determine if more users are using sparse implementations, and whether they find it more usable than previously. As for other contributions, another interesting idea I found on the GSOC page was the enhancement of Panel operations. I would be interested in doing some work in this area later on.

---


###Open Source Development Experience

I have worked in research settings during summers, as well as contributing to open-source projects related to my last summer job, specifically:

 - [Pandas](https://www.github.com/pydata/pandas) (Pull request open, tests pass):
 	- Added xz compression in `to_csv` of a DataFrame. -- [#12668](https://github.com/pydata/pandas/pull/12668)

 - [SequenceServer](https://www.github.com/wurmlab/sequenceserver):
 	- Commits: [here](https://github.com/wurmlab/sequenceserver/commits?author=terfn)
 	- Fixed various issues both on back-end and front-end.
 	- Added a background thread to remove files
 - [Bionode](https://www.github.com/bionode/bionode-ncbi)
 	- Commits: [here](https://github.com/bionode/bionode-ncbi/commits?author=terfn)
 	- Added some testing to improve code coverage
 	- Implemented a wrapper for an API call used to download specific gene sequences and added tests to it.
 
---

###Academic Experience

I study Computer Science and am in the third year of my degree. During my studies I have worked on software development group projects, as well as taking classes on algorithms, data mining, and Big Data. I am treasurer of the EECS Society at our university, and have contributed to developing our Github hosted [page](https://github.com/qmcs/qmcs.github.io). 

---
###Why this Project

I enjoy working on software engineering projects, and debugging challenging problems. On top of this I am interested in getting involved in data science and already have experience in Python, therefore Pandas seems like a perfect fit for me to work on. Furthermore I think that providing functionality for sparse datasets would be beneficial for users who are dealing with such data and need to boost the performance of their code.

---  

###About Me

 * Name: Filip Ter
 * University: Queen Mary, University of London
 * Degree: Computer Science MSci
 * Year of Study: 3
 * Email: <a href="mailto:filip.ter@gmail.com">filip.ter@gmail.com</a>
 * Github: [terfn](https://www.github.com/terfn)
 * IRC: <a href="mailto:terfn@irc.freenode.net">terfn@irc.freenode.net</a>
 * CV: [here](https://www.dropbox.com/s/slaj1ppdq4vs4ud/Filip%20T%C3%A9r%20CV.pdf?dl=0)
 

Currently based in London, UK. Bilingual in English and Czech.
