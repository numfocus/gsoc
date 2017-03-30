# PythreeJS_VR
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
HTML - based widgets.  The developers of the Ipywidgets extension PythreeJS,
developed PythreeJS as a bridge between Python widget creation and HTML
widgets with the incorporation of Javascript 3-D illustration.  ThreeJS,
Javascript 3-D library, has the functionality to create virtual reality
environments; coders create the environment, call the proper VR functions,
display the page in a browser and put on the proper VR goggles to be immersed.  
What if we had the ability toc ompute live data into a virtual reality
environment?  The overall main goal of this project to successfully implement
virtual reality functionality in a Jupyter Notebook HTML widget with the use of
Javascript 3-D graphics.

### Technical Details

The following libraries/toolkits will be required throughout this project:
    **PythreeJS**
    **Ipywidgets**
    **Jupyter Notebook**
    **ThreeJS**
    **BackboneJS**
    **RequireJS**

Ipywidgets is coded using the architectural pattern, *Model-View-Controller*.
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

The following block of code will display the source code for
a small Hello World Widget:

'''
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

'''



________________________________________________________________________________

## Schedule of Deliverables

________________________________________________________________________________

## Why This Project?

________________________________________________________________________________

## Closing Remarks

________________________________________________________________________________

## Appendix

The following links below will give further information on the following
tools, toolkits, libraries and other modules I have mentioned throughout
this proposal. They are listed in the order in which they are mentioned

[Science on a Sphere](https://sos.noaa.gov/What_is_SOS/)

[Jupyter Notebook](https://github.com/jupyter/notebook)

[ipywidgets Github](https://github.com/jupyter-widgets/ipywidgets)

[ipywidgets Documentation](https://ipywidgets.readthedocs.io/en/latest/index.html)

[pythreejs Github](https://github.com/jovyan/pythreejs)

[ThreeJS](https://threejs.org/)

[ThreeJS VREffect](https://github.com/mrdoob/three.js/blob/dev/examples/js/effects/VREffect.js)

[ThreeJS VRControls](https://github.com/mrdoob/three.js/blob/dev/examples/js/controls/VRControls.js)

[BackboneJS](http://backbonejs.org/)

[RequireJS](http://requirejs.org/)

________________________________________________________________________________


## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

{{ Delieverables }}

### May 29th - June 3rd

{{ Delieverables }}

### June 5th - June 9th

{{ Delieverables }}

### June 12th - June 16th

{{ Delieverables }}

### June 19th - June 23th, **End of Phase 1**

{{ Delieverables }}

### June 26 - June 30th, **Begin of Phase 2**

{{ Delieverables }}

### July 3rd - July 7th

{{ Delieverables }}

### July 10th - July 14th

{{ Delieverables }}

### July 17th - July 21th, **End of Phase 2**

{{ Delieverables }}

### July 24th - July 28th, **Begin of Phase 3**

{{ Delieverables }}

### July 31st - August 4th

{{ Delieverables }}

### August 7th - August 11th

{{ Delieverables }}

### August 14th - August 18th

{{ Delieverables }}

### August 21st - August 25th, **Final Week**

{{ Delieverables }}

### August 28th - August 29th, **Submit final work**

## Future works

{{ Future works }}

## Why this project?

{{ Why you want to do this project? }}
