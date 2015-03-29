Name: Pankaj Kumar

Email: me@panks.me

Blog: [http://panks.me](http://panks.me)

Github: [http://gihub.com/panks](http://gihub.com/panks)

IRC Nick: panks

Country/Region: Chennai, India

# Title
Add taxonomic name resolution to the EcoData Retriever to facilitate data science approach to ecology

## Abstract
This project deals with implementing automating reconciliation of different species names as part of the process of accessing the data in EcoData Retriever.

## Technical Details
**EcoData Retriever** is python based tool for downloading, cleaning up, and restructuring ecological data. The information about the repositories are stored in .script files which contain URLs to the files containing the datasets.

The GUI lists the repositories and the user can download them by clicking on the download icon. Downloads are handled by the **DownloadManager** class. For every download requested information from the corresponding script file is added to the queue in **DownloadManager** class, which utilizes **DownloadThread** class, which is implemented on threading library, to fetch the dataset. 

The actual download operation happen is Engine class, which uses **urllib** library to fetch the dataset file, and store it locally. This operation happenes inside **download_file()** function in **./lib/engine.py** file. 

The problem we are facing right now is that the name of species are frequently redefined which makes it difficult to maintain consistency across the datasets and so it become difficult to combine multiple datasets together.

~~How we can handle it is by implementing a function in Engine class, which updates the species names, before storing it local file.~~

We can implement this functionality in **pytaxize**, and use it in retriever.

In function **download_file()** in Engine after we fetch the file using urllib and check for clean line endings ~~we can pass the file to another function which updates the species names before we save it locally.~~ we can make a call to pytaxize, which return the list of canonical names and we can pick among the returned list, based on score or a combination of score and other parameters as suitable.

For implementing such a function we would require a source for updated/accepted species name. As given in description of the project we can make a call to one or more of the existing web services to get that information. For example, as mentioned in the description one such service is **Taxonomic Name Resolution Service** (also **GNR**). One way we could handle it is by making a list of all the species names once we are done downloading the dataset in download_file() function, and make a call to TNRS/GNR API
passing the list and retrieving the accepted names for the given species. Once we get the updated list of species name we can make those changes in the dataset and then store it locally.

The service used for each dataset (.script file) can be different, so one way to go about this would be to include this information in the corresponding .script files itself, which can then be passed to the Engine class to be utilized for getting updated names.

Also, I feel this process to update the names would add a significant amount of time to the total download time of a dataset. So I feel it would a good idea to maybe include a checkbox along with each dataset in the GUI to enable user to skip this operation in case the user doesn’t want updated name set.

## Schedule of Deliverables

### May 25th -  June 7th
Setup the system, get completely familiar with the Retriever and pytaxize codebase. Finalize the implementation details with the mentor.

### June 8th - June 21st
Get familiar with any dependencies required for adding name resolution to pytaxize. Decide with the mentor on what sources to use for each of the dataset. Start implementing/updating the function in pytaxize and Engine class of Retriever.

### June 22nd - July 5th
Finish implementation of the function and test it on a small datasets to see if the files are stored locally successfully with updated names. Take feedback from the mentor make any changes required. Submit Mid-term report.

### July 6th - July 19th
Gather information on all the sources needed to fetch the updated species names for each of the dataset present with retriever currently, and the parameters to use for picking the names, and add that information to .script files.

### July 20th - August 2nd
Check if the current implementations works for all the dataset presents, introduce more parameters if required

### August 3rd - August 16th
Make any changes needed the GUI to incorporate this utility. Finalize the code and take feedback from the mentor.

### August 17th - August 21th 19:00 UTC
Code polish and merge back to the master. Submit final evaluation.

## Future works
Addition of dataset to Retriever

## Open Source Development Experience

**digiKam**:
 
Made batch processing of images parallel on a multicore system. Implemented image filter based of image ratio and few more factors. Mentored GSoC project, which involved writing plugin to upload images to cloud services like Google drive and Dropbox.

**Caligra**:

Implemented pdf-filter so that editing of pdf documents can be done inside Caligra Words.

**MOOL Project** ([http://dos.iitm.ac.in/projects/MOOL/](http://dos.iitm.ac.in/projects/MOOL/)): 
This project deals with porting the Linux kernel to C++, my part was to port the ext4 file-system to C++.

## Academic Experience
I am currently a **5th year Dual-Degree** (B. Tech + M. Tech) student at **Indian Institute of Technology Madras** in Computer Science and Engineering Department. I am a FOSS enthusiastic and I contribute to open-source projects in my free time.

This projects deal with implementing automating reconciliation of different species names as part of the process of accessing the data in EcoData Retriever. Which requires the knowledge Python which I’m familiar with and have also used Web Services via APIs in past in few of my projects. For example: ‘Yahoo! Just-Dial’ service, which I made as a part of Yahoo! HackU, which ended up with one of the winning projects. [https://github.com/panks/Yahoo-HackU--Yahoo-Justdial](https://github.com/panks/Yahoo-HackU--Yahoo-Justdial)

Also, I have participated in GSoC (2013) and similar programs before too, so this will not be my first time participation. During **GSoC (2013) I mentored two students** under KDE organization and the projects were successfully completed.

Apart from these I have also interned at Major software companies, which include Yahoo! and Microsoft, during my summer vacations.

## Why this project?
I liked this project while going through the list of organization and their idea pages. Also, my masters project deals with processing of data and learning from a given set of reactions and predicting new pathway for synthesizing other novel organic compounds. For which I’m using KEGG database to get the set of reactions and mol files of the compounds involved.

## Appendix

I submitted patches for following issues in pytaxize:

[#8](https://github.com/sckott/pytaxize/issues/8) Name resolution ([pull request](https://github.com/sckott/pytaxize/pull/19))

[#10](https://github.com/sckott/pytaxize/issues/10) Fix names_list() fxn that reads local csv files ([pull request](https://github.com/sckott/pytaxize/pull/18))

[#12](https://github.com/sckott/pytaxize/issues/12) Support multiple names for gnr_resolve() ([pull request](https://github.com/sckott/pytaxize/pull/19))

