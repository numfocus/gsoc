# Online Non Negative Matrix Factorization

## Name and Contact Information 

**Name :**  Shubham Bhardwaj </br>
**University :** [VIT University](http://www.vit.ac.in/), India </br>
**Email :** shubham.bhardwaj4245@gmail.com </br></br>
**Github/ Gitter :** [shubham0704](https://github.com/shubham0704) </br>

## Technical Details

### Abstract
Recent research trends in NLP applications demand learning algorithms to have online nature. This comes with the rise of boom of large amount of data flooding in the main memory at the same time. Algorithms like NNMF are extensively by the used the industry and community to perform tasks like document cluster, image representation, feature reduction, speech denoising and much more in fields like -  astronomy, bioinformatics, nuclear imaging etc. Multiple implementations of the NNMF algorithm exist online  - 
1. [sckit-learn](http://scikit-learn.org/stable/modules/generated/sklearn.decomposition.NMF.html) - not-online -> **Python**
2. [libmf](http://www.csie.ntu.edu.tw/~cjlin/libmf/) - online -> **R**

**Goal of this project -**
1. Identify and re-implementing the frequently used and time
consuming part in Cython using standard profiling methods. 
2. Take inspiration from the above implementations and aim to
refactor/ reorganize. 
3. Test the finalized algorithm to have least number of calls between workers (minimize communication costs), idling.
4. Test the online learning feature of the algorithm on multiple datasets.
5. Improve the overall performance and identify other potential bottlenecks.
6. Write a simple-crisp documentation consistent with the gensim format.

## Technical Details

**Introduction**
The problem of NNMF is given as follows - 
Given a n x m matrix, R find 2 matrix P(n x k) and Q(m x k) with k dimensional features such that the product approximates R.
Taking P as the features matrix and Q as the matrix their dot product gives the sum of latent features by weights for each of the elements in the original matrix. The matrix P and Q returned by this algorithm are Non-Negative.

The DSGD algorithm for finding proper P and Q 
```
Intialize P and Q with small random numbers
for step until max_steps:
    for row, col in R:
        if R[row][col] > 0:
            compute error of element
            compute gradient from error
            update P and Q with new entry
        compute total error
        if error < some threshold:
            break
    return P, Q.T
```
Instead of computing error of each element which would make this serial and allow no room for parallelization initial approaches sought to randomly sample a sub-set of elements from the R matrix to update the corresponding p & q values.
The algorithm proposed in the paper follows the best methodology to update the values which is lock-free and ensures memory continuity.

Deliverables-
1. Implement Lock-free scheduling - Follow the DSGD(distributed SGD) algorithm  to grid R (ratings matrix) into blocks and design a scheduler to keep s threads busy in running a set of independent blocks.
2. Implement partial random sampling - to ensure memory continuity
i.e -  selects ratings (in rating matrix R) in a block orderly but randomizes the selection of blocks.

## Timeline

### May 1th - May 28th, **Community Bonding Period**

- Before starting with the project, I will study the scikit-api and their NNMF code along with the R code provided by libmf.
- I would start contributing to the gensim community fixing bugs and gain more familiarity with their internal API.
- To keep track of my project, I will be posting my weekly blog.

### May 29th - June 3rd

- Start writing a basic version of NNMF.
- Identify potential bottlenecks using standard profiling methods

### June 5th - June 16th

- Write the cython implementation of the potential bottlenecks
- Add a jupyter notebook for review for the community to review the initial implementation for suggesting changes.

### June 19th - June 23th, End of Phase 1

- Make required changes
- Procure the datasets required for testing the model.
- Post the evaluation results for review and reiterate.

### June 26 - June 30th, Begin of Phase 2

- Test the final version of the algorithm on corner cases using stress testing mechanism
- Test the model and modify to minimize communication costs and idling.

### July 3rd - July 14th

- Discuss examples to be added to documentation. The design and features for the visualizations will be finalized in this period.
- Decide the API design, figuring out what options can be provided to the user.


### July 17th - July 21th, End of Phase 2
- Finalize the API design, figuring out what options can be provided to the user and how to make it decoupled to allow ease of use.

### July 24th - July 28th, Begin of Phase 3

- Write the API.

### July 31st - August 11th

- Write the notebook tutorial explaining the usage of topic model visualization.
- Blog about what data attributes can be visualized and how it can help analyze or
compare across the topic models.

### August 14th - August 18th

- Address reviews on example notebook tutorials.
- Address review comments on  performance stats notebooks and other visualization
tutorials.

### August 21st - August 25th, **Final Week**

- Last week will involve code cleaning, adding proper docstrings, fixing possible bugs or handling corner cases.

### August 28th - August 29th, **Submit final work**

- Get the PRs merged



## Open-source contributions
I had contributed to scikit-learn last year. List of pull requests -
1. https://github.com/scikit-learn/scikit-learn/pull/8513
2. https://github.com/scikit-learn/scikit-learn/pull/8253
3. https://github.com/scikit-learn/scikit-learn/pull/8294 
4. https://github.com/scikit-learn/scikit-learn/pull/8585  - [Unmerged feature]

I gave a small pr a while ago to Rasa-NLU -
1. https://github.com/RasaHQ/rasa_nlu/pull/556 - [Unmerged, but a useful script]

I am an active member of Google Developers Groups. We strive to devote most of our leisure time developing quality open-source projects which benefit the community and for fun :)

## Past experience with gensim

During last year I transitioned from a developer to a developer/researcher while working on a creating chatbot. Whatever I did, I was never satisfied with my product. I was working with a team to develop a personal assistant for our university students. I read madly about different QA models, asking questions on S.O, R.G and found that it is still an unsolved problem, requiring tedious hand engineering if you have less data. Somewhere along the line I was introduced to gensim by the internet and after that it became my constant companion for my experiments. Mostly I used to work with document, word similarity in order to incorporate the featurizer layer it into our bots NLU unit. I wish to learn the internals on how does gensim work so fast compared to other libraries and learn that skill.

## What makes me wish to contribute to gensim

Gensim is a great library. I have used it a lot and wish to give back. GSOC with gensim would help me fully commit myself to provide the time and effort without worrying about other agendas. Last year at scikit-learn I had the opportunity to meet wonderful people who helped me develop a lot, but, unfortunately could not devote their time in the summer. Seeing the enthusiastic and dynamic nature of the community, I wish to give it a try again.

**Reasons to choose this project**
1. I have some experience with parallelizing code. 
1.1 In an ongoing research project we are working on parallizing a meta-heuristic algorithm (GSO - Galactic Swarm Optimization) using Numba. See [here](https://github.com/shubham0704/parallelGSO)
1.2 Implementing Scalable Kmeans ++ [#8585](https://github.com/scikit-learn/scikit-learn/pull/8585) .
2. Familiarity with scikit-learn API
It would be easier for me to understand the anomalies of their implementation and work on a better implementation which is online and out-of-core.

## Reference papers to be used -
[1] https://www.csie.ntu.edu.tw/~cjlin/papers/libmf/libmf_journal.pdf
[2] http://www.cs.cornell.edu/~chenhao/pub/online-NMF.pdf




