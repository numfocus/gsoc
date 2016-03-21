# Title
Categorical axes

## Abstract
matplotlib 1.5 added direct support for plotting data frames.
However, there are still a few related tasks yet to be done.  An
important one is detecting when plotting categorical data
(i.e. enumerations) and updating the tick labels accordingly.

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Intermediate | Python, Pandas, Databases, Data science | [@tacaswell][] |

## Technical Details
More broadly, this task will involve designed new user-friendly APIs
to more automatically deal with certain types of data.

This work may include:

- implementing categorical axis in top of the mpl units framework (this may or
  may not be possible / make sense)
- implement improved / combined API for bar plots
    - this API should cover all of the styles currently exposed by hist
- implement proper 2D heat map API
    - based on imshow or pcolormesh?
    - move hinton demo into main API?
- ensuring that the interactive features are categorical aware
- sort out how / if multiple categorical artists should interact with
  each other. This may interact with the Compound Artists project.
- implement API for categorical color mapping
- possible interactions with
    - altair
    - seaborn
    - pandas
  
## Schedule of Deliverables

### May 25th -  June 7th

Understanding the mpl units framework and prototyping of implementing catagorical axis. 

### June 8th - June 21th
Fully implement catagorical axis, with tests, and start incorporating into APIs:
* barplot
* lineplot & scatterplot (maybe being able to pick up xlabel and ylabel natively?)

### June 22nd - July 5th
Implement proper 2D heat map API:
  - based on imshow or pcolormesh?
  - move hinton demo into main API?
  - make heatmap catagorically aware

### July 6th - July 19th
Implement API for categorical color mapping (earlier because seems easier than interactivty)
  
### July 20th - August 2nd
Start interactive features: 
* ensure catagorically aware
* work out how multiple artists should interact with each other

### August 3rd - August 16th
Continue work on interactive features if necessary, otherwise move on to future works. 

### August 17th - August 21th 19:00 UTC
**Week to scrub code, write tests, improve documentation, etc.**

## Future works
Possible interactions with altair, seaborn, pandas

Improve table by changing the API as such:
* axtable: behaves like a legend in that it's specifically attached to a plot
* table: standalone plot/axis object 

## Open Source Development Experience
 * Tutorials on Open Source Tools:
  - AMS 2016 Python Symposium Pandas/Matplotlib Tutorials: https://github.com/story645/ams2016_tutorials
  - PyCon 2014 Matplotlib Tutorial: https://github.com/story645/matplotlib-tutorial
 * GSOC 2011 (Climate Code Foundation):  
  - opensource embeddable climate data visualization tool
  - Server side numpy/scipy/matplotlib processing
  - https://github.com/story645/ccp-viz-toolkit
  - https://ams.confex.com/ams/92Annual/webprogram/Paper204786.html

##Academic Experience
* Research Projects:
 - Apply exploratory data analysis techniques to climate data
 - Long Term Forcast Evaluation: https://bitbucket.org/story645/libltf
 - River delta flood risk factor analysis
 - Machine Learning: https://bitbucket.org/story645/lmclus
* SIParCS Intern 2012 (National Center for Atmospheric Research) 
 - Ported the C-shell driver scripts of the POP diagnostics to Python
 - Created a web interface for the diagnostics using the Pyramid web-framework
 - https://ams.confex.com/ams/93Annual/webprogram/Paper223980.html
 
## Why this project?
I have taught matplotlib so often that I'm kind of ashamed that I haven't contributed and this seems like a good entry point. 
I work with a lot of catagorical data, especially climate data, and so I've written loads of boilerplate to plot it nicely in matplotlib and so think it would be awesome to have this all incorporated into the project. 


