# Develop assembly of finite element forms on quadtrilateral and hexahedral meshes

## Abstract

One of the first steps in the finite element method (FEM) is splitting the domain on which the partial differential equation (PDE) is solved into small parts, 
called cells, which in sum make a mesh. FEniCS has always supported meshes consisting of simplex cells (e.g. triangles and tetrahedrons),
but has limited support for meshes consisting of quadrilateral (quad) and hexahedral (hex) cells. 
Finite element problems solved on quad/hex meshes often have better approximation properties and better robustness
to cell distortion than those solved on simplex meshes.
The project idea aims at being able to assemble and solve the simplest PDE, a Poisson's equation, in 2D (quad mesh) and 3D (hex mesh) in FEniCS.

## Technical Details

Many constituent parts to assemble and solve on quad/hex meshes are already in FEniCS, but
there are missing links to get the pieces working as a whole.

A key technological innovation in FEniCS is the development of a domain specific language
for specifying finite element variational forms (UFL) and a form compiler (FFC) that can 
translates UFL into low-level C++ code that is used to generate cell tensors (local matrices) on
every cell in the mesh.

Currently this toolchain cannot produce the C++ code for quad/hex cell geometries. What is needed to do is 
to interface FFC with an existing class in FInite element Automatic Tabulator (FIAT) to evaluate the tensor product finite 
element basis functions on the quad/hex cell geometry. An appropriate features
are needed to be added into FFC to compute geometric quantities on the quad/hex cell geometry, using the existing
code for triangles and tetrahedrons as an example. Finally, linking everything to the DOLFIN problem solving 
environment should be done to solve a complete problem.

