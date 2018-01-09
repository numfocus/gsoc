# FEniCS Project

In all cases, we recommend that prospective students join our [Slack channel]
(https://fenicsproject-slack-invite.herokuapp.com/) and work with us to
write their project proposals.

## Develop Complex Number support in FEniCS

### Abstract

Description

| **Intensity** | **Priority | **Involves**  | **Mentors** |
| ------------- | -----------| ------------- | ----------- |
| Moderate      | Medium     | Python, C++   | [Chris Richardson](mailto:chris@bpi.cam.ac.uk) |

### Technical Details

FEniCS has a C++ interface, called DOLFIN, and wrappers in pybind11 for a
Python interface.  PETSc and Trilinos support complex numbers.

### Open Source Development Experience

This project requires knowledge of C++ and Python.

### First steps

Install FEniCS from https://bitbucket.org/fenics-project/dolfin and try out the demos.
Install [ParaView](http://www.paraview.org) and view the output from the FEniCS demos.
The FEniCS tutorial at https://fenicsproject.org/tutorial/
has an up-to-date description of using FEniCS to solve partial differential equations.

## GMsh/XDMF/DOLFIN mesh processing pipeline

### Abstract

Finite element methods require a discretisation of a domain into small
elements, called a mesh.  Typically, users of DOLFIN use an external mesh
generation package, such as [gmsh](http://gmsh.info) to construct meshes,
before reading them into DOLFIN.  In this project, you will work to ensure that
[gmsh](http://gmsh.info), DOLFIN, and our preferred visualisation package,
[Paraview](http://paraview.org) work seamlessly together. This will be a huge
usability improvement for users working with very complex geometries.

### Description

| **Intensity** | **Priority | **Involves**  | **Mentors** |
| ------------- | -----------| ------------- | ----------- |
| Moderate      | Medium     | Python, C++   | [Jack S. Hale](mailto:mail@jackhale.co.uk) |

### Technical Details

There are two possible paths that could be taken in this project:

1. Modify gmsh to directly support [XDMF](http://www.xdmf.org/index.php/XDMF_Model_and_F   ormat) format files. DOLFIN directly supports reading and writing XDMF.
2. Contribute to and improve [meshio](https://github.com/nschloe/meshio) a package for converting between different mesh file formats, e.g. gmsh's ``.msh`` and ``.xdmf``.

### Open Source Development Experience

This project requires knowledge of C++ and Python. You will mainly be working
with DOLFIN, the finite element solver of the FEniCS Project, gmsh, meshio and
Paraview. Some knowledge of finite element methods would also be desirable, but
not necessary. Students with interest in computational geometry may find this
project particularly suitable. Furthermore, students will be required to work
with other open-source projects, requiring good interpersonal skills.

### First steps

Install FEniCS from https://bitbucket.org/fenics-project/dolfin and try out the demos.
Install [ParaView](http://www.paraview.org) and view the output from the FEniCS demos.
The FEniCS tutorial at https://fenicsproject.org/tutorial/
has an up-to-date description of using FEniCS to solve partial differential equations.


## Integration with ROL?

## Vectorisation?

### Abstract


| **Intensity** | **Priority | **Involves**  | **Mentors** |
| ------------- | -----------| ------------- | ----------- |
| High          | Medium     | Python, C++ | [Jack S. Hale](mailto:jack.hale@uni.lu)

### Technical Details

TODO

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
