# Categorical Color

## Abstract
Categorical information (non-numerical like gender, favorite food, etc.) is quite common in datasets that we analyze. However Matplotlib support for plotting data is available mostly for numerical data, with few direct APIs built for categorical data. This project attempts to build an API for plotting heat-maps and other scalarMappables using categorical data. It will also work on developing normalization and color-map APIs supporting categorical data as these will be dependencies for the main API. 
Currently, users wanting to plot categorical data with color support in Matplotlib must manually map their categorical data to numbers, creating non-existent links between classes and requiring a lot of additional effort. 

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Intermediate | Python, Data science | [@tacaswell][@story645] |

## Technical Details
With the widespread presence and usage of large datasets of varied nature, the analysis of categorical data has received much attention. Most machine learning, big-data analysis, and data-mining research and projects require the datasets available to be visualized at various stages of work. A variety of plotting techniques are utilized for this matter, and Matplotlib plays a prominent role as an essential tool for many researchers. Despite its diverse functionality with regard to numerical data visualization, Matplotlib is quite restricted in the domain of categorical data analysis. In most cases, user are compelled to manually map their categorical data to numerical data, thus introducing unnecessary relationships and patterns to the datasets, creating redundant data, and being withheld from the simplicity and ease of usage inherent to Matplotlib. 
This work hopes to tackle this issue to a certain extent. The Categorical Color Project will include two main tasks: building the heat-map and other scalarMappables API, and normalization and color-map APIs. Matplotlib currently has these frameworks set-up with functionality for numerical data. This functionality will be extended to support categorical data. The work of this project will be done solely using Python. Libraries used will include Numpy and Matplotlib. There will be no new (unused in Matplotlib previously) third-party libraries used in the course of this project. 
The work of this project will stem from existing development done categorical color support in Matplotlib issues [6889](https://github.com/matplotlib/matplotlib/pull/6889) and [7383](https://github.com/matplotlib/matplotlib/issues/7383). The starting point for the project will be extending the imshow function (matplotlib.axes._axes.imshow) to support categorical data. This mainly focuses on providing it unit knowledge with regards to non-numerical data. The approach followed here will be the initial basis for this extended support for categorical data. It will be used as a foundation to build the heat-maps API, and normalization and color-map APIs. 
Currently, the process of using matplotlib.pyplot for heat-maps and other scalarMappables includes the creation of a color-map followed by normalization of all available data to fit this color-map. Color-maps plotting functionality is threefold: sequential, divergent and qualitative. Considering the intrinsic discreteness of categorical data, the extension of the color-maps API will focus on its qualitative plotting ability. However the fundamental behavior of the color-maps API will require changes for handling categorical data as it is currently dependent on the presence of numerical scalar values in the data to be plotted. 
With regards to the normalization function, since this extension will require working with categorical data, the concept of normalization of data seems to be rendered useless. However, the possibility of integrating this into the APIs to be built will be evaluated. Also during the course of the project focus will be given to the currently available discrete normalization methods (matplotlib.colors.BoundaryNorm) which handle data in a different sense. 
Given this functionality, the final task is entwining all this functionality to allow the plotting of heat-maps and other scalarMappables. This will focus on providing units support for the existing functions with regards to categorical data, followed by integration of the existing frameworks (including those built previously in this project) to better support categorical data visualization. 
The completion of this project will require the creation of certain new classes for categorical data handling as well as extending of currently available classes and APIs.  

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**
* Working on categorical data support for imshow function 
* Self-implementation of previous work done to support categorical color for better understanding
* Setting up of project blog covering all major technical details
* Working on MEPs in Matplotlib Developers’ Guide, and Matplotlib issues related to normalization, color-map, and color-bar code. 

### May 29th - June 3rd
* imshow functionality
* color-bar support for imshow
* legend support for imshow

### June 5th - June 9th
* tests for imshow function extensions
* documentation for imshow extensions

### June 12th - June 16th
*framework outline for norm functionality extensions
* framework outline for cmap functionality extensions

### June 19th - June 23th, **End of Phase 1**
* norm and cmap functionality
* integration of norm and cmap 

### June 26 - June 30th, **Begin of Phase 2**
* tests for norm and cmap
* documentation for norm and cmaps

### July 3rd - July 7th
* framework outline for heatmaps extension

### July 10th - July 14th
* heatmaps basic functionality 
* color-bar and legend integration 

### July 17th - July 21th, **End of Phase 2**
* tests for heatmaps API

### July 24th - July 28th, **Begin of Phase 3**
* extension of heatmaps functionality to other scalarMappables
### July 31st - August 4th
* color-bar and legend integration for other scalarMappables

### August 7th - August 11th
* tests for API for other scalarMappables
* documentation for heatmaps and other scalarMappables APIs

### August 14th - August 18th
* testing of all code and bug fixing
* review all tests

### August 21st - August 25th, **Final Week**
* review of all code and documentation

### August 28th - August 29th, **Submit final work**
* code submission
* discussion of possible future developments on blog 

## Future works

## Development Experience

{{ Development Experience }}
https://github.com/matplotlib/matplotlib/pull/8371#event-1015235492
https://github.com/scikit-learn/scikit-learn/pull/8558#pullrequestreview-26737369
https://github.com/cltk


## Other Experiences

{{ Experience }}

## Why this project?

{{ Why you want to do this project? }}

## Appendix

{{ Extra content }}
 
 
