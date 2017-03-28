# Develop XDMF format for visualisation and checkpointing

## Abstract

XDMF is a file format which is designed for very large simulation datasets. 
The main file is XML, but there is provision for the "heavy data" to be stored in HDF5 using 
MPI-IO in parallel. Datasets may be hundreds of GB in size. In FEniCS we have used XDMF to produce 
visualisations, but it is also highly desirable to use for checkpointing, i.e. saving the current 
state of a simulation and reading it back in later. The data structures in FEniCS do not map 
directly onto the data structures expected by the current implementation of XDMF. We would like 
to be able to save FEniCS data structures directly in XDMF, and still be able to visualise the data.

## Technical Details

FEniCS has a C++ interface, called DOLFIN, and wrappers in SWIG for a Python interface. 
There is already an I/O module which produces XDMF output, and this would need to be extended 
and adapted to produce the suitable output for an enhanced XDMF specification. 
Additionally, a new C++ method would need to be implemented to read in the XDMF from file in 
parallel, and distribute it correctly to restore from file after checkpointing.

From the visualisation side, a set of Python filters will need to be developed to read the 
new data format in ParaView and display it using VTK. Some work has already been done in this 
direction: https://github.com/chrisrichardson/xdmf-fe-filter Ultimately, the filters will need 
to be translated to C++ and incorporated as part of the Xdmf3 library.

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

* Get know FEniCS workflow, test custom dolfin compilation.
* Study existing I/O module for XDMF output, current issues and pitfalls.
* Communicate with a mentor and FEniCS community on this issue.
* Study the XDMF3 specification.

### May 29th - June 3rd

* Edit the current XDMFFile class output methods in accordance with enhanced XDMF specification.

### June 5th - June 9th

* Develop a new C++ method to read in the XDMF from file in parallel.
* Make sure the distribution of (checkpointed) restored data is correct. 

### June 12th - June 16th

* Further work on the parallel XDMF input.

### June 19th - June 23th, **End of Phase 1**

* Generate documentation for the code written during phase 1, so the work is clear, complete
and independent.
* Goal: DOLFIN should have improved XDMF read/write methods working also in parallel. 
* ?Write tests.?

### June 26 - June 30th, **Begin of Phase 2**

* Test already prepared Python filters with newly developed XDMF output.
* Python filters partially prepared at [https://github.com/chrisrichardson/xdmf-fe-filter](https://github.com/chrisrichardson/xdmf-fe-filter).

### July 3rd - July 7th

* Further tests of Python filters in ParaView.
* Check both: serial and parallel output.

### July 10th - July 14th

* Focus on proper functioning of CGn and DGn elements.
* Finalize changes, existing code improvement, refactorization.

### July 17th - July 21th, **End of Phase 2**

* Generate documentation for the code written during phase 2, so the work is clear, complete
and independent.
* Goal: Python filters for basic FEniCS elements should correctly read newly (from phase 1) 
generated XDMF output.

### July 24th - July 28th, **Begin of Phase 3**

* Study the existing C++ ParaView/VTK/XDMF3 library.
* Be familiar with Paraview C++ API for VTK visualization and prepare the Python -> C++
conversion strategy.

### July 31st - August 4th

* Start work on Python -> C++ conversion of filters.
* Be in constant discussion with developers of ParaView(Kitware).

### August 7th - August 11th

* More work on rewriting the filters to C++.

### August 14th - August 18th

* Generate documentation for the code written during phase 3, so the work is clear, complete
and independent.
* Goal: XDMF3 library in ParaView should be able to read and visualize the newly developed
FEniCS XDMF output.

### August 21st - August 25th, **Final Week**

* I expect some bugs from phase 1, 2, 3 to appear. Solve the possible
issues and test the code again.
* Reflect minor changes from community response.

### August 28th - August 29th, **Submit final work**

## Future works

* Python filters for all FEniCS elements (higher order, Brezzi-Douglas-Marini). 

## Development Experience

* I've worked for 3 years as a web developer in webstudio in Prague. I developed CMS based on 
Symfony framework, language PHP. Symfony is an OOP framework, we worked with GIT and managed
repositiories via Bitbucket. We used Composer as a dependency manager for internal CMS parts. 
* My bachelor thesis is based on FEniCS code and I have 2 years experience in using FEniCS 
(via Python).  
* I have minor [merged pull request](https://bitbucket.org/simula_cbc/cbcpost/pull-requests/1/according-to-line-94-of-cbcbatch-runnable/diff#comment-None) in cbcpost documentation - postprocessing tool for FEniCS.

## Other Experiences

* I am master student of programme Mathematical and computational modelling in mathematics and 
physics, Charles University in Prague. 

## Why this project?

* In my bachelor thesis I tried VTK(*.pvd) and XDMF(*.xdmf) as an output format
and neither of them behaved as I expected, especially in parallel. Correct parallel
checkpointing of results is a challenge and better(more documented) IO FEniCS
workflow would help to postprocess results a lot.
* This project will help me to dig deeper into FEniCS code and be a benefit
for young and open-source community. Having some experience from this project would allow
me to contribute to FEniCS even more in the future.

## Appendix

* My webpage: [http://karlin.mff.cuni.cz/~habera/](http://karlin.mff.cuni.cz/~habera/)
