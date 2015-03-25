# Title

Add taxonomic name resolution to the EcoData Retriever to facilitate data-intensive ecology

## Abstract

The EcoData Retriever is a Python based tool for automatically downloading, cleaning up, and restructuring ecological data. It does the data munging so that the users can focus on using the data.
In ecological and evolutionary data, the names of species are constantly being redefined - This makes it difficult to combine datasets. By automating reconciliation of different species names as part of the process of accessing the data in the first place it will be easier to combine diverse datasets and in new and interesting ways.

## Technical Details

The project would consists of the following components:

1. Querying the taxonomy name resolution service:
APIs of the name resolution services have to be used to fetch the possible names using Python. The name of the species would be the input to the APIs and their possible names would be returned. This will be a separate module for the purpose of reusability. As planned, as of now pytaxize would be used to fetch the possible names.

2. Deciding on the best name
From the possible names returned from different services, an algorithm to determine the best name has to be developed. This would include giving different weights to parameters like reliability of source, consistency with other services and other parameters which need to be thought of. 

3. Updating the database
Once the decision about the best name is made, this component will update the database with the best name and its location.

4. Building control flow
This component would allow the user to choose whether to run name resolution or not. Also, name resolution for the species should not run if the species name is not present in the database. This will act as a bridge between the UI-Database module and the Name Resolution module.


## Schedule of Deliverables

Before the coding period begins, I intend to do the following:
1. Get more familiar with the existing code base.
2. Finding the best name resolution services which are to be used.
3. Making the design decisions of various modules with the mentors.

### May 25th -  June 7th

Getting the list of names, by interfacing with the different APIs, that are to be given as input to the best name deciding algorithm.  

### June 8th - June 21st

Developing the best name deciding algorithm. 

### June 22nd - July 5th

Updating the database with the best name.

### July 6th - July 19th

Building the control flow option. 

### July 20th - August 2rd

Complete the control flow and start fixing bugs.

### August 3rd - August 16th

Make the code more efficient and robust with documentation. Fix more bugs.

### August 17th - August 21st 19:00 UTC

Week to scrub code, write tests, improve documentation, etc.

## Future works

1. Adding more reliable name resolution APIs.
2. Improving the accuracy the best name deciding algorithm.
3. Bug fixing the existing and adding more engines for different formats of data. For example - JSON, XML etc.
4. Adding more features to the user interface like giving the option to the user to update the names from a specific source.
5. Support for the existing code base.

## Open Source Development Experience

I'm afraid that I have no experience contributing to open source projects (this would be my first!) but have used many open source tools and modified their code to accomplish different tasks. This has also involved reading and getting familiar with moderate sized code bases and development practices.

I am currently working with Prof. Sharad Goel from Stanford University doing research in Data Science and collaborating with other researchers using GitHub.

I have also worked with a start-up from MIT Media Lab which involved modifying and tweaking open source tools and collaborating with other developers in the team using GitHub (The repository is private).

I also like to use git for version control and put up any projects that I do on my GitHub account.


## Academic Experience

Institute: BITS Pilani - Goa Campus, India
Majors: B.E.(Hons.) Computer Science, M.Sc.(Hons.) Chemistry
Year of Study: Sophomore

Coursework and projects are listed in my resume at - http://rajatagarwal.me/Resume.pdf


## Why this project?

Pursuing majors in both science and engineering has often motivated me to bring the two fields together. Given my development experience, I'm pretty excited about contributing to this project as it helps me use my engineering skills (Computer Science) benefit scientific research.