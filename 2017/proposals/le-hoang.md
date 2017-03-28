# Alternative computational engines

## Abstract

In recent years, the deep learning world has seen the dramatic rise of many frameworks, namely Theano, TensorFlow, PyTorch, which absolutely accelerate the computing capability of nowadays’ GPUs. There is no doubt that PyMC3 should take advantage of these frameworks for its backend engine in order to make its models run effectively and efficiently. PyMC3 is currently using Theano for its backend to create and compute the graph of the probabilistic model. Therefore, it is the time for looking at the feasibility of using alternative libraries to build PyMC3 models. Generally, I would like to extend PyMC3’s backend option by adding another abstract layer over its computational engine. Thus, in this way, PyMC3 enables end users to choose their backend framework flexibly. I will also document PyMC3’s design overview and developer guide carefully. 
## Technical Details

### Plan Overview:
In order to come up with a clean and elegant design for PyMC3 backend implementation with the help of my mentors, I need to dive deep into PyMC3 source code to understand its architecture and how its modules connect together. In addition, I will discuss with my mentors and PyMC3 developers to design the abstract layers to separate PyMC3 backend and frontend, as well as the API for backend so that most of the PyMC3 existing code can remain the same. Finally, I will add the alternative backend engine and modify PyMC3’s source code as needed. Simultaneously, I will document my progress and the overview of the design for future contributions.

In this project, I plan to spend quite a lot of time on designing an API for backend, which may have a great impact on how we scale our code and its usability later, so that I would like to narrow the scope of this project by adding one alternative backend engine, which is PyTorch. 

### Computaional Libraries: 
Apart from Theano, there are two well-known numerical computation libraries to take into account:

- TensorFlow: created by Google Brain. This library adopts a similar approach with Theano. It means TensorFlow separates the definition of computations from their execution. The computation that will be performed later including: the inputs, the variables, and the operations, which are created as nodes over a computation graph. This static computation graph is powerful when dealing with problems where multiple inputs are scaled to the identical size. However, there are still some case where the dimension of inputs varies a lot, such as parse trees in natural language processing, abstract syntax trees in source code, requiring different computation graphs for each kind of input. That is the reason why recently Google has announced TensorFlow Fold to address these challenges. It allows users to build a static TensorFlow graph that includes a single instance of each operation, but can emulate input graphs of arbitrary size and topology. [1]
- PyTorch: implemented by Facebook. Unlike other, PyTorch is built to be deeply integrated into Python, so working with PyTorch is similar to using use numpy, scipy, or scikit-learn. It is clear that Chainer and DyNet have a great influence on PyTorch’s implementation. They all use dynamic computation graph to build models. The graph structure is defined at runtime, which is so-called Define-by-Run scheme. Also, PyTorch is designed to provide high-level features: tensor computation (like numpy) with strong GPU acceleration and deep neural networks built on a tape-based autograd system. [2]

### Similar Works:

- Edward: is a Python library for probabilistic modeling, inference and criticism. It is built on TensorFlow. [3]
- Keras: is a high-level neural networks API, written in Python and capable of running on top of either TensorFlow or Theano. [4]

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

- Follow pymc-devs gitter to know and understand more pymc 
- Work through examples of PyMC3 on its official website and some other documents to feel more comfortable for using it.
- Study more about probabilistic programming and Bayesian methods.
- Read PyMC3 source code in order to understand its data flow, and architecture.
- Find out all modules that depend on backend.
- Find out how Keras handles multiple backend engines in their source code and documents

### May 29th - June 3rd

- Analyze source code of PyMC3 to see which module is depended on backend (e.g. distribution parts, model.py, …) and which one should be modified first.
- During this period, I will keep in touch with my mentor and the PyMC3 community to discuss more about PyMC3’s architecture and design. 

### June 5th - June 9th

- Come up with the 1st version of backend API design
- Define an abstract layer for back-end engine and provide a clear and simple API based on current PyMC3 code. In this way, it will be easier when PyMC3 want to change to another back-end engine in the future.

### June 12th - June 16th

- Implement the 1st design, as well as refine it when needed
- Modify PyMC3 source code to meet the new API (for example, the distribution parts and model.py)

### June 19th - June 23th, **End of Phase 1**

- Write and run test for new backend
- Run some PyMC3 examples with new backend API
- Document the design
- Draw figures to show the architecture

### June 26 - June 30th, **Begin of Phase 2**

- Find out the differences between Theano and PyTorch in detail
- Design how to integrate PyTorch into the new backend API

### July 3rd - July 7th

- Implement the backend interface using PyTorch

### July 10th - July 14th

- Run tests for PyTorch backend engine
- Make further changes in the code to improve the Functionality, Exception handling, Bug Removal.

### July 17th - July 21th, **End of Phase 2**

- Document my progress and how I add PyTorch to PyMC3
- Maintain a constant touch with PyMC3 developers and let them know about the overall progress

### July 24th - July 28th, **Begin of Phase 3**

- Add JSON option file to PyMC3 (similar to Keras)
- Review and refactor my code

### July 31st - August 4th

- Write and run test with both backend Theano and PyTorch
- Spend most of the time on rigorous testing and bug fixes

### August 7th - August 11th

- Write documentation for the new backend interface
- Review and refactor my code

### August 14th - August 18th

- Write a developer’s guide for PyMC3 with the updated backend

### August 21st - August 25th, **Final Week**

- Prepare to submit my works
- Check everything again before submit

### August 28th - August 29th, **Submit final work**

## Future works

There are a lot of things to do after finishing this project. First, I will be active in PyMC3’s github or community to answer and fix issues when needed. The next step is to try to integrate TensorFlow into the PyMC3’s backend, because TensorFlow is very strong at running on distributed systems. 

## Development Experience

Apart from my graduate study, I also join a big data lab. In our lab, we build the next generation computing engine for big data which run on top of Hadoop and Yarn. I use Scala and Python every day for my works and study. Besides, I also teach myself web development (react, ember,…), because I want to build a visualizing tool for my deep learning research or study.

## Other Experiences

Before attending a full-time Master program in Computer Science and Engineering in POSTECH in Korea, I work for an USA semiconductor company in Vietnam as an electrical engineer who design a chip layout for many devices. 

## Why this project?

I am currently attending Deep Learning class in which I learn many predictive and generative models. Thus, I understand the importance of probabilistic programming and Bayesian methods when using it to build generative models, namely variational auto-encoder. This project offers me a great opportunity to not only study more about Bayesian methods, but also make a contribution to the open source community. I believe that this work will help spread PyMC3 in the scientific community.

## Appendix

- My lab website: http://pl.postech.ac.kr/
- NumFOCUS issue: https://github.com/numfocus/gsoc/issues/215 
- [1] TensorFlow: https://www.tensorflow.org/
- [2] PyTorch: http://pytorch.org/
- [3] Edward: http://edwardlib.org/ 
- [4] Keras: https://keras.io/
