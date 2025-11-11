# Add taxonomic name resolution to the EcoData Retriever to facilitate data-intensive approaches to ecology

## Abstract

The EcoData Retriever is a Python based tool for automatically downloading, cleaning up, restructuring ecological data files into standard formats, and then storing them in your choice of database management systems. It does the hard work of data munging so that scientists can focus on doing science.

This project focuses on the problem that the names of species in ecological data are constantly being redefined. By automating reconciliation of different species names as part of the process of accessing the data in the first place, it will become much easier for users to combine diverse datasets for specific scientific research.

## Technical Details

This project would build a system for sending a species name to one or more of the taxonomic name resolution services and determine the best name to appear in the generated databases. The project is composed of five main components: accessing the resolution services, determining the election strategy, updating the data model, the user interface and the control flow.

1. **Accessing the resolution services.** This could be built by using [pytaxize](https://github.com/sckott/pytaxize) to resolve species names with specific parameters. Current `pytaxize` supports resolving names by some services, such as [Global Names Resolver](http://resolver.globalnames.org/). We could also build out `pytaxize` to support more resolution services and have the Retriever use it.

2. **Determining the election strategy.** Since each resolution service may return many candidate names, we need to choose a best name according to the returned scores of services and some qualities of the candidate names. We also need to determine a few default standard data sources since it would be much faster than searching all data sources. For example, there are [181 sources](http://resolver.globalnames.org/data_sources) in `GNR`. My current idea is simple, which is to choose the name with highest average score from all data sources and the score also must be higher than a threshold. I will continue to talk with mentors to determine the final election strategy.

3. **Updating the data model.** When using this name resolution, the data model is also needed to update with returned best names. I think we could add two columns, which contain "recommended species name" and "recommended score". "Recommended score" is determined by the election strategy with a value between 0 and 1. In this way, users could determine whether to adopt our recommended names.  

4. **Updating the user interface.** This project also needs to update the user interface to allow the user to control whether or not to use this taxonomic name resolution for a particular dataset. My current idea is to add an option button beside the "download button". When users click it, an interface will come out. Users could choose whether to use taxonomic name resolution and change the default selected data sources by checking different checkboxes. There is also an input box with a text indicating which column contains the species names. This text could be obtained either from the dataset script or filled in the box by users.

5. **Updating the control flow.** Before downloading, we should check if users choose to use taxonomic name resolution and whether the corresponding column exists. If so, we need to generate the final data with extra two columns.

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

Week to scrub code, write tests, improve documentation, etc.

## Future works

* Continue to work on the Retriever Project and finish some feature requests or fix some bugs in Retriever [Issues Page](https://github.com/weecology/retriever/issues). After I finish this gsoc project, I think I will have more ideas about how to improve the Retriever much better. 
* Improve `pytaxize` by adding more functions. Since `pytaxize` is a incomplete python port of the R package [taxize](https://github.com/ropensci/taxize), it is very meaningful to extend it by adding more APIs of different data sources.

## Open Source Development Experience

Although I don't have much open source development experience before, I'm always looking forward to have a chance to contribute more. 

* I have implemented some image processing projects in my research work and put them on the github, such as [NRSR](https://github.com/lyttonhao/NRSR) and [FH-Eigen](https://github.com/lyttonhao/FH-Eigen).
* I also begin to contribute to the Retriever and pytaxize by some pull requests.

## Academic Experience

I'm a fourth year undergraduate student at Peking University, Beijing, China. My major is computer science.

* I have implemented many course projects individually or in teams using C++, Python, JavaScript and other languages.
* I am an intern student in the Institute of Computer Science & Technology of Peking University and doing some research about computer vision and image processing with Python and Matlab.
* I have been an intern Software Development Engineer to finish some projects using Python in the Face Match Team of Hulu, Beijing.

## Why this project?

I have been using Python for over one year and I think I have the capability to implement this project. This project is also very interesting to me since it could help scientists to do their research. I will be glad if I could help others through open source projects. So I think it's very meaningful and useful to improve and extend features of Retriever.

## Contribution to the project
* [Retriever #274](https://github.com/weecology/retriever/pull/274) Fix [#206](https://github.com/weecology/retriever/issues/206) to change ownership of .retriever directories to the user who does the install.

* [pytaxize #13](https://github.com/sckott/pytaxize/pull/13) Return [] for each query with no result returned instead of Raise NoResultException.

* [pytaxize #27](https://github.com/sckott/pytaxize/pull/27) Delete temporary name list file in _gnr_resolve().

* [pytaxize #33](https://github.com/sckott/pytaxize/pull/33) No matching results warning should be checked in gnr_resolve instead of _gnr_resolve.
