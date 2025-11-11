# A scikit-bio-based bioinformatics file format converter

## Abstract
In bioinformatics, there are many defined file formats that represent very similar data. scikit-bio has a powerful I/O framework that enables users to load diverse formats into their relevant in-memory representations, regardless on input file format. It would be useful for many bioinformatics end-users (who are often not comfortable working with APIs) to have a file format converter for formats supported in scikit-bio. The goal of this project is to develop a scikit-bio Python API for file format conversions and then develop a simple web based conversion tool to for easy file upload and conversion. This project would also likely involve the development of additional file format readers and writers for scikit-bio, to increase the power of this application and of scikit-bio.

## Technical Details
scikit-bio is an open-source python package providing data structures, algorithms, and educational resources for bioinformatics.  It provides  a powerful I/O framework  called the I/O registry which enables users to load diverse formats into their relevant in-memory representations, regardless on input file format. The scikit-bio I/O registry provides a single entry point to all I/O using a simple procedural interface. Additionally, the registry dynamically generates an equivalent object-oriented API for any scikit-bio object that can be serialized or deserialized (i.e., written to or read from a file). Finally, the registry supports automatic file format detection via file sniffers. In this project, we will develop an API for file conversion based on this framework and in the second part of the project, we will develop a file conversion tool using our API. The file conversion tool will also allow us to upload unknown file formats and use in build sniffers from scikit-bio  to auto detect the file format before conversion.
We will use the Django REST framework in this project to develop the API.

## Schedule of Deliverables

### May 25th -  June 7th

**Read Documentation of Django REST framework.**
**Study the scikit-bio I/O registry**
**Setup Working Environment and create Django project and adding scikit-bio to it.**	

### June 8th - June 21th

**Extract I/O handling routines from I/O registry into a new class to handle conversion**
**Setup HTML forms of GUI conversion tool.**

### June 22th - July 5th
****
**Add support for a class to handle unknown file formats using the in build sniffers**	

### July 6th - July 19th

**Add utility  controllers to our new  I/O class and handling Exceptions.**	

### July 20th - August 2rd

**Implement API request methods and verbs**

### August 3rd - August 16th

**Implement API request methods and verbs and handling of Exceptions**	

### August 17th - August 21th 19:00 UTC

**Write tests for our API**

**Perform some format conversions**

**Write Documentation**

## Future works

This project will serve as ground work for a more robust API and conversion tool that will be enhanced to support more and even third party data formats not included in the scikit-bio package.


## Open Source Development Experience

I have contributed to open source projects in  the past where I developed a Data import tool for OpenMRS. I have also fixed bugs in other projects 

## Academic Experience
I am currently pursuing a B.Eng Computer Software Engineering at the university of Buea. I am equally a holder of a B.Sc in Microbiology 

## Why this project?

I have been coding in python for 3 years now and have build couple of applications using Django and play frameworks. I am very Interested in everything python and I founded a python club at my university Pygineers meaning python engineers. 

## Appendix

