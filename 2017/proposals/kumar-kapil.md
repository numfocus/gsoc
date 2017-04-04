### **Adding Python Interface and a Command Line wrapper for Data retriever in Julia**
 Kapil Kumar, NumFocus 2017

### **Abstract**

The Data retriever provides a command Line Interface in python and a CLI wrapper in R that automates the process of finding, cleaning, and standardizing  publicly available datasets. It then stores this data in a relational database or in a csv/Json/XML file.

Data Scientists working in python and Julia, constantly use the Data retriever tool to import datasets. This makes it important for us to provide a native Interface for Data retriever Tool in Python and a Command Line wrapper in Julia.

The aim of this project is to:

1. **Add native Python Interface for Data retriever:** Currently, if a python user wants to work with a dataset, the user has to download it using command line interface. After obtaining the data locally, the user can import that data, preferably into a data frame.  
The goal of this project is to extend the Data retriever so that users can interface with the tool using python. This will allow users to directly fetch any dataset using the Data retriever package from within python. For example
    ```python
      import retriever
      Dataframe = retriever.fetch(‘iris’,’csv’)       # Dataframe is a pandas dataframe object
    ```

2.  **Create a Julia Interface for the Data retriever** : With the increasing popularity of Julia among Data analysts, it has become very important for us provide Data retriever support in Julia. We would like to create CLI wrapper for Julia similar to [what we have for R language](https://github.com/ropensci/rdataretriever).  
This would allow Julia users to use Data retriever package functionality from within Julia. For example
    ```julia
      using retriever
      using DataFrames
      retriever.datasets() # list all available datasets
      Dataframe = retriever.fetch("iris")     # Return one/more DataFrame objects
      ...use Dataframe...
    ```



## **Technical Details**

This project can be divided into 2 parts:

* **Adding  native Python Interface for Data retriever**.

* **Creating a Julia Interface for the Data retriever.**


### **Adding  native Python Interface for Data retriever**

This task would involve:

1. Discussing with core Developers and listing all the functions that we would like to provide in Data retriever’s python package.

2. Cleaning up current  *\_init_.py* file.  
  * *\_init_.py* file in current  Data retriever module exposes a lot of functionality that we don’t want, when imported in python. This can cause problems with python Interface that we want to create.

  * Similarly, if we create a new module *interface.py*, adding all new functionality in this module and exposing it in *\_init_.py*, we might get a case of circular dependency.
    ```python
    # in  _init_.py,  to expose scriptlist function to users
    from retriever.interface import scriptlist

    # in interface.py
    from retriever import SCRIPT_LIST           # script_list is declared in _init_.py file

    # This creates a circular dependency as
    _init_.py => interface.scriptlist => _init_.py’s  SCRIPT_LIST
    ```
    So to resolve above mentioned problems, we would create a new module *initializer.py,* move all functions present in *\_init_.py* file to initializer.py and import it in *\_init_.py* file.
    ```python
    from retriever.initializer import *
    ```

3. Creating a new module *interface.py* where all the functionality needed for python Interface will be implemented. All required functions will be exposed to users, by importing them in _init_.py file. Some Key functions to be implemented in this module are:

  * Install: This function will be used to install a dataset specified by user to local databases or a csv/json/XML file.
    ```python
    def install(dataset_name, backend='csv', conn_file=None, db_file=None, debug=False, not_cached=False):
      ```

  * Download: This function directly downloads data files with no processing, allowing downloading of  non-tabular data.
    ```python
    def download(dataset_name, path='.', sub_dir=False):
      ```

  * Fetch: This function installs a dataset, load that dataset into a pandas dataframe and return the dataframe object.
    ```python
    def fetch(dataset_name, backend='csv', conn_file=None, db_file=None, debug=False, not_cached=False):
      ```

  As suggested by Core Developers, it would be a great idea to implement fetch function such that instead of loading all datasets into a csv file and then moving it to dataframe(as currently implemented in [R package](https://github.com/ropensci/rdataretriever)), we should load data directly from the database into a Dataframe.

  We can use SQLAlchemy to do so:
  ```python
  from sqlalchemy import create_engine
  import pandas as pd
  .....
  if(backend=='mysql'):
    db_engine =create_engine("mysql+pymysql://"+user+":" +password +"@"+host+"/"+db_name)
  .....
  pd.read_sql(tb_name, db_engine)
  ```

  * For sqlite:
    ```python
    if(backend=='sqlite'):
      db_engine = create_engine('sqlite:////'+db_file)
      #  db_file is a configuration file for sqlite
      ```

  * For postgres:
    ```python
    if(backend=='postgres’'):
      db_engine = create_engine("postgresql+psycopg2://"+user+ ":"+password + "@"+host+":”+port+"/"+db_name)
      ```
  * For mysql:
    ```python
    if(backend=='mysql'):
      db_engine =create_engine("mysql+pymysql://"+user+ ":"+password+"@"+host+"/"+db_name)
      ```
  * For csv:
    ```python
    if(backend=='csv'):
      return pd.read_csv(file_name)
      ```
  * For JSON:
    ```python
    if(backend=='json'):
      return pd.read_json(file_name)
      ```
   Libraries that can be used are SQLAlchemy, Pandas, PyMySQL, Psycopg2.

4. Testing: Tests specific to this python Interface will be added in our test suite. We will make sure that new development does not break existing codebase, by continuous testing and Integration. All tests in our test suite should pass.

5. Code Cleaning and Documenting "Python Interface for Data retriever".

I have developed a prototype for implementing "[Python Interface for Data retriever](https://github.com/kapilkd13/retriever/blob/packageRT/interfaceTesting/Instructions.txt)". Currently I have implemented Install, Fetch and List functions.  
Fetch function is implemented in such a way that it allows fetching data directly from databases instead of downloading to a csv file, as implemented in R Data retriever.


### **Creating a Julia Interface for the Data retriever**

This task would involve:

1. Making a clear outline of all the functions and their properties to be supported in the Julia interface for Data retriever.

2. Making a Julia package as a Github project, whose structure could be:

    ```julia
      retriever/                                 # this is retriever package
        src/                                     # this is the subdirectory containing the source
          retriever.jl                           # this is the main file/module
            module retriever                     # inside this file, declare the module name
            using DataFrames                     # using Dataframe packages
            export install, fetch, datasets      # export some functions of this package
            function fetch(...)                  # function definition
            ......
            end
      ```

3. Creating a Data retriever module in this package, all the functionality related to Julia Interface for Data retriever will be implemented in this module.
    ```julia
    module retriever
    using DataFrames
    export install, fetch, datasets
    function install(...)
    ......
    end
    ```

4. Some of the Key functions to be made in this module are:

  * Install : This function downloads a dataset in a relational database or an XML/CSV/JSON file.
    ```julia
    function install(dataset, connection; db_file= nothing, conn_file= nothing, data_dir='.', log_dir= nothing, debug= false, not_cached= false)
    ```

  * Fetch: This function loads a dataset in a DataFrame object and returns it.
    ```julia
    function fetch(dataset; log_dir=nothing, debug=false, not_cached=false)
    ```

  * Download: This function downloads non-tabular data.
    ```julia
    function download(dataset, path='.', sub_dir=false, log_dir=nothing)
    ```

5. Testing: Test cases will be added to test the functionality of individual functions implemented in the Julia interface for retriever.

6. Code Cleaning and Documenting Descriptions, License, Readme etc.

I have developed a prototype for "[Julia Interface for Data retriever](https://github.com/kapilkd13/JuliaRetriever)". Here I have implemented install, fetch and List functionality. Code is present in [retriever.jl](https://github.com/kapilkd13/JuliaRetriever/blob/master/retriever.jl) module and you can test it with [testing.jl](https://github.com/kapilkd13/JuliaRetriever/blob/master/testing.jl) by following instructions provided in the Readme file.

## **Schedule of Deliverables**

#### **May 1st - May 29th, Community Bonding Period**

* Getting completely familiar with Data Retriever package and R Data retriever’s codebase.

* Getting familiar with package structure in Julia.

* Discussing details of Timely reviews of my progress with my mentors.

#### **May 30th - June 5th, Week 1**

* Cleaning *_init_.py*, by moving its variables and functions to a new *initializer.py* module.

* Change all the references to *_init_.py’* s functions in codebase.

* Testing our changes, making sure that we didn’t break our existing functionality.

#### **June 6th - June 12th, Week 2**

* Defining structure of interface.py module. Defining function structure, their arguments and return value.

* Start implementing this functionality.

#### **June 13th - June 19th, Week 3**

* Implementing complete functionality of the interface.py module.

#### **June 20th - June 25th, Week 4,  End of Phase 1**

* Adding unit tests in our existing test suite to test the new functionality.

* Thoroughly testing our code to make sure it does not regress. We will make sure that all tests in our test suite pass.

#### **June 26 - July 2nd, Week 5,  Begin of Phase 2**

* Code cleaning, Documentation and Integration of Code.

* Packing and distributing our python interface package.

#### **July 3rd - July 9th, Week 6**

* Defining the structure of Data retriever module in Julia.

* Making a clear outline of its functions, their arguments and return type.

#### **July 10th - July 16th, Week 7**

* Coding functionality of *retriever.jl* module

#### **July 17th - July 23rd, Week 8,  End of Phase 2**

* Creating Test suite for Julia Interface.

* Testing and Bug removal.

#### **July 24th - July 30th, Week 9,  Begin of Phase 3**

* Code Cleaning. Documenting User Guide, Readme, License etc.

* Releasing **Julia Interface for Data retriever** package for public use.

#### **July 31st - August 6th, Week 10**

During this week I would like to work on:

* Adding debug, not-cached option in [R Data retriever](https://github.com/ropensci/rdataretriever).

* Improving the functionality of the Fetch function in R Data retriever, to directly fetch from database instead of downloading to a csv file and then loading.

* (optionally) Adding appropriate type stubs to allow continuous static type checking by tools like mypy.

#### **August 7th -August 20th, Week 11 and 12**

* A Buffer of two weeks has been kept for any unpredictable delay.

* I would like to use this period to solve bugs and add features to existing Data retriever implementation.

#### **August 21st - August 29th, Final Week**

* Submit complete work for final evaluation.

#### **Future work**

* After the end of GSOC 17, I would remain associated with Data retriever Team and would like to contribute on issues like "*better logging of errors or warning using _logging and allowing debug/quiet mode*" etc.

### **Deliverables**

At the end of this Project:

* We will have a native Python Interface for Data retriever.

* We will have a Julia Interface for Data retriever.

### **Development Experience**

I am familiar with Data retriever’s source flow and have contributed few bug fixes and feature additions:

* [#850](https://github.com/weecology/retriever/pull/850) Bug fix: fixing edit_json and delete_json functions

* [#846](https://github.com/weecology/retriever/pull/846) Feature: allow user specified message on Download

I have been able to develop prototype designs for both Python and Julia interfaces as a way to demonstrate my capabilities and desire to contribute to this project.  
With the current python interface on GitHub, I have been able to implement the key functionality like install, fetch, List_script etc. You can view the prototype design for [Python interface for retriever](https://github.com/kapilkd13/retriever/blob/packageRT/interfaceTesting/Instructions.txt) on Github.  
I have also developed a prototype for [Julia Interface for retriever](https://github.com/kapilkd13/JuliaRetriever) implementing install and fetch function that allows loading a dataset in a dataframe which can further be used by analysts working in Julia.

### **Other Experiences**

I am new to the open source community and have started contributing to open source projects like [CCextractor](https://github.com/kapilkd13/ccextractor),[ ](https://github.com/weecology/retriever)[CWLTool](https://github.com/common-workflow-language/cwltool). I have worked on some personal projects like  
* [goGeneric](https://github.com/kapilkd13/goGeneric)- Find Generic alternative of expensive medicines.  
* [UMIXPlayer](https://github.com/kapilkd13/UMIXPlayer)- A Music Player in Java.  
* [ComicDownloader](https://github.com/kapilkd13/comicDownloader)- Scripts to download comics like Calvin and Hobbes, Cyanide and happiness etc.

I am holding the post of Deputy General Secretary at Coding Club, NIT Delhi. I have organized numerous events like NITD Programming League, NITD Hackathon, Clash of Codes(1 and 2) and have managed them successfully.  
Here is a link to my [Resume](https://docs.google.com/document/d/1fEbxpcRrtTJoDm57RbHZkwdqp-k30c7tXAj9xuIDq3g/edit?usp=sharing).

### **Why this project?**

I first heard about Data retriever tool from my senior who used it in his research. Later, when I was viewing GSOC organization’s project ideas, I saw  "*Adding Python Interface and Command Line wrapper for Data retriever in Julia*" listed there.  
Adding native Python Interface and a Julia wrapper for Data retriever CLI would give access to the tools provided by the Data retriever in three major open source languages- R, Julia and Python for data oriented computing.

### **Appendix**

#### **Contact Information**


| Name     | Kapil Kumar                                            |
|----------|:------------------------------------------------------:|
| Branch   | Computer Science and Engineering                       |
| Institute| National Institute of Technology, Delhi                |
| Email    | kapilkd13@gmail.com , 141100043@nitdelhi.ac.in         |
| Github   | [kapilkd13](https://github.com/kapilkd13)              |
| IRC      | kapilkd13                                              |

#### **Other Obligations**

My summer vacation starts in the first week of May. After that I will be able to devote 8 hours a day (more if necessary) till the last week of July. After that, my college reopens in August but I will still be able to devote 5-6 hours a day for the project. I don’t have any summer plans other than GSoC.

Time Zone : Indian Standard Time (IST) UTC +5:30

Hours per week :
* **May 30-July 31** : 40-45 hours per week
* **Aug 1-Aug 29** :  30-35 hours per week
