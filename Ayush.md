#  Improve Distance Search Methods in MDAnalysis

## Opening Details

### Contact Information
* Personal Email : ayush.suhane@gmail.com
* School Email : ayush.suhane@alumni.ubc.ca
* github : https://github.com/ayushsuhane
* Cell Phone : +1(236)777-4212

### Brief Introduction
I am Ayush Suhane, a graduate student in the Materials Engineering Departemnt at University of British Columbia. Although I have been interested in computers from a long time, I have started contributing towards open source projects very recently. I have been involved in multiple development projects, few of which include developement of Phase Field solvers for microstructural evolution, Cellular automata solver for grain growth simulations, heat transfer and linear regression modelling for quality assesment for an steel industry.
Currently I am interested in Molecular Dynamics study of topological defects in materials. A major part of my project is related to the distance evaluations between different elements efficiently. Contributing to an open source project in a subject I am currently working and interested in, would be a great way of learning. With this project, I hope to learn a lot from the core developers, and other members as well. After the project, I  aim to constanty work towards contributing in open source projects with the acquired learnings during the course of the project.

## Abstract
MDAnalysis is a library built in python to allow fast and reproducible analysis from the output of Molecular Dynamics(MD) studies. An independent library, which can deal with multiple extensions of different MD output along with powerful inbuilt analysis algorithms is a powerful tool for science scholars.  
With the capability of multiple MD codes to easily handle milions of atoms, a major roadblock to analysis of this vast amount of data corresponding to positions of each atoms at every timestep is the time to evaluate pairwise distance between multiple atoms. Almost every operation requires the distance between the pair of atoms, fast calculation of pairwise distance is of utmost importance. Multiple basic analysis functions like Radial Distribution Function, Contact Matrices, depepend very heavily on fast distance evaluations. 
Apart from naive approach for pairwise calculations which scale as O(N^2), other forms of data structures like KDTree, Octree are sugested for faster calulations based on the requirements. Based on the MDAnalysis, two use cases are identified as highly used in majority of the analysis algorithms. 
The goal of the project is to identify the data structure based on the requirements of the use case and implement in the MDAnalysis library along with clear documentations and test cases.



general classification of distance problem needs to be tackled in MDAnalysis is (1) all the pairs within a particular distance from each other (2) selection of particular atoms around a residue. The complications arise due to the periodic boundary conditions, triclinic cell, algorithmic complexity to store huge number of data with their position, distance calculations and memory and time requirement for data structure handling.  
The goal of the project is to analyze different datastructures and provide benchmarks for extreme cases handled by MDAnalysis with an objective to propose and if time permits integrate few already developed methods with MDAnalysis. 

## Technical Details
Distance calculations are one of the most important aspect of MD analysis and simulations. As almost every analysis technique requires distance between pairs an an input, it is necessary to have efficient data structures and algorithms to evaluate distances quickly. With milions of atoms in a single trajectory, it becomes redundant to reduce the processing time with in efficient algorithms. 
For MDanalysis, the use cases for distance evaluations can be classfied as:
* All the pairs of atoms within a particular distance from each other.
* Selection of a cluster of atoms around a residue.

Although both problems requires to evaluate distance between atoms, the efficient solution to both the problem is different. It is intuitive to understand the differences as one requires to evaluate the distance between all the particles while other requires few distance evaluations with its surroundings. It can be observed that the naive approach take O(N^2) time to evaluate distances. Certain data structures like binary and three dimensional quadtree exist to divide the complexity into bigger clusters whie simultaneously sorting into a tree. These data structures provide viable alternative to reduce the amount of time for distance calculations. These data structure requires O(N) time for building and O(logN) for evaluating pairwise distance. However, these data structures should be able to handle an additional constraint of Periodic Boundary Condition(PBC).Furthermore, depending on the symmetry, it is also necessary to handle the calculations based on a triclinic unit cell. 

