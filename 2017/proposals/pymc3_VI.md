# Extend Variational Inference Methods in PyMC3

## Abstract
I'm 3d year undergradate student from Moscow State University, Economics faculty. I'm very interested in bayesian machine learning, especialy in scalable variational methods.  

Variational inference is a great method that can be potentially scaled on large problems. My recent contributions ([Implement OPVI](https://github.com/pymc-devs/pymc3/pull/1694)) to PyMC3 created a good basis for extending them even further. I also have a side project [Gelato](https://github.com/ferrine/gelato) for using pymc3 in neural networks. My future plans are the following:
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

As support material I'l use papers from arXiv:

[Variational Inference with Normalizing Flows](https://arxiv.org/abs/1505.05770)
[Improving Variational Auto-Encoders using Householder Flow](https://arxiv.org/abs/1611.09630)
[Operator Variational Inference](https://arxiv.org/abs/1610.09033)

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

Merge PR [Implement OPVI](https://github.com/pymc-devs/pymc3/pull/1694)

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

I'm a great fan of bayesian statistics and like develop great things

