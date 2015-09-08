[NumFOCUS](http://numfocus.org/)
is a public charity in the United States
that supports and promotes world-class, innovative, open source scientific software
(specially the scientific Python stack, [Julia](http://julialang.org/) and [rOpenSci](http://ropensci.org/)).

This is the first time that NumFOCUS participated in Google Summer of Code (GSoC)
although many of the projects that NumFOCUS support participated in many
previous editions as mentoring organizations or under the umbrella of one
mentoring organization (for example the [Python Software
Foundation](https://www.python.org/psf/)).

The students working with us completed three projects
and we are grateful for their incredible work,
the many hours that their mentors spent during the summer
and the amazing people that helped us with the GSoC application.

#   Cython API for DyND

[DyND](https://github.com/libdynd/libdynd) is a C++ library for dynamic, multidimensional arrays.
The motivation for this project was to make DyND available
for the Python scientific stack,
that uses Cython extensively,
without having to go through the trouble of manually looping through each portion of an array.

If you have ever use Cython you know that this was not a easy
project to develop. We are happy that Ian completed the project
even though he spent many hours hunting for bugs and reorganizing code
(something that he did not expect).

Read more about it at [Ian's post](https://insertinterestingnamehere.github.io/posts/gsoc-concluding-thoughts.html).

#   Enhance AMY, a workshop-management platform for Software Carpentry

The [Software Carpentry Foundation](http://software-carpentry.org/scf/index.html) (SCF)
is a non-profit volunteer organization
whose members teach researchers basic software skills.
SCF started to run over a hundred workshops worldwide a year
and managing the workshops became a problem.

During the summer, Piotr worked to enhance [AMY](https://github.com/swcarpentry/amy/),
a Django application that manages SCF's workshops,
adding many new features, fixing bugs
and helping SCF's program coordinators to keep updated
with all the changes in AMY.

The option to create yet another tool from the scratch
instead of use a CRM solution like [CiviCRM](https://civicrm.org/)
was based on the fact that (1) most of the SCF members have some knowledge of
Python so they could help maintain AMY in the long run
and (2) an small Django application could fit better for
others organizations running software training like
[PyLadies](http://www.pyladies.com/) and [Django Girls](http://www.pyladies.com/).

Read more about it at [Piotr's post](http://piotr.banaszkiewicz.org/blog/2015/09/05/amy-update-7/).

#   JuliaQuantum: Framework for solvers

[Julia](http://julialang.org/) is a high-level,
high-performance dynamic programming language for technical computing,
with syntax that is familiar to users of other technical computing environments.
In the last few years Julia got many third party libraries,
which integrate external libraries or use native implementations in Julia.
One of those efforts is [JuliaQuantum](https://juliaquantum.github.io/),
which aims to provide tools and frameworks for dealing with problems
from quantum mechanics and quantum information science.

During the summer Amit worked
on a framework for solving dynamical equations for JuliaQuantum.
He integrated several solvers like the Quantum Monte-Carlo Wave Function Method.
The [new interface](https://github.com/JuliaQuantum/QuDynamics.jl) makes it easy to add new solvers
and to test different methods for a given problem.

Read more about it at [Amit's post](https://juliaquantum.github.io/news/2015/08/GSoC2015-Wrap-up-and-Outlook).
