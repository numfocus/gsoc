# FEniCS Project

## Develop [XDMF](http://www.xdmf.org) format for visualisation and checkpointing

### Abstract

XDMF is a file format which is designed for very large simulation datasets. The main file
is XML, but there is provision for the "heavy data" to be stored in HDF5 using MPI-IO in parallel.
Datasets may be hundreds of GB in size. In FEniCS we have used XDMF to produce visualisations, but
it is also highly desirable to use for checkpointing, i.e. saving the current state of a simulation
and reading it back in later. The data structures in FEniCS do not map directly onto the data
structures expected by the current implementation of XDMF. We would like to be able to save FEniCS
data structures directly in XDMF, and still be able to visualise the data.

| **Intensity** | **Priority | **Involves**  | **Mentors** |
| ------------- | -----------| ------------- | ----------- |
| Moderate      | Medium     | Python, C++, XML | [David DeMarle](mailto:dave.demarle@kitware.com), [Chris Richardson](mailto:chris@bpi.cam.ac.uk) |

### Technical Details

FEniCS has a C++ interface, called DOLFIN, and wrappers in SWIG for a Python interface.
There is already an I/O module which produces XDMF output, and this would need to be extended and
adapted to produce the suitable output for an enhanced XDMF specification. Additionally, a new
C++ method would nede to be implemented to read in the XDMF from file in parallel, and distribute it
correctly to restore from file after checkpointing.

From the visualisation side, a set of Python filters will need to be developed to read the
new data format in ParaView and display it using VTK. Some work has already been done in this direction:
https://github.com/chrisrichardson/xdmf-fe-filter
Ultimately, the filters will need to be translated to C++ and incorporated as part of the Xdmf3 library.

### Open Source Development Experience

This project requires knowledge of C++ and Python, and will require working with multiple git repositories
and different teams with FEniCS and Kitware developers.

### First steps

Install FEniCS from https://bitbucket.org/fenics-project/dolfin and try out the demos. Install [ParaView](http://www.paraview.org)
and view the output from the FEniCS demos.
