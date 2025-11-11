# Julia Quantum : Framework for Solvers

### Contact Info

Name: Amit   
Email: bitsjamadagni@gmail.com   
Location/Timezone: India, GMT +5:30   
University: Birla Institute of Technology and Science-Pilani, India     
Mentor: Alexander Croy

## Personal Background:
### Motivation for the project:

I am a pre-final year undergraduate pursuing Masters in Mathematics along with Bachelors in Electronics and Electrical Engineering from BITS-Pilani, India. Being a Masters student in Mathematics I have been exposed to courses like Analysis, Algebra, Topology, Differential Geometry among many others. My interest for physics got me started with Quantum Mechanics, and this interest intensified as I started to realize the abstract nature of math being applied to develop axiomatic physical theories. Working with large software libraries always inspired me to construct one from ground zero. I was in search of a project where I could put my efforts in this direction, and I came across Julia. As I searched further I was excited to see Julia Quantum package which had intersections with my interests and since then have tried to contribute to it.

### Programming Skills:

My journey with open source started with SymPy where I had fixed some bugs in Quantum physics module, my interest in Topological Quantum Computation introduced me to the low dimensional topological based ideas of Links/Knots which further led to a Google Summer of Code 2014 project for Sage Mathematical Software where I was involved in developing Computational Knot theory library. The project was based on Python and it mainly dealt with conversions between various representations of Links/Knots, on the top of it there were few invariants implemented to distinguish between various Links/Knots. In addition my interest towards hardware allowed me undertake projects involving Raspberry Pi, the details of which could be found on my GitHub repo (Repo titles : [PiSMS : sending SMS using Pi] (https://github.com/amitjamadagni/PiSMS), [Project : Controlling GPIO pins via wireless communication techniques] (https://github.com/amitjamadagni/Project)). I had an opportunity to contribute to Julia and Julia Quantum in the form of enhancing few features. This project would further enhance my knowledge in various domains ranging from Quantum Mechanics to further understanding of Julia. 
 
* GitHub Profile : https://github.com/amitjamadagni
* GSoC 2014 : https://www.google-melange.com/gsoc/project/details/google/gsoc2014/amitjamadagni/5676830073815040
* GSoC 2014 blog : https://knotsknotted.wordpress.com/category/gsoc/
* Contribution to Julia : Implementation of ror! and rol! (https://github.com/JuliaLang/julia/pull/9822)         
* Contribution to Julia Quantum : Normalize to QuArray (https://github.com/JuliaQuantum/QuBase.jl/pull/15)           

---
### Project Synopsis : 

The aim of the project is to equip the [JuliaQuantum] (http://juliaquantum.github.io/) libraries with solvers which are on par with QuTiP. To this end, a flexible and unified framework will be created, which allows to access different solvers. The project will make use of existing packages (if possible) and aims to contribute back in order to homogenize their interfaces and improve the performance.

Main goals of the project include:

1. Framework to solve Schrodinger (SE) and Quantum Master Equations (ME) by
    * Exponential solvers
    * ODE solvers (Runge-Kutta(RK) Methods)
    * Monte Carlo Wave Function Method (MCWF)
    * Steady State Methods

2. Parallelizing some of the above tasks (Integrating QuArrays and methods involved in MCWF) using the existing infrastructure for parallel computing features in Julia to achieve greater efficiency.

3. In the course of integrating other packages, contribute back to improve their functionality and usability.


---
## Details :

### Implementation :
The implementation details can be broadly classified into pre and post parallelization. The pre parallelization phase would mainly deal with getting the basic algorithms and design work keeping in mind the parallelization that could be achieved. Once the base is set, I hope to work on parallelizing the implementation. Hamiltonian and the Master equation are used for propagation (Most of the theoretical  reference is from the QuTiP Documentation page [here] (http://qutip.org/docs/3.1.0/guide/guide-dynamics.html) and Python notebooks [here] (http://nbviewer.ipython.org/github/qutip/qutip-notebooks/tree/master/examples/) ). The main algorithmic construction is of the methods used for state evolution. 

* Exponential Solvers : The implemenation would use packages like [ExpmV.jl] (https://github.com/marcusps/ExpmV.jl) and [Expokit.jl] (https://github.com/acroy/Expokit.jl) to implement the evolution.

* Differential Equation Solvers : The implementation would use the solvers from [ODE.jl] (https://github.com/JuliaLang/ODE.jl) package.

* Monte Carlo Wavefunction : The implementation would be based on the proposed implementation in QuDOS.jl package and also with reference from QuTiP.

* Steady State equation : Various methods based out of QuTiP. 

The parallelization techniques can be achieved at certain level of abstraction. Some of the ideas are as follows :

*  Level of Abstraction --> Algorithmic (for example : Monte Carlo methods)

*  Level of Abstraction --> Type definitions (for example : `QuArray`) based out of DArray or using the package [ParallelSparseMatMul.jl] (https://github.com/madeleineudell/ParallelSparseMatMul.jl) given the fact `QuArray`s are based on arrays.

At the end of the project, JuliaQuantum will be equipped with a framework to solve dynamical equations arising in quantum mechanics. The framework itself and the structures defined in QuBase will be useable with distributed/shared arrays to enable development of methods using parallel computing.

---
### Design :
    
We have discussed the implementation design [here]   (https://github.com/JuliaQuantum/JuliaQuantum.github.io/issues/20)

The idea is to encapsulate Schrodinger and Master Equations by appropriate types, for example :
```julia

abstract QuantumEquation

type SE <: QuantumEquation
    H::QuMatrix # Hamiltonian
end

type ME <: QuantumEquation
    H::QuMatrix # Hamiltonian 
    c_ops::Vector{QuMatrix} # collapse operators
end
```

Different solvers would also be represented by types, for instance
```julia

abstract QuantumPropagator

type QuantumMonteCarloWaveFunction <: QuantumPropagator
    qe::QuantumEquation
    init_state::QuState
    ntraj::Int
    options (parallel techniques to be put in here) 
end
```     

We could propagate using the following function which would return an instance of QuantumMonteCarlo

```julia
propagate(qe::QuantumEquation, init_state::QuState, tlist, method=:qmcwf)
       # this would return an instance of QuantumMonteCarlo defined above.

#on similar lines

propagate(qe::QuantumEquation, init_state::QuState, tlist, method=:RK)
       # this would return an instance of RK method for quantum state evolution.
``` 
One of the aims would be to use Julia's iterator interface to get states at intermediate steps.

### Prototypes :

At the end of the project, the results would look as follows :

```ijulia

# Birth and Death of Photons in a Cavity.

a = lowerop(5) # annihilates a photon in cavity mode
H = a'*a  # Hamiltonian of cavity mode
init_state = densitymatrix(5) # create initial density matrix (ground state)
kappa = 1.0/0.129 # coupling strength to thermal bath
nth = 0.063 # thermal occupation of thermal bath     

c_ops = [sqrt(kappa * (1 + nth)) * a, sqrt(kappa * nth) * a']

quantum_eqn = ME(H, c_ops)
prop = propagate(quantum_eqn, init_state, tlist, method=:RK)
# prop has the evolved states at various values in tlist, evaluated using the RK-method.

# using iterator methods we could get evolved states at particular time instances, for instance
for qs in prop
   # each iteration step gives use the state qs for the times specified in tlist
   println("<n> = ", dot(a'*a, qs))
end
```

Ref: QuTiP Example [here] (http://nbviewer.ipython.org/github/qutip/qutip-notebooks/blob/master/examples/example-pho ton_birth_death.ipynb)

---
## Timeline :

Community Bonding Period :
* Thorough understanding of the various algorithms that need to be implemented.

* Enhance QuBase with necessary machinery i.e., additional operators (spin operators, pauli spin 1/2 operators) and Density Matrix implementation.

* Thorough understanding of the various packages to be used in the development of the solvers i.e., Parallelization techniques and packages based on it, ODE solver packages.

Goal : Complete the implementation of required machinery. This would play a very crucial role in setting the pace for the project. 

Week 1 - Week 3:

* Implementation of steady state solvers. This should setup the project as most of the machinery is already present or has been worked upon during the Community Bonding Period. Add sufficient testing machinery and document.

Goal : Complete implementation of steady state solvers with checks from QuTiP.

Week 4 - Week 5:

* Implementation of exponential methods, both time dependent and time independent. Tests and Documentation to be added. Start work on RK methods.

Goal : Complete implementation of exponential methods, start work on RK methods.

Week 6 - Week 7:

* Complete the implementation related to RK methods. Tests and Documentation to be added.

Goal : Complete implementation related to RK methods, start work on Quantum Monte Carlo.

Week 8 - Week 10:

* Complete the implementation related to Quantum Monte Carlo methods including tests and documentation.

* Start work on the parallelization and optimization of currently implemented methods.

Goal : Complete implementation of Quantum Monte Carlo methods. Focus on efficiency and parallelization techniques.

Week 11 - Week 13:

* Focus on efficiency and complete the work related to parallelization.

Goal : Efficiency of the implemented algorithms and parallelization techniques.

Week 14:

* Work on the documentation, fixing bugs and polishing the work to be ready for Master.

---
### Post GSoC :
I would love to continue contributing to Julia Quantum :)
