# Julia Quantum: Framework for Quantum Computation Simulators

### Contact Info

Name: Xiuzhe Luo

English Name: Roger

Email: rogerluo@mail.ustc.edu.cn

Location/Timezone: China, CST

University: University of Science and Technology of China

## Personal Background:
### A brief intro
I am a junior student major in Physics and Computer Science in university of Science and Technology of China with rich numerical calculation experience.

##### Research Experience
I started my experience with numerical simulations since I was a freshman in Prof. Qin's([Hong Qin](http://www.princeton.edu/plasma/faculty/hqin/)) lab with my first project: Stochastic Differential Model For Run-away Electrons in Tokmak, which came from a small simulation program for lightning by Monte Carlo method.

I joined [Prof. Li's](http://lqcc.ustc.edu.cn/cfli/cfli-eng.html) lab in the Summer of 2015 and started my second project on numerical calculation for [non-classical path](http://journals.aps.org/prl/abstract/10.1103/PhysRevLett.113.120406) in quantum interference experiments to find a better physical quantity for measurement(finally I confirmed that it's possible to measure this so-called non-classical path by using [weak measurement](http://science.sciencemag.org/content/332/6034/1170.short)), and the thesis will soon be on arxiv.

I also participated in some other work in Physics such as China Undergraduate Physics Tournament(national grand prize in top 5), ICM (mostly for numerical calculations and programming)

### Motivation for the project:
As quantum computation implementation in experiments grows rapidly, a toolkit for bridging the gap between experimentalist and theorists could be necessary. To be specific, Translating quantum algorithm to quantum circuits or Hamiltonians in other models with a lower resource cost is important for both experimentalists and theorists.

This project will help scientists to design better architecture of quantum computers in real life.

My interest in quantum computation and recently work in simulations for quantum computation experiments turned me to propose this idea about a framework for quantum computation (Kebab).

Furthermore, adding some features which other libraries may not include will bring JuliaQuantum to a more powerful state and may also attract old users from other languages or libraries.

### Programming Skills
- Know well about: C/C++(lower than 14), Julia, Python (2.7/3), markdown
- Can use: Java, Mathematica, LaTeX, html, javascript/typescript

My first try on Julia was in the Summer of 2015 when I was programming for the non-classical path in C++, and I search 'best programming language for scientific programming' which finally leads me to Julia. So I had my first try and use Julia finished all the numerical calculations for this project. And I have experience about numerical simulations as mentioned above.


---
## Project Synopsis

##### Content
- Project Goal
    + Features
- Proposal Goal

#### Project Goal
A long-term aim of the project is to equip the [JuliaQuantum](http://juliaquantum.github.io/) with a new library for quantum information and computation, including simulations for quantum circuits, quantum computational models, error corrections, quantum compilers and related visualization.

**A brief structure of this project**

![](https://github.com/Roger-luo/Resources/tree/master/GSoC/2016/imag/structure.png)

##### Explanation for each layer

- **1st layer** provides the other layers the basic simulation environment for quantum computation
- **2nd layer** is based on the 1st layer can applies basic operations on a simulated quantum computer
    + **Built-in Algorithms** provides efficient implementation for commonly used algorithms, eg. quantum fourier transformation
    + **Circuit CAD toolkit** provides a user-friendly way to construct quantum circuits and test their outputs
    + **Compiler** ,the most complex part in this layer, provides optimizations for circuits and compile the quantum programming languages to circuit's APIs(instructions) of the 1st layer
- **3rd layer** is for quantum programming languages, such as Quipper and LIQUi> or developers can design their own languages and part of the compiler based on the simulated environment provided by 1st and 2nd layer. 


##### Similar libraries/softwares

- Microsoft [LIQUi|>](research.microsoft.com/en-us/projects/liquid/)
- QuTiP's [Quantum Information Processing](http://qutip.org/tutorials.html) part.

##### Features may be different from other libraries/softwares

- users can design their **own** compiler/language/algorithm based on the framework of first layer
- this project will provide users **various** computation models rather than quantum circuits only and will also provide developers a way to add their own models. 
- Different models(except quantum circuits) will be organized by gate-like modules in this project which will give users flexibility to mix different models or use only one pure model.
- **A brief standard** for users to add their own gates or computation models
- The compiler will be designed to have the ability to **optimize circuits** for different implementations (implementation here means optical implementation, super-conductor implementation, ...) 
- **A circuit CAD toolkit** will provide users a GUI to input their circuits, test the output of circuits and visualize them.
- **high efficiency** by implementing in Julia and other fast mature libraries.

#### Proposal Goal
This proposal will mainly focus on the 1st layer of the project (basic simulators), and will realize part of the circuit CAD tool kits and compiler due to  limitation of both time and knowledge in this summer.

**The proposal will realize**:

- a user-friendly type system to help developers add new features or gates in the framework (based on defining gates-like modules as subtype of `AbstractGates`)
- simulations:
    + [quantum circuit](https://en.wikipedia.org/wiki/Quantum_circuit)
    + [adiabatic quantum computation](https://en.wikipedia.org/wiki/Adiabatic_quantum_computation)
    + [one-way quantum computation](http://arxiv.org/abs/quant-ph/0602096v1)
    + [quantum error corrections](https://en.wikipedia.org/wiki/Quantum_error_correction) (eg. [CSS code](https://en.wikipedia.org/wiki/CSS_code)) by quantum circuits and one-way model
- optimizations:
    + **Quantum circuit optimization** optimize the circuit to an easy-realized form for experiments, eg. three qubit gates could cost a lot more resource than some one or two qubit gates' combinations, so break a three qubit gate into combinations of one and two qubit gates will be necessary. 
        + [Qcompiler with CSD method](http://cpc.cs.qub.ac.uk/summaries/AENX_v1_0.html) 
    + **Adiabatic quantum computation optimization** reduce the evolution time 
        + [Ground state cooling of quantum systems via a one-shot measurement](http://arxiv.org/abs/1503.04133)
        + [Fast quantum algorithm for EC3 problem with trapped ions](http://arxiv.org/abs/1412.1722)
- visualizations:
    + **quantum circuit visualization** visualize the quantum circuits described by user in this framework by [Compose.jl](https://github.com/dcjones/Compose.jl) or [Luxor.jl](https://github.com/cormullion/Luxor.jl)
    + **graph states** graph states visualization will be based on [GraphPlot.jl](https://github.com/afternone/GraphPlot.jl)

At the end of the project, JuliaQuantum will be equipped with a framework to simulate quantum computation. Including the features about:

- quantum circuit's simulation and visualization
- Adiabatic model simulators
- one-way model's simulation and visualization
- basic error corrections

The project will make use of existing packages and aims to contribute back, for example:

- `QuArray` will be used to store the states of Qubits
- QuDynamics.jl will provide adiabatic computational model with more efficient Shordinger equation solvers with good accuracy. 


Explanation of nouns:

- **computational models** here means the algorithms that can provide universal computation and is possible to be realized in experiments


---
## Details :

### Structure of Types

![](https://github.com/Roger-luo/Resources/tree/master/GSoC/2016/imag/typestructure.png)


### Prototype
I wrote a prototype for my idea with only simple features about quantum circuit and adiabatic quantum simulation module in [Kebab](https://github.com/Roger-luo/Kebab).

### Implementation:

#### Type System
- `AbstractModels` is the supertype of all the quantum computation models
    - `QuCircuit` is the supertype of quantum circuit model
        + `Circuit` is the object for circuits
        + `Gate` is the object for quantum gates, eg. C-NOT, Hadamard, ... and will need an operator to be initialized
    - `Adiabatic` is the supertype of adiabatic computation
    - `Oneway`    is the supertype of one-way computation
        + `GraphState` is the type for graph states
- `AbstractOp` is the supertype for quantum operators
    + `Clifford` is the supertype for Clifford groups 
    + `TimeOp` is the supertype for all time operators in adiabatic computation
    + ...

At the end of the project, JuliaQuantum will be equipped with a framework to simulate quantum computation models including quantum circuits, adiabatic quantum computation, one-way model, and quantum error corrections with CSS code, graph code.


---
### Design :

The idea is to organize solvers, quantum gates and other quantum computation simulators in the form of quantum gates.

```julia
abstract AbstractModels
abstract QuCircuit<:AbstractModels
abstract Adiabatic<:AbstractModels
abstract Oneway <:AbstractModels

abstract AbstractOp{N}
abstract TimeOp{N}<:AbstractOp{N}
abstract Clifford<:AbstractOp{1}

# Basic Types for states
## should QuBit become a subtype of QuArray? or I just write my own type first?

type QuBit #or AbstractVector
    state::AbstractVector
    bitnum::Integer

    function QuBit(state::AbstractVector,bitnum::Integer)
        check_state(state) #check if state length meets bitnum
        new(state,bitnum)
    end
end

## A graph state is stored by an adjacent matrix
type GraphState<:Oneway
    adjaMatrix::AbstractMatrix
end

## or a group of stabilizer
type stabilizer<:GraphState
    vertexA::Integer
    vertexB::Integer
end

## at least there should be two constructors
GraphState(adjmatrix::AbstractMatrix)
GraphState(stabilizers::AbstractVector{stabilizer})
## A groups of stabilizers stored by vector

# Basic Types for Computation Models

# Type for Gates

immutable Gate{T}<:QuCircuit
    name::AbstractString
    op::T
    bitnum::Integer

    Gate(name::AbstractString,op::T,n::Integer)=new(name,op,n)
end

type GateUnit{T}<:QuCircuit
    control_bit::AbstractVector
    realated_bit:AbstractVector
    gate::Gate{T}
    time_layer::Integer
end

typealias Gates AbstractVector{GateUnit}

type Circuit<:QuCircuit
    gates::Gates
    bit_num::Integer
end
```

### At the end of the project, the result would look as follows:

Prototype 1: Simple Quantum Circuits
## Construct a circuit
a circuit is constructed by `Circuit`:

you can construct an empty circuit:
```julia
circuit = Circuit()
```

to add gates, use the method `addgate!`

the `addgate!` method allows following way to add a quantum gate or a module to a specific place in the circuit.

```julia
# for single qubit gates(like Hadamard)
# circuit : the circuit object
# gate : gate object you want to insert
# id   : the id of the qubit you want to add this gate to
addgate!(circuit,gate,id,time_layer)

# for example
# |a> ----[H]----
# |b> -----------
# |c> -----------
# let's add a Hadamard gate to the first qubit in this three-qubit circuit

# first create your circuit
c = Circuit()
addgate!(c,Hadamard,1,1)

# for controlled gates
# |a> -----x--------
#          |
# |b> ----[H]-------
# |c> --------------

c_H = Circuit()
addgate!(c_H, 1, Hadamard, 2, 1)
# or for multiple input gates(will be useful for other gate-like modules)

# |a> --------x------
#             |
# |b> ------|***|----
#           | M |
# |c> ------|***|----
c_M = Circuit()
addgate!(c_M, 1, M, [2, 3], 1)

# or for multiple controlled gates

# |a> -------x--------
#            |
# |b> -------x--------
#            |
# |c> ------[H]-------
cc_H = Circuit()
addgate!(cc_H, [1,2], Hadamard, 3)

```

to measure a circuit, simply use the `measure` method on the circuit you want to measure with the the circuit's input state

```julia
# measure the circuit:
# |a> ----[H]----
# |b> -----------
# |c> -----------
# with input state:
# 1/2*sqrt(2)(|000>+|001>+|010>+...+|111>)

input_state = InitState(3)
circuit = Circuit()
addgate!(circuit, Hadamard, 1, 1)
res_state = measure(circuit,input_state)
```

Remove a gate from circuit
```julia
# given a circuit
# |a> ----[H]-------
# |b> ---------[H]--
# |c> --------------

removegate!(circuit,1,1)

# remove the first gate
# |a> --------------
# |b> --------[H]---
# |c> --------------
```

Prototype 2: quantum  Fourier Transformation

[wiki](https://en.wikipedia.org/wiki/Quantum_Fourier_transform)
[implementation in adiabatic computational model](http://journal.frontiersin.org/article/10.3389/fphy.2014.00044/full)

![QFT circuit pic](imag/fourier.svg)

```julia
# Quantum Fourier Transformation
init_state = InitStates(4)# inputs the number of bits

QFT_curcuit = Circuit()

for i=1:4
    addgate!(QFT_circuit,Hadamard,i)
    for j=1:i-1
        addgate!(QFT_circuit,bitID-i,R_k(i),1)
    end
end

res_state = measure(QFT_circuit,init_state)
# suppose this to be the command line(the result is made up)
---
measurement of QFT_circuit:
states| prob
|0000>: 0.0001
|0001>: 0.2000
...
|1111>: 0.2000
---

figure(1)
# For visualization, a circuit should be plotted by
plot(QFT_circuit)
show()
```

Prototype 3:Daemon-like cooling

![cooling circuit](imag/circuit.png)

*This picture comes from [doi:10.103](http://www.nature.com/nphoton/journal/v8/n2/full/nphoton.2013.354.html)*

```julia
# Daemon-like Cooling Circuit
# This prototype aims to show how other computation should be inserted in this framework

init_state = PrepareBellStates(4)
cooling = Circuit()

addgate(cooling,1,"Hadamard")
#if the gate needs parameters,will be passed after its name
addgate(cooling,1,"R",gamma) 
addgate(cooling,1,[2,3,4],"TimeEvo",Hs,t)
addgate(cooling,1,"Hadamard")

res_state = measure(cooling)

figure(1)
#output figure will looks like the circuit figure in the picture above
plot(cooling)
show()
```

# Proposal Timeline

## Community Bonding Period

#### before April 22

- To familiarize myself with some other quantum physics libraries
- Survey on basic theories : quantum circuit optimization and one-way simulation
- To familiarize myself with JuliaGPU's libraries
- Try different type design in the prototype repo [Kebab](https://github.com/Roger-luo/Kebab)

#### April 22 - May 23 (Before the official coding time):

- To do self coding with Julia language to improve my knowledge and ideas about details in the realization.
- Study of other theories about quantum circuit optimization
- Discuss details of the Kebab with mentors and other community members, and to decide the final structure of Kebab.

## Coding Period

#### week 1
- Define the basic structure of types in quantum circuit and adiabatic model and `AbstractOp`
    + C-NOT
    + Hadamard
    + Pauli-X, Pauli-Y, Pauli-Z
    + TimeOp: real-time evolution operator, imagine-time evolution operator
    + object: `Circuit`

#### week 2-3
- Realize quantum circuits and adiabatic computation
    + method: `addgate!`, `removegate!` 
    + method: `pHamiltonian` :: Hamiltonian constructors for a given problem

#### week 4
- Visualize quantum circuit
    + method: `plot` (overload):: plot a given circuit object constructed by `Circuit`
- Realize basic features of quantum error corrections
    + Implementation of CSS code

#### week 5
- polishing of the code


*June 27, Mid-term evaluations*

#### week 1
- Realize basic features of one-way quantum computation model,including:
    + graph state initializer

#### week 2-4
- Realize basic features of one-way quantum computation model,including:
    + measurement simulation
        * Clifford groups as subtype of `AbstractOp`
        * optimization for a given graph state to a lower cost graph state
    + (will be done if time is enough) error corrections by one-way model

#### week 4-6
- improve performance of finished functions
- Visualize graph state
- check the experimental realization with the simulation. Adjust parameters and structure to approximate the simulation to the experimental realization.

#### week 7-8
- For Documentation and polishing of code

A Buffer of 9 days has been kept for any unpredictable delay.
