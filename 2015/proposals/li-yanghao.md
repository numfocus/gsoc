# Add taxonomic name resolution to the EcoData Retriever to facilitate data-intensive approaches to ecology

## Abstract

The EcoData Retriever is a Python based tool for automatically downloading, cleaning up, restructuring ecological data files into standard formats, and then storing them in your choice of database management systems. It does the hard work of data munging so that scientists can focus on doing science.

This project focuses on the problem that the names of species in ecological data are constantly being redefined. By automating reconciliation of different species names as part of the process of accessing the data in the first place, it will become much easier for users to combine diverse datasets for specific scientific research.

## Technical Details

This project would build a system for sending a species name to one or more of the taxonomic name resolution services and determine the best name to appear in the generated databases. So the project mainly contains three components: accessing the resolution services, determining the election strategy and updating the user interfaces.

1. **Accessing the resolution services.** This could be built by using [pytaxize](https://github.com/sckott/pytaxize) to resolve species names with specific parameters. Current `pytaxize` supports resolving names by some services, such as [Global Names Resolver](http://resolver.globalnames.org/). We could also build out `pytaxize` to support more resolution services and have the Retriever use it.

2. **Determining the election strategy.** Since each resolution service may return many candidate names, we need to choose a best name according to the returned scores of services and some qualities of the candidate names. We also need to determine a few default standard data sources since it would be much faster than searching all data sources. For example, there are [181 sources](http://resolver.globalnames.org/data_sources) in `GNR`. My current idea is simple, which is to choose the name with highest average score from all data sources and the score also must be higher than a threshold. I will continue to talk with mentors to determine the final election strategy.

3. **Updating the user interfaces.** This project also needs to update the user interface to provide options interface for users before downloading a dataset. This interface contains some options, such as whether to use name resolution and choices of data sources.

## Schedule of Deliverables

###Before May 25th

Get more familiar with the Retriever codes.
Determine the election strategy.

### May 25th -  June 7th

Add functions to access more name resolution services in pytaxize.

### June 8th - June 21th

Finish adding functions in pytaxize and test them.
Write the module in Retriever to query names through pytaxize ports.

### June 22th - July 5th

Do mid-term evaluations.
Finish up the module to query names and process responses to choose the best name.

### July 6th - July 19th

Test on some datasets to evaluate the replacement results and make some updates of the election strategy.

### July 20th - August 2rd

Update the user interfaces for users to choose some options about the name resolution.

### August 3rd - August 16th

Finish the user interfaces and make sure it works well with back-end codes.
Test the entire code.

### August 17th - August 21th 19:00 UTC

Week to scrub code, write tests, improve documentation, etc

## Future works

Continue to work on Retriever Project and finish some requests or fix some bugs in Retriever Issues.
Improve pytaxize by adding more functions.

## Open Source Development Experience

I'm sorry, I don't have any open source development experience before. However, I'm always looking forward to have a chance to contribute.  

## Academic Experience

* I'm a fourth year undergraduate student at Peking University, Beijing, China. My major is computer science.
* I have implemented many course projects individually or in a team using C++, Python, JavaScript and other languages.
* I am an intern student in the Institute of Computer Science & Technology of Peking University and doing some research about computer vision and image processing with Python and Matlabzui.
* I have been an intern Software Development Engineer to finish some projects using Python in the Face Match Team of Hulu, Beijing.

## Why this project?

I have been using Python for over one year and I think I have the capability to implement this project. This project is also very interesting for me. The EcoData Retriever is very helpful for researchers to reduce their time and makes them focus on more important things. So I think it's very meaningful to improve and extend features of Retriever.

## Contribution to the project

[Retriever #274](https://github.com/weecology/retriever/pull/274) Fix [#206](https://github.com/weecology/retriever/issues/206) to change ownership of .retriever directories to the user who does the install.

[pytaxize #13](https://github.com/sckott/pytaxize/pull/13) Return [] for each query with no result returned instead of Raise NoResultException.

## Appendix
