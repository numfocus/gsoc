#Integrating `pandas.Panel` and xarray Features

## Abstract

Pandas package excels at processing tabular models, especially 1-D and 2-D models with data structure `pd.Series` and `pd.DataFrame`. However, despite the specialized data structure for 3-D and N-D data, `pd.Panel`, pandas is not well-designed for high dimensional data processing. Xarray was then developed to compensate the weak part of pandas. Xarray package enabled statistic and mathematical analysis of high-dimensional data, and implemented several new features that are missing in pandas. However, some useful features in pandas were not implemented in xarray yet. Thus, my project will be focusing on porting features from pandas to xarray, and improving data structure conversion between pandas and xarray. By the end of my work, I expect pandas/xarray users can have specialized tools for different types of data processing, and migrate between pandas and xarray smoothly. 

## Technical Details

Most of my proposal is supposed to be carried out with current implemented features in pandas and xarray. For PCA part, to improve the performance, I might switch to C++ and Eigen3 library. 

## Schedule of Deliverables

###Milestone 1: Migrate features from pandas to xarray 
###Week 1 - 5, May 23rd - June 26th

I will have my coursework finished within the first two weeks
I will implement more multiIndex-related features and port features from pandas to xarray.

  - Week 1: Enable multi-datatype in `xr.DataArray` like `pd.panel`
  - Week 2: Enable groupby for one-dimensional variables / Make levels accessible as coordinate variables
  - Week 3: Based on [#702](https://github.com/pydata/xarray/pull/702) and according to [#719](https://github.com/pydata/xarray/issues/719), I will implement selection return objects with MultiIndex, and add `set_index`/`reset_index`/`swaplevel` to make it easier to create and manipulate multi-indexes.
  - Week 4 - 5: Port features from pd.panel to `xr.DataArray`, such as `prod`, `cumsum`, `rank`, and etc.. Ref [#791](https://github.com/pydata/xarray/issues/791)

###Milestone 2: Implement interface between pandas and xarray data structure
###Week 6 - 9, June 27th - July 24th

To ensure that data can migrate between pandas and xarray data structure smoothly, I want to spend time on optimizing the conversion between data types. I will check current conversion methods, and implement more features - from multi-indexed/ hierarchical-indexed `pd.DataFrame` to `xr.DataSet`; from xr.DataSet to 2-D pd.DataFrame with PCA.

  - Week 6: Test current conversion methods with newly added features in Milestone1 and fix existing bugs.
  - Week 7: Based on [#702](https://github.com/pydata/xarray/pull/702) and Milestone1, I am going to take one step further and implement conversion from multiIndexed `pd.DataFrame` to `xr.DataSet`.
  - Week 8 - 9: It might be useful to compress multi-dimensional metadata into 2-dimensional or 3-dimentional dataset and view in matplotlib. I am going to implement PCA method in xarray, and enable conversion from `xr.DataSet` to `pd.DataFrame` via PCA with certain data types. 

###Milestone 3: Port xarray features to pandas
###Week 10 - 11, July 25th - Aug 7st 

`pd.DataFrame` currently supports basic `.loc` indexing. I am going to introduce `xr.loc` back into pd.DataFrame to enable dictionary syntax for indexing. 
I will also check for other useful features in xarray and migrate back to pandas.

###Milestone 4: Clean-up and Wrap-up
###Week 12 - 13, Aug. 8th - Aug. 23th

Checked for and fix opening issues for xarray. 
Clean-up codes and finish documentations.

## Future works
I would like to continue working on pandas and xarray project, to make pandas and xarray better libraries.

## Open Source Development Experience
I have been using several open-source packages for my lab projects, while I just started to contribute to open-source projects. I fixed several bugs for Pandas package in Python, and contributed patches for Shogun, a C++ - based machine learning toolkit. 
I have gained a lot from my limited experience. First, trying to solve problems kicked me out of my comfort zone and drove me to learn new skills. Also, reading other developers' codes and talking to people interactively greatly improved my insights in coding. Meanwhile, I started to learn the rules for developing and maintaining giant projects. Thus, open-source projects development experience benefitted and will benefit me a lot.

##Patch samples: 
  ####Pandas:
    [#12614](https://github.com/pydata/pandas/pull/12614) Fixed bug: `pd.crosstab` `margins=True` ignoring dropna
    [#12650](https://github.com/pydata/pandas/pull/12650) Fixed bug: `pd.pivot_table` `dropna=False` drops columns/index names
  ####Shogun: (a C++ - based machine learning toolkit)
    [#3092](https://github.com/shogun-toolbox/shogun/pull/3092) Project-wise FLAG removal
    [#3096](https://github.com/shogun-toolbox/shogun/pull/3092) API: add mean computation to linear algebra library

## Informatics skills

###Python/Numpy/Pandas
  ####COURSES:
    Fundamentals of Computing Specialization (Coursera, License: 5ePAwImNEe)
    Bioinformatics Algorithms (Coursera)
  ####PROJECTS:
    Genome-wide Mitochondrial Targeting Prediction in C. elegans
    RNA-seq Analysis of MSAF-1 Mutant Gene Expression Profile in C. elegans 
    
###Linear Algebra/Statistics
  ####COURSES:
    Linear Algebra and Analytical Geometry (Undergraduate, grade 95/100)
    Probability and Statistics (Undergraduate, grade 99/100),
    Stochastic Mathematical Methods (Undergraduate, grade 98/100)
    Quantitative Genomics and Genetics (Graduate, Honor)
  ####PROJECTS: 
    S phase genes identification and pathway enrichment study via microarray and singular value decomposition (SVD)
    Genome-wide Association Study (GWAS) on Casual Polymorphisms Regulating Gene CG9186 Expression at Pupation Stage in Drosophila melanogaster

###Other skills: R, Matlab, Java, C++, bash

## Academic Experience

Sept. 2013 - present	
  Memorial Sloan Kettering Cancer Center
  Graduate Research Assistant,
  Cell Department, Laboratory of Dr. Cole Haynes

Sept. 2012 - present	
  Weill Cornell Graduate School of Medical Sciences
  Graduate student,
  Biochemistry, Cell and Molecular Biology Allied Program

Aug. 2008 - Jul. 2012
  Tsinghua University, China
  Undergraduate Student, School of Life Sciences, Major in Biological Sciences
  Rank: 6/68
  Graduate with Honor
  Distinguished Dissertation

## Why this project?

I have used the pandas package before in my lab project - deep sequencing data analysis. I also started to contribute to pandas by fixing bugs: `pd.crosstab margins` ignoring `dropna` and `pd.pivot_table` `dropna=False` drops columns/index names. 
I am interested in becoming a data scientist, in which career python and pandas are powerful and popular tools. I would like to join the development of pandas, because it helps me understand pandas better, as well as improves my coding skills and insights.
I chose to join the panel/xarray project because the project offers me the chance to review and study the general features and advantages/disadvantages of both pandas and xarray packages.

