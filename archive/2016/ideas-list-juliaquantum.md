# Ideas Pages - JuliaQuantum

## Overview

[JuliaQuantum](http://juliaquantum.github.io) is an open-source organization to build libraries in Julia for Quantum science and technology. 
Through the fast development of last year--especially by successfully completing a GSoC project, the organization has implemented a framework of libraries to represent basic concepts in quantum mechanics and to solve foudamental quantum dynamics equations. 
However, there are still just a few breaks of chains to fully promote the existing projects to be as useful as other packages written in other programming languages. Implementing some binding packages with applications to existing packages are the focus of the ideal J/GSoC projects this year in JuliaQuantum:

- Generalizing and enhancing the existing quantum state type system. For example, developing a "proper Array-with-basis type/package"--basically replacing our `QuArray` stuff and making it available for others. This may also be useful outside JuliaQuantum. In the mean time, we are still missing the tensor network representation for some quantum many-body simulations.  
- Optimizing and enhancing the current quantum dynamics solvers and including more well-developed equation solvers for users to choose. Also building a plugin to let the solvers easily compatible with various parallel computing strategies. ***@obiajulu*** proposed to implement symplectic splitting methods for time-dependent equations. In the mean time, implementing new solvers for stochastic quantum dynamics and the like would be helpful.
- Building a new package on the application level which can bind most of our existing repos for a fairly large group of potential users. Some possible application directions are many-body physics simulations and quantum information applications.
- Visualizing the abstracts: It would be cool and useful to have a package to visualize quantum states on the Bloch sphere, quantum circuits and fidelity matrix etc using a framework which allows it to switch the underlying plotting platform. 
- Fully building a symbolic calculas package for quantum machenics. Although the [SymPy.jl](https://github.com/jverzani/SymPy.jl) package is enough for calling the symbolic package in Python, our community users has found some important features not implemented in the original SymPy Python package. It would be nice if one can implement a symbolic representing package interfacing with our existing types and ecosystem for quantum science and extentable to other fields. The native type system of Julia could be a very essential feature to implement this idea efficiently. Our current [QuDirac.jl](https://github.com/JuliaQuantum/QuDirac.jl) project could be extended to this direction. 

### Discussion threads

-Discussions can be found in the [JuliaQuantum.github.io repo](https://github.com/JuliaQuantum/JuliaQuantum.github.io/issues/32). 

## Current proposal: Julia Quantum: Framework for Quantum Computation Simulators

### Motivation

As quantum computation implementation in experiments grows rapidly, a toolkit for bridging the gap between experimentalist and theorists could be necessary. To be specific, Translating quantum algorithm to quantum circuits or Hamiltonians in other models with a lower resource cost is important for both experimentalists and theorists.

This project will help scientists to design better architecture of quantum computers in real life. It will link the existing base libraries under JuliaQuantum to the widely known quantum information application level, and outline a framework of the type system and necessary solvers in Julia. 

### Proposer

Name: Xiujie (Roger) Luo: 

Email: rogerluo@mail.ustc.edu.cn

GitHub profile: [@Roger-luo](https://github.com/Roger-luo)

Location/Timezone: China, CST

University: University of Science and Technology of China

### Mentors and contact information

1. Yongjian Han:
  
  Professor, Key Lab of Quantum Information, University of Science and Technology of China.
  
  Contact: smhan@ustc.edu.cn
  
2. Alexander Croy:
  
  Postdoctoral fellow, Max Plank Institure for the Physics of Complex Systems.
  
  Contact: [@acroy](https://github.com/acroy)
  
### Detailed Proposal

[Julia Quantum: Framework for Quantum Computation Simulators](https://github.com/numfocus/gsoc/blob/master/2016/proposals/Roger-luo-proposal-for-JuliaQuantum.md).