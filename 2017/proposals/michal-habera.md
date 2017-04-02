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

* Get know FEniCS workflow.
* Try custom DOLFIN compilation within Docker.
* Study existing I/O module for XDMF output, current issues and pitfalls.
* Study Pugi XML parser manual - a cornerstone of current XDMF I/O.
* Study the XDMF3 specification.
* Communicate with a mentor and FEniCS community on this issue.

### May 29th - June 3rd

* Edit the current XDMFFile class output methods in accordance with enhanced XDMF3 specification.
* Clean and refactor current XDMFFile class. There are some places, where logic is unnecessary cloned, 
e.g. when adding XDMF node and version attributes.
* Think of higher-order mesh IO. New XDMF IO model should respect possible-in-future higher-order mesh
geometry. Currently, there is support for quadratic mesh (XDMFFile::build_mesh_quadratic), but it is not
allowed in parallel.
* Related issue [#759](https://bitbucket.org/fenics-project/dolfin/issues/759/more-xdmf-features-wanted)
* Related issue [#488](https://bitbucket.org/fenics-project/dolfin/issues/488/add-support-xdmf-mesh-entity-attributes)
* Related issue [#837](https://bitbucket.org/fenics-project/dolfin/issues/837/io-on-ghosted-meshes-buggy)
* Work mostly with DOLFIN/dolfin/io/XDMFFile.\*, DOLFIN/dolfin/io/HDF5File.\*


### June 5th - June 9th

* Develop a new C++ method to read in the XDMF from file in parallel.
* Make sure the distribution of (checkpointed) restored data is correct. 
* Add close/clear/destroy function to XDMFFile, related issue [#390](https://bitbucket.org/fenics-project/dolfin/issues/390/add-close-clear-destroy-function-to)
* Work mostly with DOLFIN/dolfin/io/XDMFFile.\*, DOLFIN/dolfin/io/HDF5File.\*

### June 12th - June 16th

* Further work on the parallel XDMF input.
* Possibly attend FEniCS‘17 conference at University of Luxembourg, 12-14 June 2017,
to meet the mentors in person and discuss progress.

### June 19th - June 23th, **End of Phase 1**

* Generate documentation for the code written during phase 1, so the work is clear, complete
and independent.
* Goal: DOLFIN should have improved XDMF read/write methods working also in parallel. 
* Write tests.
* The work should be merged into master branch with all tests passed.

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

* I expect some bugs from phase 1, 2 to appear. Solve the possible
issues and test the code again. If not, think of how items from Future works section could be 
implemented. If have enough time then do so.

### July 31st - August 4th

* Study the existing C++ ParaView/VTK/XDMF3 library.
* Be familiar with Paraview C++ API for VTK visualization and prepare the Python -> C++
conversion strategy.
* Start work on Python -> C++ conversion of filters.
* Be in constant discussion with developers of ParaView(Kitware).

### August 7th - August 11th

* More work on rewriting the filters to C++.

### August 14th - August 18th

* Generate documentation for the code written during phase 3, so the work is clear, complete
and independent.
* Goal: XDMF3 library in ParaView should be able to read and visualize the newly developed
FEniCS XDMF3 output.

### August 21st - August 25th, **Final Week**

* I expect some bugs from phase 1, 2, 3 to appear. Solve the possible
issues and test the code again.
* Write additional tests.
* Reflect minor changes from community response.

### August 28th - August 29th, **Submit final work**

## Future works

* Python filters for all FEniCS elements (higher order, Brezzi-Douglas-Marini). 
* Higher order mesh geometries.

## Development Experience

* I've worked for 3 years as a web developer in webstudio in Prague (20 hrs per week). I developed CMS based on 
Symfony framework, language PHP. Symfony is an OOP framework, we worked with GIT and managed
repositiories via Bitbucket. We used Composer as a dependency manager for internal CMS parts. 
As the web developer I worked in PHP, JavaScript, HTML, SQL, Linux. Besides my bachelor and master
thesis being in Python I have written scripts/code in C/C++, R, Mathematica, LaTeX.
* My bachelor thesis is based on FEniCS code and I have 2 years experience in using FEniCS 
(via Python).  
* I have [merged pull request](https://bitbucket.org/fenics-project/dolfin/pull-requests/349/added-simple-xyzfile-python-unit-test/diff) to improve FEniCS test coverage.
* I have minor [merged pull request](https://bitbucket.org/simula_cbc/cbcpost/pull-requests/1/according-to-line-94-of-cbcbatch-runnable/diff#comment-None) in cbcpost documentation - postprocessing tool for FEniCS.
* I have minor [merged pull request](https://bitbucket.org/fenics-project/docker/pull-requests/20/add-e-flag-to-sudo)
in FEniCS installation guide via Docker.

## Other Experiences

* I am master student of programme Mathematical and computational modelling in mathematics and 
physics, Charles University, Prague. 

## Why this project?

* In my bachelor thesis I tried VTK(\*.pvd) and XDMF(\*.xdmf) as an output format
and neither of them behaved as I expected, especially in parallel. Correct parallel
checkpointing of results is a challenge and better(more documented) IO FEniCS
workflow would help to postprocess results a lot.
* Moreover, there is a discrepancy in what data could FEniCS save and reload, and what data could FEniCS
save for visualisation (in Paraview). One can save and reload his results with pure "heavy-data" HDF5 format,
but one cannot use this files for visualisation in Paraview. I think, this is a very good issue to improve. 
FEniCS community and its users will greatly benefit from having XDMF3 format which handles both. This will
be implemented into FEniCS sooner or later, but GSoC could boost the process substantially.
* This project will help me to dig deeper into FEniCS code and be a benefit
for young and open-source community. Having some experience from this project would allow
me to contribute to FEniCS even more in the future.
* I pland to do a PhD that will inevitably use finite element method and its FEniCS implementation.

## Workload and summer plans

* I plan to devote 8 hrs daily to work on this project. I am having my final state exam in mid of june, so there might
be a slight drop in workload to approx. 6 hrs per day. After that I have no obligations until october and I am prepared to work on the project full speed.

## Appendix

* My webpage: [http://karlin.mff.cuni.cz/~habera/](http://karlin.mff.cuni.cz/~habera/)
