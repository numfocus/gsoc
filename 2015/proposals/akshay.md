# Title

Add taxonomic name resolution to the EcoData Retriever to facilitate data 
science approaches to ecology.

## Abstract

The EcoData Retriever is an engine which automates the tasks of finding,
downloading, and cleaning up ecological data files, and then stores them in a
local database of your choice.The program cleans and munges the datasets and
formats them  before inserting it into your database.

This project will tackle the problem of resolving scientific names stored 
inconsistently across different datasets. This would allow the users to combine
different datasets and do interesting things.

## Technical Details

The main component of this project would be to build a system to resolve a 
species name by sending it to one of many Taxonomic name resolution services 
and processing the result to determine the best name. This can be done by 
incorporating the feature in the retriever directly or using the already 
existing library [pytaxize](https://github.com/sckott/pytaxize) in the retriever. 
I would propose to use pytaxize and use that in the retriver as this will 
ensure modularity and help develop pytaxize in the process so that it becomes a 
complete port of its R counterpart.This will involve a fair amount of work as 
different name resolution services have to be tested by running variety of data
and have to compared for the results. And a fair amount of thought has to be 
given for cases wherein two different services give two equally good results.

The second component of this project would be to restrict the running of 
resolution depending on the type of data. This can be done by adding a seperate
column in the scripts so that it contains the species name. We will make sure 
that the resolution will not run in the absence of the species name information in the table.

The third component involves updating the user interface of the retriever to 
falicitate users to perform Taxonomic name resolution.

Additional Components:
* Developing pytaxize
* Adding more datasets(There are a lot of dataset requests in the issues section)

## Schedule of Deliverables

### May 25th -  June 7th

Start working on pytaxize and test different Taxonomic name resolution services

### June 8th - June 21th

Implement these services in pytaxise and add unit tests.

### June 22th - July 5th

Build the control flow for running or not running taxonomic name resolution 
depending on the type of data and the users desires.

### July 6th - July 19th

Update the data model and the user interfaces to work with information about 
species and taxonomic name resolution.

### July 20th - August 2rd

Finish up UI. Make sure the entire thing works properly. Make sure that the 
code is well tested.

### August 3rd - August 16th

* Work on open issues in the retriever like adding new datasets and fixing bugs.
* Work on pytaxise to make it more robust.

### August 17th - August 21th 19:00 UTC

Week to scrub code, write tests, improve documentation, etc.

## Open Source Development Experience

I participated in last year's Google Summer of Code under SymPy. I made 
improvements to their existing Geometry module. The project was entirely object 
oriented in nature. [Here](https://github.com/sympy/sympy/wiki/GSoC-2014-Application--Akshay--Geometry-Module) 
is the link to my application. Apart from that I have also contributed to Scikit-Learn.

I have been using Python for over two years now and I am quite comfortable working 
with it. Apart from that I have a decent knowledge of Java and C++.I use Ubuntu
 14.04 as my work machine and have been using git for over a year now.

## Academic Experience

Hi, I am a third year undergraduate student at Bits-Pilani, India pursuing a 
dual degree in B.E(Hons) Electronics and Instrumentation and M.Sc (Hons) 
Economics. I have taken wide range of courses from Control Systems to Probabilty 
and also courses such as Applied Econometrics which requires analysing large amounts of data.

## Why this project?

I was going throught the list of project ideas and found this really interesting. 
This project will improve the functionality of the retriever and benefit the users of the system.

## Contributions to the project

* [281](https://github.com/weecology/retriever/pull/281) Started working on adding
  an XML engine to the retriever. This has helped me in gaining insights to the core
  codebase.

* [283](https://github.com/weecology/retriever/pull/283) Removed trailing white
  spaces in the entire codebase.
