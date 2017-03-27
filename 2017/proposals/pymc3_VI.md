# Extend Variational Inference Methods in PyMC3

## Abstract
Variational inference is a great approach for doing really complex, often intractable bayesian inference in approximate form. Common methods (e.g. ADVI) lack from complexity so that approximate posterior does not reveal the true nature of underlying problem. In some applications it can yield unreliable decisions. 

Recently on NIPS 2017 [OPVI](https://arxiv.org/abs/1610.09033) framework was presented. It generalizes variational inverence so that the problem is build with blocks. The first and essential block is Model itself. Second is Approximation, in some cases $log Q(D)$ is not really needed. Necessity depends on the third and forth part of that black box, Operator and Test Function respectively. 

Operator is like an approach we use, it constructs loss from given Model, Approximation and Test Function. The last one is not needed if we minimize KL Divergence from Q to posterior. As a drawback we need to compute $loq Q(D)$. Sometimes approximation family is intractable and $loq Q(D)$ is not available, here comes LS(Langevin Stein) Operator with a set of test functions.

Test Function has more unintuitive meaning. It is usually used with LS operator and represents all we want from our approximate distribution. For any given vector based function of $z$ LS operator yields zero mean function under posterior. $loq Q(D)$ is no more needed. That opens a door to rich approximation families as neural networks. 

Not only ADVI and Langevin Stein Operator VI is appliable with OPVI framework. Normalizing, Householder Flows fit well for it.

## Motivation

My recent contributions ([Implementing OPVI](https://github.com/pymc-devs/pymc3/pull/1694)) to PyMC3 created a good basis for extending variational inference in PyMC3 even further. I tried to transfer theoretical framework to python code and it succeed. Now main logic is in base classes and all rotines are abstracted and use public interface that is provided by developer. Implementing state-of-the-art methods is now not a challenge, you should just break the problem into 4 blocks described above and implement abstract methods.
 
I also have a side project [Gelato](https://github.com/ferrine/gelato) for using pymc3 in neural networks. So my future plans are the following:

1. Implement Normalizing Flows
2. Implement Householders Flows
3. Implement Langein Stein Operator (not sure in success)
4. Integrate OPVI to Gelato
5. Make it possible to bayesionize pretrained Lasagne model and then fine tune it

## Technical Details

I'm going to use the following libraries:
    
* **Theano**
* **PyMC3**
* **Gelato**
* **Lasagne**
* **NumPy**

As support material I'll use papers from arXiv:

[Variational Inference with Normalizing Flows](https://arxiv.org/abs/1505.05770)
[Improving Variational Auto-Encoders using Householder Flow](https://arxiv.org/abs/1611.09630)
[Operator Variational Inference](https://arxiv.org/abs/1610.09033)

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

Merge PR [Implement OPVI](https://github.com/pymc-devs/pymc3/pull/1694) [**Merged**]

### May 29th - June 10th

Implement Normalizing Flows

### June 11th - June 23th

Implement Householder Flows

### July 3rd - July 7th

Implement Langevin Stein Operator and Test Function for it

### July 10th - July 23d **End of Phase 2**

Get out why it didn't work this winter and try to fix the problem (It was really strange)

### July 24th - July 28th, **Begin of Phase 3**

Prepare environment for toy examples (data, model construction)

### July 31st - August 4th

Prepare draft with working examples

### August 7th - August 18th

Write some text for examples

### August 14th - August 18th

Refine text

### August 21st - August 25th, **Final Week**

Publish a post, optionally present the work on bayesian summer school

### August 28th - August 29th, **Submit final work**

Submit

## Future works

Read arXiv, collect ideas

## Development Experience

Yandex Analyst-Developer Intern (summer 2016), PyMC3 developer

## Other Experiences

Yandex Data Factory Intern (now)

## Why this project?

I'm a great fan of bayesian statistics and see it is usefull for many practical applications. I also love development and good codestyle. This project is interesting for me from both point of views. I'm also planning to use my results for my research projects and work.

