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
and view the output from the FEniCS demos. The FEniCS tutorial at https://fenicsproject.org/tutorial/ 
has an up-to-date description of using FEniCS to solve partial differential equations.

## Develop assembly of finite element forms on quadtrilateral and hexahedral meshes

### Abstract

One of the first steps in the [finite element method](https://en.wikipedia.org/wiki/Finite_element_method) 
is splitting the domain on which the partial differential equation is solved into small parts, called cells, 
which in sum make a mesh. FEniCS has always supported meshes consisting of simplex cells (e.g. triangles and tetrahedrons),
but has limited support for meshes consisting of of quadrilateral (quad) and hexahedral (hex) cells. 
Finite element problems solved on quad/hex meshes often have better approximation properties and better robustness
to cell distortion than those solved on simplex meshes. We would like to be able to assemble and solve the simplest 
PDE, a Poisson problem on a quad/hex mesh in FEniCS.

| **Intensity** | **Priority | **Involves**  | **Mentors** |
| ------------- | -----------| ------------- | ----------- |
| High          | Medium     | Python, C++ | [Jack S. Hale](mailto:jack.hale@uni.lu), [Chris Richardson](mailto:chris@bpi.cam.ac.uk), [Martin Alnaes](mailto:martinal@simula.no) |

### Technical Details

Many constituent parts to assemble and solve on quad/hex meshes are already in FEniCS, but
there are missing links to get the pieces working as a whole. You will lead a project
get these missing pieces into place.

A key technological innovation in FEniCS is the development of a domain specific language
for specifying finite element variational forms (UFL) and a form compiler (FFC) that can 
translates UFL into low-level C++ code that is used to generate cell tensors (local matrices) on
every cell in the mesh.

Currently this toolchain cannot produce the C++ code for quad/hex cell geometries. You will
need to interface FFC with an existing class in FIAT to evaluate the tensor product finite 
element basis functions on the quad/hex cell geometry. You will then need to add appropriate code
into FFC to compute geometric quantities on the quad/hex cell geometry, using the existing
simplex code as an example. Finally, you will need to add hooks in our DOLFIN problem solving 
environment to solve a complete problem.

### Open Source Development Experience

This project requires knowledge of C++ and Python, and will require working with multiple git repositories
and different teams with FEniCS developers. Some knowledge of finite element methods would also be desirable, 
but not necessary. Those who are interested compiler technology might also find this project suitable.

### First steps

Install FEniCS from https://bitbucket.org/fenics-project/dolfin and try out the demos. The FEniCS book
https://fenicsproject.org/pub/book/book/fenics-book-2011-06-14.pdf (GNU Free Doc License) contains a 
description of the form compiler technology behind FEniCS. The FEniCS tutorial at
https://fenicsproject.org/tutorial/ has an up-to-date description of using FEniCS to solve partial
differential equations.