The main sources of information for this project are the sourcecode and [The FEniCS Book](https://link.springer.com/book/10.1007%2F978-3-642-23099-8).
Any other book on FEM can also be useful (for example a [book by Claes Johnson](https://www.amazon.com/Numerical-Solution-Differential-Equations-Mathematics/dp/048646900X))

## Schedule of Deliverables

### May 4th - May 30th, **Community Bonding Period**

* Familiarize myself completely with FEniCS functionality and architecture.
* Be in constant touch with my mentor and the FEniCS community.
* Thus with the help of my mentor I will become absolutely clear about my future goals, the final implementations that need to be done as well as the approach that I will follow.

### May 30th - June 3rd

* Start working on building an interface between FFC and FIAT for quad/hex mesh.
   
   Following script should work correctly:  
   ```python
   from dolfin import *
   mesh = UnitQuadMesh(mpi_comm_world(), 1, 1) #2D mesh of quadrilaterals with 4 vertices and 1 cell
   print(assemble(1.0*dx(mesh))) #should return 1.0
   ```  
   Modifications needed in ffc/fiatinterface.py, ffc/representation.py
   
### June 5th - June 9th

* Testing and improving the interface.  
 * Work on uflacs representation for quad/hex mesh in FFC.
 * (optional) Fix plotting of quad/hex mesh:
   
   `plot(mesh)` currently does not work  
   In 3D plots with `HTML(X3DOM().html(mesh))` quad faces are not represented correctly
   
### June 12th - June 16th

* Defining function spaces for quad/hex mesh
   
   The data of a FunctionSpace is represented in terms of a triplet consisting of a Mesh, a DofMap and a FiniteElement.  
   The DofMap provides the function tabulate_dofs which maps the local degrees of freedom on any given cell of the Mesh to global degrees of freedom.  
   The FiniteElement defines the local function space on any given cell of the Mesh.

   Following script should work correctly: 
   ```python
   from dolfin import *  
   mesh = UnitQuadMesh(mpi_comm_world(), 1, 1) #2D mesh of quadrilaterals with 4 vertices and 1 cell  
   V = FunctionSpace(mesh, "P", 1) #Function space on a quadrilateral  
   f = Function(V) #Function in FE space V  
   ```   
   Modifications needed in dolfin/functions/functionspace.py  

### June 19th - June 23th

* Further work on function spaces.
* Skeleton code should be ready by Phase 2.

### June 26 - June 30th, **End of Phase 1**

* Existing code evaluation and improvement.

### July 3rd - July 7th, **Begin of Phase 2**

* Test and get the interpolation of an expression into function space working
   
   Following script should work correctly:    
   ```python
   from dolfin import *  
   mesh = UnitQuadMesh(mpi_comm_world(), 1, 1) #2D mesh of quadrilaterals with 4 vertices and 1 cell  
   V = FunctionSpace(mesh, "P", 1) #Function space on a quadrilateral  
   m = interpolate(Expression(('...', '...')), V)
   ``` 
   dolfin/fem/interpolation.py

### July 10th - July 14th

* Further work on `interpolate`, `project`, `assemble`

### July 17th - July 21th

* Finalize changes for quad/hex mesh in FIAT/FFC.

### July 24th - July 28th, **End of Phase 2**

* Existing code evaluation and improvement.

### July 31st - August 4th, **Begin of Phase 3**

* Finalize linking everything to DOLFIN.
   
   Following functionality should be tested and fixed if needed:  
   
    * Assembling the functionals 1*dx (for unit size mesh should return 1) and x[0]*dx (for unit size mesh should return 0.5)
    * Defining a FunctionSpace `V = FunctionSpace(mesh, "P", 1)`
    * Interpolating an expression into a function `interpolate(Expression(('...', '...')), V)`
    * Assembling the functional f*dx `assemble(f*dx(mesh))`
    * Assembling a linear form fvdx `L = f*v*dx`
    * Assembling a bilinear form uvdx `a = inner(u, v)*dx`
    * Projecting an expression into a function space `project('...', FunctionSpace)`
    * Solve a Poisson problem

### August 7th - August 11th

* Make the documentation.
* Make the demo program with Jupyter Notebook.

### August 14th - August 18th

* One week of break. I am going to attend a Nordic Graduate Course in Computational Mathematical Modeling in Oslo.

### August 21st - August 25th, **Final Week**

* The last week will involve any remaning code cleaning, and finishing the documentation.

### August 28th - August 29th, **Submit final work**

* Have the PR merged.
* Discussion and evaluation of the work done.

## Future works

I plan to do my PhD on a topic related to FEM, and I think FEniCS will be one of the tools I will use.
I will try to keep contributing to FEniCS with issues and PRs, and remain an active part of the community.

## Development Experience

For my bachelor's thesis I was developing a tool for 3D microstructure generation with the grain boundaries in Matlab.
Also this project involved developing subroutines in Fortran for Finite Element Analysis Program (FEAP) for modelling crack growth between the grains with cohesive elements with hex and wedge type of elements support.
Recently, I have worked on adding an improved boundary condition implementation for open source program Pencil Code (so far 2 PRs merged [first](https://github.com/pencil-code/pencil-code/pull/15), [second](https://github.com/pencil-code/pencil-code/pull/16)).
I have added minor fixes to FEniCS Project [PR #70](https://bitbucket.org/fenics-project/ufl/pull-requests/70/), [PR #340](https://bitbucket.org/fenics-project/dolfin/pull-requests/340/).

## Other Experiences

I am master student in Aalto University, Finland. My previous experience involves volunteering at [Slush](http://www.slush.org/), writing my bachelor's thesis in the Intitute of Applied Mechanics RWTH Aachen, and summer engineering job.
My [resume](https://drive.google.com/open?id=0B7_NVEEiR5wUNkpKRlkwQ2dQeDg) shows the details of my work and research experience.

## Why this project?

I am very interested in the field of computational mechanics. FEniCS is one of the most advanced open source software for FEM and I wish to improve it by adding additional functionality for quad/hex mesh.
Working on this project will let me further develop my knowledge and understanding of the essential parts of the Finite Element Method.
Collaborating and sharing the ideas with the experts in the area of FEM and scietific computing will help me to determine my path for the future research.

