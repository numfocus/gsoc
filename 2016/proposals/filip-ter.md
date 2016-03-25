## Making the implementation of Sparse Data Structures independent and enhancing their functionality

---

### Table of Contents


 1. Abstract
 2. Project Goals
 3. Technical Details
 4. Milestones / Schedule of Deliverables
 5. Future Works
 6. Open Source Development Experience
 7. Academic Experience
 8. Why this project
 9. About Me
 10. Appendix
 
---

### Abstract

Currently sparse data structures provided by pandas exist in a `sparse` module under the `pandas` project. This package has few users and is easy to overlook when developing new features. I propose to change sparse to its own independent project named `pandas-sparse`, which will then be an external dependency of pandas. This will greatly enhance the modularity of pandas, as well as making it easier to develop new features for `pandas-sparse` by making it intuitive that these data structures have a different implementation. While making the module independent, compatibility with the `pandas/core` data structures will remain a top priority for `pandas-sparse`. 

The `sparse` module also has many [issues](https://github.com/pydata/pandas/issues?q=is%3Aopen+is%3Aissue+label%3ASparse) that remain unresolved. The errors make sparse more difficult to use and require workarounds. On top of this there are enhancements to sparse that have been requested on Github. In addition to refactoring the module I plan to fix some of the more relevant bugs and implement requested enhancements, in order to provide a wider range of features to sparse data structures. 

--- 

### Project Goals

The project goals are as follows:


1. Fix existing bugs:
	- The list of 'Bug' labelled issues is posted [here](https://github.com/pydata/pandas/issues?utf8=✓&q=is%3Aopen+is%3Aissue+label%3ASparse+label%3ABug) - will likely change before the start of gsoc
	- It is good to start with this for two reasons:
		1. It will mean that known bugs are not transferred to the new `pandas-sparse` module
		2. It will be a good opportunity to gain a deep understanding of `sparse` internals and implementation, which will make the refactoring into a separate module easier
		
2. Refactor sparse to an independent module: 
	- Change `sparse` to be an independent module named `pandas-sparse` which will be its own project with no dependency on `pandas`
	- This `pandas-sparse` will then be an external dependency of `pandas`, which is how pandas will provide sparse functionality
	- `pandas-sparse` will have its own tests, but the `sparse` module already has its own tests so this should not be too difficult to refactor
	
3. Implement enhancements:
	- List of enhancements to implement: [here](https://github.com/pydata/pandas/issues?utf8=✓&q=is%3Aopen+is%3Aissue+label%3ASparse+label%3AEnhancement), if there is not enough time to for them all, I will choose a subset to implement
	- This section will involve writing tests for the enhancements that will be implemented


--- 

### Technical Details

- Goal 1:
	
	Fixing the bugs will involve modifying existing code, making existing tests pass, and perhaps adding 	tests that didn't catch the bug. As usual when working on Pandas issues I will add a reference to 		the Github issue that it relates to in the code for the fix.

	For example if I were to implement a fix for bug [#9850](https://github.com/pydata/pandas/issues/9850) in `sparse/series.py`:

	```python
	  ...
	
	  	def to_frame():
	    	#GH9850
	
	    	#... fix bug here
	    	return SparseDataFrame(...)
	  ...
	```
     
- Goal 2:
		
	A new repository `pandas-sparse` will be created, hosted on Github under `pydata`. This will host the code for the project onwards. The module `pandas-sparse` will be have no dependency on `pandas` and will use `numpy` and `cython` internally. 
	
	Example Usage:
	
	```python
		from pandas_sparse import SparseDataFrame
			
		df = SparseDataFrame({'A': [1,0,2,0,0], 'B': [0,0,0,7,0],},
				     		fill_value=0)
	```
    
	The fill_value is equal to `np.nan` by default.

	As mentioned before `pandas-sparse` will be an external dependency of pandas, therefore I will implement conversion methods to pandas that will allow dense data structures to be converted to sparse and vice versa. These methods currently convert between `pandas/core` and `pandas/sparse`, the new ones will only be implemented in `pandas/core` as `pandas-sparse` will not include `pandas` in any way.

	Example usage with `Series` is show below. It will be almost the same for other data structures, this could also be an example of a test of the conversion methods:
	
	```python
	    import numpy as np
	    import padnas as pd
	
	    series = pd.Series(np.random.randn(5), 
	    		      		index=['a', 'b', 'c', 'd', 'e'])
	
	    ##Will return instance of pandas_sparse.SparseSeries
	    sparse_series = series.to_sparse()
	
	    ##The from_sparse method will convert from sparse data structures back to dense data structures
	    original_series = Series.from_sparse(sparse_series)
	
	    ##This test should pass if the methods are implemented correctly
	    assert_series_equal(series, original_series)
	
	```
	
	Snippet of what the implementation of the `to_sparse` and `from_sparse` methods in `pandas/core/series.py` will look like:
	
	```python
	   ...
	
	 	def to_sparse():
	        ...			
	
	        from pandas_sparse import SparseSeries
	        return SparseSeries(self.values, kind=self.kind,
	                            fill_value=self.fill_value)
	    ...
	
	    @classmethod
	    def from_sparse(sparse_series):
	        ...
	        return Series(sparse_series.sp_values, 
	                      index=sparse_series.index,
	                      name=sparse_series.name)
	
	    ...
	```

	The repository for `pandas-sparse` will contain the implementation of the sparse data structures, tests, and documentation. Most of the implementation will be taken from the existing `sparse` module of `pandas`, in addition to that it will be necessary to write code that implement some `internals` to make the module function independently, although it is largely independent already.
	
	Finally the project will use [Travis CI](https://travis-ci.org/) and [Codecov](https://codecov.io/) to track code coverage and whether builds work. Pandas already uses these tools so it should be easy for contributors to Pandas to add features to `pandas-sparse`.
	
	
- Goal 3:
	
	When it comes to implementing the enhancements, as a general rule any new feature I will implement will have added tests, if the existing ones do not already cover it. The `pandas-sparse` module will have its documentation, there I will note the implementation of new enhancements as I make them. As is the case with `pandas` and `xarray` I will include a `whatsnew.rst` file in which new additions will be reported.
	
	For example when I implement the enhancement [#667](https://github.com/pydata/pandas/issues/667), I will make the corresponding entry into whatsnew in the `doc/source/whatsnew` of `pandas-sparse`:
	
    
	```rst
		Enhancements
		~~~~~~~~~~~
			...
		
			- Multiple dtypes are now supported. 
			  Sparse data structures are no longer limited to 
			  only float (:issue:`667`).
		
			...
		
	```	

--- 

### Milestones / Schedule of Deliverables


- During the community bonding period I will do further research on the Pandas code base, as well as refine my understanding of the necessary enhancements.

- I will likely change locations during the summer at least once from London to Prague, this would mean a change of timezone from UTC to UTC+1. In both cases I will have access to the internet and a computer and be able to fully work on this, so the change of location will have no effect on my ability to work. I will give notice well in advance of moving.

- When implementing the bug fixes and enhancements, it will make sense to fix related issues in order. This will speed up the progress of implementing the fixes.



	#### Milestone 1:
	
	 **Achieve Goal 1: Fix Existing Bugs**
	 
	  Start Date: *25 May*<br/>
	  End Date: *21 June*
	 
	
	- I will still have exams at university until 6th June therefore I will not work at my fastest during that period. I do have quite a few days between exams however, and therefore will have time to work on the project. This is the least difficult milestone therefore the exams should not pose too much of a hindrance.
	
	 	**Schedule:**
		- May 25th - July 7th:
		  - Select the most relevant bugs from the list begin fixing them
		- June 8th - June 21st: 
	 		- Begin working on mid-term evaluation
	 		- Continue working on bugs and finish by the 21st
	
	
	#### Milestone 2:
	
	 **Achieve Goal 2: Refactor sparse to an independent module**
	 
	 Start Date: *22 June*<br/>
	 End Date: *19 July*
	
	- Create a new module`pandas-sparse` that will provide sparse functionality, that will be independent of `pandas`.
	Then make the new module a dependency in `pandas`
	
	   **Schedule:** 
	
	   - June 22nd - July 5th:
	      - Submit mid-term evaluation before 27th
	      - Begin work on refactoring sparse to an independent module
	   - July 6th - July 19th:
	   	  - Finish refactoring `sparse` into `pandas-sparse`
	      - Create tests by refactoring the existing ones and writing new ones
	      - Set up [Travis CI](https://travis-ci.org/) and [Codecov](https://codecov.io/) on `pandas-sparse`
	      - Transfer and update documentation
	 
	#### Milestone 3:
	 
	 **Achieve Goal 3: Implement enhancements**
	 
	 Start Date: *20 July*<br/>
	 End Date: *21 August*
	
	 
	 - Once sparse has been refactored and bugs have been fixed, I can focus on making enhancements to extend the functionality of `pandas-sparse`
	 
	 	**Schedule:**
	 	- July 20th - August 2nd:
	 		- Select enhancements to implement and start implementing them. For every enhancement, will write tests
	 	- August 3rd - August 16th:
	 		- Continue working on adding enhancements and testing them
	 		- Begin writing mentor evaluation
		- August 17th - August 21st:
	 		- Finish and submit mentor evaluation
 	
---

### Future Works

After the project I would like to evaluate the success of refactoring sparse to an independent module. I intend to look through any user feedback `pandas-sparse` gets, and try to determine if more users are using sparse implementations, and whether they find it more usable than previously. On top of that I would analyse numerical data such as number of users, and possibly promote the module by making a webpage for the module either on its own site or under [pandas.pydata.org](http://pandas.pydata.org).  Finally I would like to contribute to Pandas in other areas. For example, another interesting idea I found on the GSOC page was the enhancement of Panel operations. I would be interested in doing some work in this area later on.

---


### Open Source Development Experience

I have worked in research settings during summers, as well as contributing to open-source projects related to my last summer job. List of OS projects I contributed to:

 - [Pandas](https://www.github.com/pydata/pandas):
 	- Added xz compression for reading and writing CSV files -- [#12668](https://github.com/pydata/pandas/pull/12668) 
 - [SequenceServer](https://www.github.com/wurmlab/sequenceserver): 
 	- Commits: [here](https://github.com/wurmlab/sequenceserver/commits?author=terfn)
 	- Fixed various issues both on back-end and front-end
 	- Added a background thread to remove automatically generated job folders
 - [Bionode](https://www.github.com/bionode/bionode-ncbi):
 	- Commits: [here](https://github.com/bionode/bionode-ncbi/commits?author=terfn)
 	- Added some testing to improve code coverage
 	- Implemented a wrapper for an API call used to download specific gene sequences and added tests to it
 - [Drf-Extra-Fields](https://github.com/Hipo/drf-extra-fields) : Extra serializable fields for the Django Rest Framework
 	- Enabled Base64 encoded Image downloads, also tested my addition -- [#15](https://github.com/Hipo/drf-extra-fields/pull/15)
 
---

### Academic Experience

I study Computer Science and am in the third year of my degree. During my studies I have worked on software development group projects, as well as taking classes on algorithms, data mining, and Big Data. I am treasurer of the EECS Society at our university, and have contributed to developing our Github hosted [page](https://github.com/qmcs/qmcs.github.io). I also contributed articles to our society page in the first year of my studies. 

Other accomplishments:

- Participated in programming competitions, came second out of 18 teams in a university competition
- Participated in Facebook London Hackathon 2015
- Co-authorship of [paper](http://biorxiv.org/content/early/2015/11/27/033142): <em>Sequenceserver: a modern graphical user interface for custom BLAST databases related to my contributions to the project</em>  

---
### Why this Project

I enjoy working on software engineering projects, and debugging challenging problems. On top of this I am interested in getting involved in data science and already have experience in Python, therefore Pandas seems like a perfect fit for me to work on. Furthermore I think that providing functionality for sparse datasets would be beneficial for users who are dealing with such data and need to boost the performance of their code. 

---  

### About Me

 * Name: Filip Ter
 * University: Queen Mary, University of London
 * Degree: Computer Science MSci
 * Year of Study: 3
 * Email: <a href="mailto:filip.ter@gmail.com">filip.ter@gmail.com</a>
 * Github: [terfn](https://www.github.com/terfn)
 * IRC: <a href="mailto:terfn@irc.freenode.net">terfn@irc.freenode.net</a>
 * CV: [here](https://www.dropbox.com/s/slaj1ppdq4vs4ud/Filip%20T%C3%A9r%20CV.pdf?dl=0)
 * Timezone: UTC / UTC+1
 

Currently based in London, UK. Bilingual in English and Czech.

---

### Appendix

- Initial draft report and discussion: [https://gist.github.com/terfn/c63f7dc6039bdf47b63d](https://gist.github.com/terfn/c63f7dc6039bdf47b63d) (Revision 2)
- Pandas sparse [bugs](https://github.com/pydata/pandas/issues?utf8=✓&q=is%3Aopen+is%3Aissue+label%3ASparse+label%3ABug)
- Pandas sparse [enhancements](https://github.com/pydata/pandas/issues?utf8=✓&q=is%3Aopen+is%3Aissue+label%3ASparse+label%3AEnhancement)
