# FEniCS Project

In all cases, we recommend that prospective students join our [Slack channel](https://fenicsproject-slack-invite.herokuapp.com/) and work with us to
write their project proposals.

## Complex Number support in FEniCS

### Abstract

Currently the finite element solver of the FEniCS Project, DOLFIN, supports
real numbers represented as floating point arithmetic. However, many
applications, such as electromagnetics, require complex numbers, represented
as two floating point numbers representing a real and imaginary part. In this
project you will adapt DOLFIN and FFC to support complex numbers, opening up
the possibility of large-scale electromagnetics problems to the users of the
FEniCS Project.

### Description

| **Intensity** | **Priority | **Involves**  | **Mentors** |
| ------------- | -----------| ------------- | ----------- |
| Moderate      | Medium     | Python, C++   | [Chris Richardson](mailto:chris@bpi.cam.ac.uk) |

### Technical Details

FEniCS has a C++ interface, called DOLFIN, and wrappers in pybind11 for a
Python interface. The linear algebra backends supported by DOLFIN, which solve
linear systems of the form Ax = b, already support complex numbers. You will
need to adapt FFC, the FEniCS Form Compiler, to generate code with complex
numbers.  Furthermore, DOLFIN, the finite element solver, will also need to be
modified to support complex numbers. 

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

1. Modify gmsh to directly support [XDMF](http://www.xdmf.org/index.php/XDMF_Model_and_Format) format files. DOLFIN directly supports reading and writing XDMF.
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

## Vectorisation

### Abstract

Modern CPUs contain SIMD (Single-Instruction, Multiple Data) instructions that
allow that exploit data-level parallelism to compute multiple floating
operations in one CPU cycle. This is commonly called vectorisation. Taking
advantage of SIMD operations is critical to acheive the maximum number of
floating point operations (FLOPS) in numerical simulation codes, such as the
FEniCS Project. However, FFC, the FEniCS Form Compiler, which compiles
high-level UFL form language into C++ code, does not currently generate code
with SIMD assembly instructions. In this project you will extend FFC to
recognise structures that would benefit from SIMD acceleration, and output
SIMD-friendly C/C++ code.


| **Intensity** | **Priority | **Involves**  | **Mentors** |
| ------------- | -----------| ------------- | ----------- |
| High          | Medium     | Python, C++ | [Jack S. Hale](mailto:jack.hale@uni.lu)

### Technical Details

Modern compiler toolchains (Intel, GCC) enable the automatic vectorisation of
code, if they can detect loops and memory layouts (alignment) compatible with
vectorisation. You will work to modify FFC to produce code compatible with
automatic compiler vectorisation. Such features would ensure that modern CPU
architectures are being used optimally.

### Open Source Development Experience

This project requires knowledge of C++ and Python, and will require working
with multiple git repositories and different teams with FEniCS developers. Some
knowledge of finite element methods would also be desirable, but not necessary.
Those who are interested compiler technology might also find this project
suitable. The project would particularly suit students with a background in
Computer Science.

### First steps

Install FEniCS from https://bitbucket.org/fenics-project/dolfin and try out the
demos. The FEniCS book
https://fenicsproject.org/pub/book/book/fenics-book-2011-06-14.pdf (GNU Free
Doc License) contains a description of the form compiler technology behind
FEniCS. The FEniCS tutorial at https://fenicsproject.org/tutorial/ has an
up-to-date description of using FEniCS to solve partial differential equations.
