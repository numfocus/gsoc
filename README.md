# Google Summer of Code

| [Sub organizations](https://github.com/numfocus/gsoc/tree/update-docs#sub-organizations) | [IDEAS LIST][IL] | [Student guides][CONTRIBUTING]  |

[NumFOCUS][] will be applying again as an umbrella mentoring organization
for [Google Summer of Code 2020][GSoC]. [NumFOCUS][] supports and
promotes world-class, innovative, open source scientific software.

[Google Summer of Code][GSoC] is an annual open source internship program
sponsored by Google. This repository contains information specific to NumFOCUS'
participation in GSoC. For general information about the competition, including
this year's application timeline and key phases involved, please see the [GSoC
website](https://summerofcode.withgoogle.com/how-it-works/)

<!--
This Git repository stores information about NumFOCUS' participation in
Google Summer of Code 2020 program and previous editions.
-->

This Git repository stores information about NumFOCUS'
application for Google Summer of Code in the current and previous years.

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-generate-toc again -->
**Table of Contents**

- [Students](#students)
- [Sub Organizations](#sub-organizations)
- [Organizations Confirmed Under NumFOCUS Umbrella](#organizations-confirmed-under-numfocus-umbrella)
- [NumFOCUS Projects](#numfocus-projects)
    - [Fiscally Sponsored Projects GSoC Status](#fiscally-sponsored-projects-gsoc-status)
    - [Affiliated Projects GSoC Status](#affiliated-projects-gsoc-status)
    - [Other Projects GSoC Status](#other-projects-gsoc-status)
- [About GSoC](#about-gsoc)

<!-- markdown-toc end -->

## Students

NumFOCUS is participating as a umbrella organization. This means that
you will need to identify a specific project to apply to under the
NumFOCUS umbrella. (Projects are listed below.)

Read [this document][CONTRIBUTING] to learn how to apply for the
GSoC program with NumFOCUS. Please also check out our [ideas list][IL].

For any questions, please open an issue in our [issue tracker][issues]
or send a email to gsoc@numfocus.org, our mailing list address.
Please also consider subscribing to the mailing list at
https://groups.google.com/a/numfocus.org/forum/#!forum/gsoc.

## Sub Organizations

If you want to participate as a sub organization of NumFOCUS please read
this [guide](CONTRIBUTING-mentors.md).

## Organizations Confirmed Under NumFOCUS Umbrella

<!--
The list should contain for each project.
 - A short description
 - link to their website
 - link to ideas page
 - link how to best contact them
 - link to beginners guide
-->

In alphabetic order.

<table>


  <tr>
    <td>
      <img width="300px" src="img/arviz.png"/>
    </td>
    <td>
       <h1>ArviZ</h1>
       <p>
        ArviZ is a Python package for exploratory analysis of Bayesian models. Includes functions for posterior analysis, sample diagnostics, model checking, and comparison. The goal is to provide backend-agnostic tools for diagnostics and visualizations of Bayesian inference in Python, by first converting inference data into xarray objects.
       </p>
       <p>
         <a href="https://arviz-devs.github.io/arviz/">Website</a> | <a href="https://github.com/arviz-devs/arviz/wiki/GSoC-2020-projects">Ideas List</a> | <a href="https://gitter.im/arviz-devs/community"> Contact (Gitter) </a> | <a href="https://github.com/arviz-devs/arviz">Source Code</a>
       </p>
    </td>
  </tr>
  
  <tr>
    <td>
      <img width="300px" src="img/clawpack.png"/>
    </td>
    <td>
       <h1>Clawpack</h1>
       <p>
        Clawpack (“Conservation Laws Package”) is a collection of finite volume methods for linear and nonlinear hyperbolic systems of conservation laws.
       </p>
       <p>
       <a href="https://www.clawpack.org">Website</a> | <a href="">Ideas List</a> | <a href="https://join.slack.com/t/clawpack/shared_invite/enQtNTAwMjQxNTk5NjY5LTJjMzJkMzhjZTQ0NzQ2Y2JkZTg3YWFhN2M4ZjIwMDA3YWNiNDM5NTA2NGNlZDA2YzlmNDkwMzA5ZGFkODgyMzc"> Slack </a> | <a href="https://github.com/clawpack">Source Code</a>
       </p>
    </td>
  </tr>


<tr>
    <td>
      <img width="300px" src="https://avatars2.githubusercontent.com/u/11897326?v=3&s=200"/>
    </td>
    <td>
       <h1>Conda Forge</h1>
       <p>
        A community led collection of recipes, build infrastructure and distributions for the conda package manager.
       </p>
       <p>
         <a href="https://conda-forge.org">Website</a>  | <a href="">Ideas List</a> | <a href="https://gitter.im/conda-forge/conda-forge.github.io"> Gitter </a> | <a href="https://github.com/conda-forge">Source Code</a>
       </p>
    </td>
  </tr>

  <tr>
   <td>
     <img width="300px" src="img/ecodata-retriever.png"/>
   </td>
   <td>
      <h1>Data Retriever</h1>
      <p>
        The Data Retriever is a package manager for data. It downloads, cleans,
        and stores publicly available data, so that analysts spend less time
        cleaning and managing data, and more time analyzing it.
      </p>
      <p>
        <a href="http://www.data-retriever.org/">Website</a>  | <a href=""> Ideas List</a> | <a href="https://gitter.im/weecology/retriever"> Contact (Gitter) | <a href="https://github.com/weecology/retriever">Source Code</a>
      </p>
   </td>
 </tr>
 
  <tr>
   <td>
     <img width="300px" src="img/econark.png"/>
   </td>
   <td>
      <h1>Econ-Ark</h1>
      <p>
        The aim of the Econ-ARK project is to make it easier for social scientists to do ‘structural modeling’ of economic choice behavior by providing a well-documented, open source codebase containing the core techniques in a way that can be relatively easily adapted to address many different questions in both macro- and microeconomics. ‘Structural’ modeling of economic choices aims to identify the logic or reasons behind observed behavior, rather than just describing that behavior statistically.
      </p>
      <p>
        <a href="https://econ-ark.org">Website</a>  | <a href=""> Ideas List</a> | <a href=""> Contact | <a href="https://github.com/econ-ark/HARK">Source Code</a>
      </p>
   </td>
 </tr>

  <tr>
   <td>
     <img width="300px" src="img/fenics.png"/>
   </td>
   <td>
      <h1>FEniCS</h1>
      <p>
        FEniCS is an automated finite element library used to solve equations used in
        modeling, featuring a domain-specific language and automated code generation.
        Users input a problem that looks very much like mathematical notation; FEniCS
        then translates that into computer code. It solves problems for which there is
        no analytical (exact) solution numerically.
      </p>
      <p>
        <a href="https://fenicsproject.org/">Website</a> | <a href="https://fenicsproject.org/community/">Community Page With Access to Slack</a> | <a href =""> Ideas Page | <a href="https://github.com/FEniCS">Source Code</a>
      </p>
   </td>
 </tr>
 
 <tr>
  <td>
    <img width="300px" src="img/nteract.png">
  </td>
  <td>
     <h1>nteract</h1>
     <p>The nteract open source organization is committed to building the future of interactive computing. We are committed to:</p>
     <ul>
     <li>Creating fantastic interactive computing experiences that allow people to collaborate with ease.</li>
     <li>Emphasizing simplicity and composability as core design principles to provide users ideal building blocks for their unique data applications.</li>
     <li>Valuing our contributors and users and cooperating to produce solutions that delight others.</li>
     </ul>
     <p>To fulfill our mission, the nteract open source organization produces SDKs, applications, libraries, and CLIs to help individuals leverage the full power of Jupyter and other interactive computing environments.</p>
     <p>
       <a href="https://nteract.io/">Website</a> | <a href="https://slack.nteract.io/">Contact (Slack Community)</a> | <a href="https://github.com/nteract/nteract/wiki/GSoC-2020-Ideas">Ideas Page</a> | <a href="https://github.com/nteract/nteract"> Source Code (Core SDK and Applications)</a>
     </p>
  </td>
</tr>

<tr>
  <td>
    <img width="300px" src="img/pvlib.png">
  </td>
  <td>
     <h1>pvlib</h1>
     <p>pvlib python provides a set of functions and classes for simulating the performance of photovoltaic energy systems.</p>
     <p>
       <a href="https://pvlib-python.readthedocs.io/en/stable/">Website</a> | <a href="">Contact</a> | <a href="">Ideas Page</a> | <a href="https://github.com/pvlib/pvlib-python"> Source Code</a>
     </p>
  </td>
</tr>

  <tr>
    <td>
      <img width="300px" src="img/pymc3-logo.png">
    </td>
    <td>
       <h1>PyMC3</h1>
       <p>PyMC3 is a python module for Bayesian statistical modeling and model fitting which focuses on advanced Markov chain Monte Carlo and variational fitting algorithms. Its flexibility and extensibility make it applicable to a large suite of problems.</p>
       <p>
         <a href="https://pymc-devs.github.io/pymc3/">Website</a> | <a href="https://discourse.pymc.io/">discourse</a> | <a href="https://github.com/pymc-devs/pymc3/wiki/GSoC-2020-projects">Ideas Page</a> | <a href="https://github.com/pymc-devs/pymc3"> Source Code</a>
       </p>
    </td>
  </tr>

  <tr>
    <td>
      <img width="300px" src="img/signac.png">
    </td>
    <td>
       <h1>signac</h1>
       <p> The signac framework is a complete solution for managing workflows operating on file-based data designed to scale to HPC systems. </p>
       <p>
         <a href="https://signac.io">Website</a> | <a href="https://gitter.im/signac/Lobby">Gitter</a> | <a href="https://github.com/glotzerlab/signac/wiki/GSoC-2020-Projects">Ideas Page</a> | <a href="https://github.com/glotzerlab/signac"> Source Code</a>
       </p>
    </td>
  </tr>

</table>


## NumFOCUS Organizations

Not all NumFOCUS organizations participate under our umbrella. These lists show
which organizations are participating with GSoC and where you can find
information how to work with them.

### Fiscally Sponsored Organizations GSoC Status


| Project                         | Status                           | Ideas Pages                                         |
| -------                         | ------                           | -----------                                         |
| [AstroPy]                       | Unknown     |    |
| [Blosc]                         | Unknown |
| [Cantera]                       | Unknown |                          |
| [Econ-ARK][Econ-ARK]            | Applying under NumFOCUS umbrella |                                                     |
| [FEniCS Project][FEniCSproject] | Applying under NumFOCUS umbrella |  |
| [IPython]                       | Unknown                          |                                                     |
| [Julia]                         | Unknown |           |
| [JuMP]                          | Unknown |                                         |
| [Matplotlib]                    | Unknown |                                                 |
| [nteract]                       | Applying under NumFOCUS umbrella | https://github.com/nteract/nteract/wiki/GSoC-2020-Ideas |
| [NumPy]                         | Unknown                |                                                     |
| [Open Journals][theoj]          | Unknown                          |                                                     |
| [Project Jupyter][Jupyter]      | Unknown                          |                                                     |
| [pandas]                        | Unknown                          |                                                     |
| [PyMC3](pymc3)                  | Applying under NumFOCUS umbrella | https://github.com/pymc-devs/pymc3/wiki/GSoC-2020-projects |
| [PyTables]                      | Unknown                          |                                                     |
| [QuantEcon]                     | Unknown                          |                                                     |
| [rOpenSci]                      | Unknown                |                                                     |
| [Shogun]                        | Unknown |                                                    |
| [SunPy]                         | Unknown |  |
| [SymPy]                         | Unknown |  |
| [Stan]                          | Unknown |                                                     |
| [yt]                            | Unknown |                                                     |

### Affiliated Organizations GSoC Status

| Project              | Status                           | Ideas Pages |
| -------              | ------                           | ----------- |
| [ArviZ]              | Applying under NumFOCUS umbrella |  https://github.com/arviz-devs/arviz/wiki/GSoC-2020-projects           |
| [Bokeh]              | Unknown  |             |
| [Chainer]            | Unknown  |            |
| [Clawpack]           | Applying under NumFOCUS umbrella |           |
| [Conda]              | Unknown                          |             |
| [conda-forge]        |  Applying under NumFOCUS umbrella |             |
| [CuPy]               | Unknown  |           |
| [Cython]             | Unknown                          |             |
| [Dash]               | Unknown |         |
| [Data Retriever][DR] | Applying under NumFOCUS umbrella | |
| [Dask]               | Unknown |     |
| [DyND]               | Unknown                          |             |
| [Gensim]             | Unknown                          |             |
| [MDAnalysis]         | Unknown |  |
| [Numba]              | Unknown                          |             |
| [Orange]             | Unknown                          |             |
| [Pomegranate]        | Unknown                          |             |
| [pvlib]              | Applying under NumFOCUS umbrella | https://github.com/pvlib/pvlib-python/wiki/GSoC-2020-Project           |
| [PythonXY]           | Unknown                          |             |
| [QuTiP]              | Unknown |        |
| [SciPy]              | Unknown                          |             |
| [scikit-image]       | Unknown                          |             |
| [scikit-bio]         | Unknown                          |             |
| [scikit-learn]       | Unknown                          |             |
| [signac]             | Applying under NumFOCUS umbrella |  |
| [Statmodels]         | Unknown                          |             |
| [Spack]              | Unknown                          |             |
| [Spyder]             | Unknown |    |
| [Theano]             | Unknown                          |             |
| [xarray]             | Unknown                          |             |
| [Yellowbrick]        | Unknown |       |

[ArviZ]: https://arviz-devs.github.io/arviz/
[AstroPy]: https://www.astropy.org/
[Blosc]: https://www.blosc.org/
[Bokeh]: https://docs.bokeh.org/en/latest/
[cantera]:  https://cantera.org/index.html
[Chainer]: https://chainer.org/
[Clawpack]: https://www.clawpack.org/
[CONTRIBUTING]: CONTRIBUTING-students.md
[Conda]: https://github.com/conda/conda
[conda-forge]: https://conda-forge.org
[CuPy]: https://cupy.chainer.org/
[Cython]: https://cython.org/
[CF]: https://conda-forge.github.io/
[Dash]: https://plot.ly/dash/
[Dask]: https://dask.org/
[DR]: https://www.data-retriever.org/
[DyND]: http://libdynd.org/
[Econ-ARK]: https://econ-ark.github.io/HARK/
[FEniCSproject]: https://fenicsproject.org/
[Gensim]: https://radimrehurek.com/gensim/
[GSoC]: https://summerofcode.withgoogle.com/
[IL]: 2020/ideas-list.md
[IPython]: https://ipython.org/
[issues]: https://github.com/numfocus/gsoc/issues
[Julia]: https://julialang.org/
[JuMP]: https://www.juliaopt.org/
[Jupyter]: https://jupyter.org/
[Matplotlib]: https://matplotlib.org/
[MDAnalysis]: https://www.mdanalysis.org/
[Numba]: http://numba.pydata.org/
[NumFOCUS-Projects]: https://numfocus.org/sponsored-projects
[NumFOCUS]: https://numfocus.org/
[NumPy]: https://numpy.org/
[nteract]: https://nteract.io/
[theoj]: http://www.theoj.org
[Orange]: http://orange.biolab.si/
[pandas]: https://pandas.pydata.org/
[Pomegranate]: https://pomegranate.readthedocs.io/en/latest/
[pvlib]: https://pvlib-python.readthedocs.io/en/stable/
[PyTables]: http://pytables.github.com/
[PythonXY]: http://python-xy.github.io/
[QuTiP]: http://qutip.org/
[rOpenSci]: https://ropensci.org/
[quantecon]: https://quantecon.org/
[SCF]: https://carpentries.org/
[scikit-bio]: http://scikit-bio.org/
[scikit-image]: https://scikit-image.org/
[scikit-learn]: https://scikit-learn.org/stable/
[SciPy]: https://www.scipy.org/
[signac]: https://signac.io
[Spack]: https://spack.io
[Spyder]: https://www.spyder-ide.org/
[Statmodels]: http://www.statsmodels.org/stable/index.html
[Stan]: https://mc-stan.org/
[Shogun]: https://www.shogun-toolbox.org/
[SunPy]: https://sunpy.org/
[SymPy]: https://www.sympy.org/
[Theano]: http://deeplearning.net/software/theano/
[xarray]: http://xarray.pydata.org/
[Yellowbrick]: http://www.scikit-yb.org/en/latest/
[yt]: https://yt-project.org/
