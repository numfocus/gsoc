# Matplotlib Serialization
### Kaitlyn Chait


## Contents
    1. Opening Details
        * Contact Information
        * Brief Introduction
        * Research Experience & Programming Skills
    2. Project Details
        * Abstract
        * Technical Details
        * Future Works
    3. Schedule of Deliverables
        * Community Bonding Period & Phase 1
        * Phase 2
        * Phase 3
    4. Why This Project?
    5. Closing Remarks
    6. Appendix
________________________________________________________________________________

## Opening Details

#### Contact Information:
    * Personal Email: katiec1029@gmail.com
    * School Email: kchait000@citymail.cuny.edu
    * [Personal Github](https://github.com/katierose1029)
    * Cell Phone: (718) 594 4909

### Brief Introduction:
My name is Kaitlyn Chait and I am a Computer Science student at The City
University of New York at The City College of New York.  When I was 15,
the high school I attended thought it was a great suggestion for me to take
my first computer science course, and I can most definitely say they were
correct.  In my opinion, the field of computer science is appealing in its
various challenges and satisfying success.  There is always a new challenge
to encounter and infinite possibilities to have success in solving the problem.

During my free time, I enjoy thrilling sports such as skiing, listening
to various genres of music and attending live music concerts.  

### Research Experience & Programming Skills:
I have been working with my mentor Dr.Michael Grossberg at the City College
of New York.  He was the one that came to me with original idea to comprise
a demonstration of a Virtual Reality application of Science on a Sphere.
Science on a Sphere is an education tool developed by the National
Oceanic Atmospheric Administration to help illustrate climate change,
changes in ocean temperature, and other weather pattens of the sorts.
After the success of our first demonstration, we wanted to expand it to
plotting live data in Jupyter notebooks and other scripts alike.

Programming Skills:
    * Backend: Python, Java
    * Databases: MYSQL
    * Frontend: Javascript, HTML

________________________________________________________________________________

## Project Details

### Abstract
Jupyter Notebook allow coders to compute data in an web - based interactive
environment.  Ipywidgets is a module that extends Jupyter Notebooks and
Ipython Kernel in that a coder has the ability to create interactive
HTML - based widgets.  The following link will bring you to

[MEP25](https://github.com/matplotlib/matplotlib/wiki/MEP25#id7)


### Technical Details

The following libraries/toolkits will be required throughout this project:
    **Ipywidgets**
    **Jupyter Notebook**
    **BackboneJS**
    **RequireJS**

**Ipywidgets** is coded using the architectural pattern, *Model-View-Controller*.
Components of a Model-View-Controller application:
    *Model*: describes the manipulation/storage of the data and the user input
    *View*: output representation of the data; displays the data that has been
            manipulated and stored
    *Controller*: controls how the user inputs are controlled between model
            and view
    [MVC Diagram](https://i.stack.imgur.com/ocEWx.png)

There are a variety of widgets already programmed into Ipywidgets, however
coders can extend this library to create their own custom widgets.  The
following link is a diagram of how the Ipython Kernel (Backend) interacts with
the HTML widget (Frontend): [Ipywidget Diagram](http://ipywidgets.readthedocs.io/en/latest/_images/WidgetModelView.png)

What the diagram does not show you is that Ipywidgets relies heavily on the
Javascript library BackboneJS.  BackboneJS is a library that gives coders
control of the structure between how models and their corresponding views
interact. In Ipywidgets, widgets are created in the backend and automatically
synchronized with their corresponding BackboneJS views in the frontend,
the widget that is seen in the notebook.

Within the code block below, you will see the source code to a small
widget called Hello and will display the test 'Hello World!'.
The first half of the code block contains the backend model of the widget
in Python and the second half contains the frontend view in Javascript.
You can see that the model is an extension of the class
*ipywidgets.widgets.DOMWidget*, which is a type of widget that directly
manipulates the DOM of the HTML widget.  Where I have the source code for
the view of the widget in Javascript, the first line of code uses
the Javascript library RequireJS, which is required to load the correct
modules.  The view extends the class *widgets.DOMWidgetView*, which is the
Javascript class that is overridden for the purpose of widget Hello.

~~~~
Python (Backend Model):

import ipywidgets as widgets
from traitlets import Unicode, validate

class HelloWidget(widgets.DOMWidget):
    _view_name = Unicode('HelloView').tag(sync=True)
    _view_module = Unicode('hello').tag(sync=True)

________________________________________________________________________________

Javascript (Frontend View):

require.undef('hello');

define('hello', ["jupyter-js-widgets"], function(widgets) {

    var HelloView = widgets.DOMWidgetView.extend({

        // Render the view.
        render: function() {
            this.el.textContent = 'Hello World!';
        },
    });

    return {
        HelloView: HelloView
    };
});

~~~~
[src](https://ipywidgets.readthedocs.io/en/latest/examples/Widget%20Custom.html)


**Solution:**

~~~~

~~~~

________________________________________________________________________________

## Schedule of Deliverables

**Community Bonding Period:** (*May 1st - May 28th*)

**Phase 1:** (*May 29th - June 23rd*)
    The focus of this stage will be matplotlib serialization.
    By the end of this phase, the following questions will be answered:
        1.

**Phase 2:** (*June 26th - July28th*)

**Phase 3:** (*July 24th - August 25th*)

**Hand in Final Works** (*August 28th - August 29th*)

________________________________________________________________________________

## Why This Project?
I have been determined and motivated to complete this project for a while.
Completing this would be a great addition to my portfolio and can assist me
in my future as a coder.

________________________________________________________________________________

## Closing Remarks


________________________________________________________________________________

## Appendix

The following links below will give further information on the following
tools, toolkits, libraries and other modules I have mentioned throughout
this proposal.

[BackboneJS](http://backbonejs.org/)
[Google Cardboard](https://vr.google.com/cardboard/)
[ipywidgets Github](https://github.com/jupyter-widgets/ipywidgets)
[ipywidgets Documentation](https://ipywidgets.readthedocs.io/en/latest/index.html)
[ipywidgets.widgets.DOMWidget](https://github.com/jupyter-widgets/ipywidgets/blob/master/ipywidgets/widgets/domwidget.py)
[ipywidgets/jupyter-js-widgets](https://github.com/jupyter-widgets/ipywidgets/tree/master/jupyter-js-widgets)
[Jupyter Notebook](https://github.com/jupyter/notebook)
[Oculus Rift](https://www.oculus.com/rift/)
[pythreejs Github](https://github.com/jovyan/pythreejs)
[RequireJS](http://requirejs.org/)
[Science on a Sphere](https://sos.noaa.gov/What_is_SOS/)
[ThreeJS](https://threejs.org/)
[ThreeJS VREffect](https://github.com/mrdoob/three.js/blob/dev/examples/js/effects/VREffect.js)
[ThreeJS VRControls](https://github.com/mrdoob/three.js/blob/dev/examples/js/controls/VRControls.js)

________________________________________________________________________________
