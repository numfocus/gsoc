Port testsuite to pytest
========================


Abstract
========
Testing is crucial to the development of any software and MDAnalysis currently uses nose to test their code. 
Unfortunately, nose is no longer under active development so the community has decided to shift over to pytest.
The objective of this project is to implement this shift in a way that existing development work is not affected 
and to standardise and improve the existing test cases.


Current State
=============
Currently the testsuite consists of 8 modules that have a total of 4731 test cases. Running this testsuite with 
pytest (without any modifications) results in the following output:

`1789 failed, 2630 passed, 52 skipped, 1610 pytest-warnings, 26 error`

Also, it is to be noted that pytest misses out some (231 to be exact) test cases too. This is due to differences 
between test discovery rules of nose and pytest.




Technical Details
================

Part 1
------
This part will make the bare minimum changes needed to shift from nose to pytest and get TravisCI builds along with 
code coverage running as usual. This is being done in a single PR to avoid any weird coexistence of two testing 
libraries.

* Modify the code to make it discoverable by pytest (take care of `__init__` and inheritance problems)
* Port the 5 existing nose plugins to pytest -
    * Capture error (`capture_err.py`)
    * Cleanup (`cleanup.py`)
    * Knownfailure (`knownfailure.py`)
    * Memory leak (`memleak.py`)
    * Open files (`open_files.py`)
* Configure TravisCI and code coverage
* Take care of time limit for a single test case

Part 2 
------
This part will focus on the coordinates module and its tests. The coordinates module is a very important part of 
MDAnalysis and any bugs in it will result in error in further calculations. Currently the coordinates module has not 
been tested in a consistent fashion. Also all the reader/writers don’t necessarily follow the standard API.

* Modify Reader/Writer API for files in the coordinates module for to follow the standard API.
* Modify coordinates tests to use base Reader/Writer test classes. This will help to set a baseline that all 
readers/writers will have to conform to and at the same time avoid code duplication among test classes.
* Modify code to make use of pytest only features such as assert statements and fixtures. 

Part 3
------
This part will focus on the remaining tests and will modify them to use pytest features such as assert statements and
fixtures. This part should go relatively smoothly because I will be able to follow the patterns and conventions 
formed while working on the second part of the project.


Personal Information
===================

**Name**: Utkarsh Bansal

**Email**: bansalutkarsh3@gmail.com

**Github**: utkbansal

**Blog**: utkarshbansal.me

**Timezone**: UTC + 5:30

I am a 4th year undergraduate student pursuing my B.Tech. in Computer Science and Engineering.

Why are you interested in working with us?
-----------------------------------------

Have you used MDAnalysis for your research already?
--------------------------------------------------
No. I haven’t done any research.

Do you have any experience programming?
----------------------------------------
Yes, I have over 3 years of experience with Python and

Do you have any exams during GSoC or plan a vacation during the summer?
----------------------------------------------------------------------
Yes


# Port testsuite to pytest

## Abstract

{{ Abstract }}

## Technical Details

{{
Long description of the project.
**Must** include all technical details of the projects like libraries involved.
}}

## Schedule of Deliverables

### May 4th - May 29th, **Community Bonding Period**

{{ Delieverables }}

### May 30th - June 3rd

{{ Delieverables }}

### June 5th - June 9th

{{ Delieverables }}

### June 12th - June 16th

{{ Delieverables }}

### June 19th - June 23th, **End of Phase 1**

{{ Delieverables }}

### June 26 - June 30th, **Begin of Phase 2**

{{ Delieverables }}

### July 3rd - July 7th

{{ Delieverables }}

### July 10th - July 14th

{{ Delieverables }}

### July 17th - July 21th, **End of Phase 2**

{{ Delieverables }}

### July 24th - July 28th, **Begin of Phase 3**

{{ Delieverables }}

### July 31st - August 4th

{{ Delieverables }}

### August 7th - August 11th

{{ Delieverables }}

### August 14th - August 18th

{{ Delieverables }}

### August 21st - August 25th, **Final Week**

{{ Delieverables }}

### August 28th - August 29th, **Submit final work**

## Future works

{{ Future works }}

## Development Experience

{{ Development Experience }}

## Other Experiences

{{ Experience }}

## Why this project?

{{ Why you want to do this project? }}

## Appendix

{{ Extra content }}