![Triclinic System](https://imgur.com/a/34nyF)

MDAnalysis has already implemented a distance evaluation method under MDAnalysis.lib.distances method. This currenty has a parallel implementation for triclinic system for both with and without periodic boundary conditions. [src](https://github.com/MDAnalysis/mdanalysis/blob/develop/package/MDAnalysis/lib/distances.py)
Although not efficient, it has a clear and tranparent extension to multiple cores. A seperate code is also avaiable to calculate distances in PBC which is specifically developed to maximize the CPU output based on the architecture. However, it cannot be made a core dependency of MDAnalysis as it requires to be compiled in seperately depending on the architecture of the system. The code can be found here. [src](https://github.com/kain88-de/pbc_distances). SIMD is an attractive method to extend the PBC as well as intuitively scale naive approach. Another alternative is to use Cell list as can be found in Appendix F here [src](https://www.sciencedirect.com/science/book/9780122673511). An implementation of this method can be found here. [src](https://github.com/MDAnalysis/cellgrid). These methods are good alternative to be used for pairwise calculations within all the atoms. For the selection of cluster of atoms around a residue, other data structures like KDTree or Octree are more better suited. To reduce the memory, Octree are a better suited candidate for such 3D distance evaluations. However, KDTree has been implemented for few specific case in MDAnalysis, which can be found here. [src](https://github.com/MDAnalysis/mdanalysis/blob/develop/package/MDAnalysis/lib/pkdtree.py). One of the objective is to provide benchmark against the interspersed methods for use cases generally used in MD simulations. 
Due to its non-uniform structure, it takes a large amount of time to build the datastructure depending on the data. An alternative such as Octree is proposed as a better alternative, which promises to enhance the performance for the case of high density or clustered systems. As opposed to KDTree, OCTree requires less amount to build for a certain data due to its uniformity, along with faster indepth movement to child nodes for distance calculations. It also requires less storage space. Since the number of atoms are in millions, space to store the positions is also an important issue.
Since different methods are efficient for different density of atoms in a case as well as the requirement, a seamless transition from one to another method is viewed as an end goal, which this project will be able to initiate with the help of benchmarks against different attractive methods.
The main objective of the project is to ensure proper benchmark of already implemented methods like CellGrid, pbc_distances, distances, KDTree for different use cases. Furthermore, efficient OCTree will also be checked against the bechmark cases to provide information for an informed result. In the second half of the project, the chosen method will be transparently integrated with MDAnalysis, along with tests and clear documentations.



## Schedule of Deliverables

The goal for the completion of GSoC is to suggest a better alternative to improve the distance evaluation algorithm in MDAnalysis. This can be divided into three deliverales (1) Benchmark case for KDTree, CellGrid, pbc_distance, distance.py already implemented in MDAnalysis. (2) Implement OCTree and bechmark against previous methods along with test and documentation
(3) Replace the distances in the MDAnalysis with the selected method for pairwise calcuation with PBC, test and docs
(4) If time permits, one or two cases for selection using OCTree or extension of KDTree

### **Community Bonding Period**

Get more Familiar with the pbc_distances, Cellgrid method already implemented. Test the working of the code. Get a clear idea of SIMD, and OCtree implementation along with discussion with developers about the KDTree and implemetation strategy of different distance searching methods in the Global vision of MDAnalysis.


### **Phase 1**
(May 29 - June 23)  Submit PR with benchmark cases on CellGrid, pbc distance for cubic, triclinic system and for periodic and non periodic systems.
Another PR for KDTree Benchmark for leaflet case.

### **Phase 2**
(June 26 - August 25)
Implement OCTree for the required case of neighbour searching problem. Produce benchmark cases for periodic and non periodic system for the memory usage and time to build the trees
Finalize the method for both the use cases for MDAnalysis users. 
Submit a PR to implement another keyword in selections.py alogn with docs and tests

### **Final Week**
Submit another PR to provide Use case for contact maps and RDF using the implemented method .
At this stage you should finish up your project. At this stage you should make
sure that you have code submitted to your organization. Our criteria to mark
your project as a success is to submit code before the end of GSoC.



## Other Experiences


## Why this project?

I am active user of Molecular Dynamics. My current project has a necessity to evaluate the distance etween atoms. While searching for any library, I stumbleb upon MDAnalysis I think it would be a good contribution which I will be working for my own as well as contribute towards the future MD users. This project would also expose me towards the open source community, which would be a great experience and a good learning opportunit
