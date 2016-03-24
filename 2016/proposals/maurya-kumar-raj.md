# Add taxonomic name resolution to the EcoData Retriever to facilitate data science approaches to ecology

## Abstract

The EcoData Retriever is an automated software that is used for finding, downloading, and cleaning up ecological data files, and then organizes the whole data in a local database of your choice. The program cleans and restructures datasets in normalized form and formats these datasets according to the suitable standards of the database management systems.

This project deals with handling the problem of resolving scientific names that are stored in appropriate manner across various datasets. This software would enable it's users to conveniently combine different datasets that will be helpful in answering scientific queries. 

## Technical Details

EcoData Retriever is python based tool for downloading, cleaning up, and restructuring ecological data. The information about the repositories are stored in .script files which contain URLs to the files containing the datasets.

The Challenges which we are facing currently is that, the name of species are frequently redefined which makes it inappropriate to maintain consistency across the datasets and so it becomes difficult to combine multiple datasets together.

This project would include a system for sending a species name to one or more of the taxonomic name resolution services and determine the best name to appear in the generated databases. The project is composed of the following main components: 
* Accessing the resolution services.
* Determining the selection strategy.
* Updating the data model. 
* The user interface and the control flow.

The download operation is performed through Engine class, which uses urllib library to fetch the dataset file, and store it locally. This operation happens inside download_file() function in ./lib/engine.py file.

The general problem that we will be dealing would be to maintain the accuracy of datasets for which the names of species are redefined that raises the difficulty in combining the multiple datasets together.

## Schedule of Deliverables

This time line has been made keeping in mind that it will require work at least 40 hours per week. I am not planning on taking any vacations during the summer and would dedicatedly work on the project and also, I will continue to contribute afterwards.

### May 25th -  June 7th

### June 8th - June 21th

### June 22nd - July 5th

### July 6th - July 19th

### July 20th - August 2nd

### August 3rd - August 16th

### August 17th - August 21th 19:00 UTC

## Future works

* I will continue to work on the Retriever Project and finish some feature requests and I will be involved in fixing some bugs in Retriever.
* Adding more reliable name resolution APIs.
* Improving the accuracy of the best name deciding algorithm.
* Bug fixing the existing and adding more engines for different formats of data. For example - JSON, XML etc.

## Open Source Development Experience

I have worked on few open source projects like Google's tensorflow that deals with Deep Learning and involves the Machine Learning concepts. I also have contribution in fixing bugs in Mozilla community at bugzilla and also contributed as Firefox Student Ambassador.

I have also worked with a start-up from IIT Kanpur,SIDBI Incubation Center, (India) which involved the development of Dynamic UI graph using plot.ly API for Python and Java Script for testing and plotting the large experimental data sets.

I have been the member of NumFocus community and supporting it as a volunteer.

## Academic Experience

I am currently a 3rd year (B.Tech) Computer Science and Engineering student at University Institute of Engineering and Technology, Kanpur, India. I have done some academic projects on database management systems. I am a Free Open Source Software (FOSS) enthusiast and like to contribute, collaborate for the same.

This project deals with implementing and automating the process of accessing the data with the taxonomic names resolved. The project requires the knowledge of Python and database systems like MySql, SqLite.
I have worked on similar projects that had the same requirements and skill set.

I also have interned at some research organizations like Indian Institute of Science Education Research (IISER) Kolkata, UNESCO.
Apart from these I have also interned at start-up companies which includes:
* Absentia Virtual Reality
* Kanopy Techno Solutions
* Pollinate Energy
* Pariksha.co

## Why this project?

While going through the list of organizations and their ideas pages, I was actually looking for the projects which involves the extensive use of database systems, manipulating the large datasets accordingly and I found this project more suitable as well as favorable to me as it involves the usage of almost 80% of the skill sets and technologies that I am already familiar with. Also, my academic as well as non academic projects involved the processing of large data sets using sophisticated database systems.

I want to be involved in this project because it could help scientists and researchers to do their research and will save their precious time. I will be glad to contribute to this project and help others through this open source projects. This project will be a stepping stone for expanding my experience, skills and abilities as well.

## Contributions and ideas

Concurrent access of the SQLite database:
https://github.com/numfocus/gsoc/issues/120

## Appendix
