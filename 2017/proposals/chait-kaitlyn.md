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
HTML - based widgets.  The developers of the Ipywidgets extension PythreeJS,
developed PythreeJS as a bridge between Python widget creation and HTML
widgets with the incorporation of Javascript 3-D illustration.  ThreeJS,
Javascript 3-D library, has the functionality to create virtual reality
environments; coders create the environment, call the proper VR functions,
display the page in a browser and put on the proper VR goggles to be immersed.

What if we had the ability to compute live data into a virtual reality
environment?

The overall main goal of this project to successfully implement
virtual reality functionality in a Jupyter Notebook HTML widget with the use of
Javascript 3-D graphics.

### Technical Details

The following libraries/toolkits will be required throughout this project:
    **Ipywidgets**
    **ThreeJS**
    **PythreeJS**
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

Now that we have established the fundamentak structure of ipywidgets, let's
dive into ThreeJS and PythreeJS.

**ThreeJS** is a Javascript 3-D graphics library that can used to code infinite
3-D illustrations and environments.  Within this library there are two
functions that can be used together to  render the environment and
control the "camera" in a virtual reality application.

The following link will take you to a simple demo hosted from my personal
Github page.  This demo is uses ThreeJS and can be viewed using any VR enabling
device so long as the browser is webVR enabled.
[Simple Demo](https://katierose1029.github.io/SimpleDemo/index.html)

*VREffect*: ThreeJS function that renders the page to have a 'stereo effect'.
VREffect is coded so that the web page is rendered so that when a users
puts on virtual reality glasses the eyes see the environment in VR instead
two images. The eyes are 'tricked' into seeing one image instead of two.
*VRControls*: ThreeJS function that controls the movement of the camera
as a user moves his/her head throughout exploring the virtual environment.

This then brings me to reintroducing **PythreeJS**, a Python module
that extends ipywidgets and bridges ThreeJS to Python widgets.  In creating
this bridge, PythreeJS uses the Jupyter Notebook infrastructure to create
HTML widgets that contain ThreeJS graphics.  A coder can use PythreeJS to
create a geometric shape (spheres, cube, and other parametric shapes) and
mesh to texturize it.

So the question associated with the objective of this project is,
"How can we incorporate ThreeJS VR functionality in a PythreeJS Widget?"

I have been thinking about this for a while myself and have come up with
at least two possible solutions:
**Solution 1:**
    In implementing VR functionality into PythreeJS I think it would make sense
    to recreate Threejs.VREffect() and Threejs.VRControls() as a separate module.
    To do this, I would reverse engineer these functions and attempt to
    code them in terms of *jupyter-js-widgets*.
    *Threejs.VREffect()* takes in a *Threejs.WebGLRenderer()* and
    *Threejs.VRControls()* takes in a *Threejs.PerspectiveCamera()* in
    its parameters in order to work properly.  In the following code
    block, I will show a prototype os how this solution would be implemented
    and tested in a Jupyter Notebook.

~~~~
Backend:

import ipywidgets as widgets
from traitlets import Unicode, validate

class VREffectWidget(DOMWidget):
    _view_module = Unicode(npm_pkg_name).tag(sync=True)
    _model_module = Unicode(npm_pkg_name).tag(sync=True)
    _view_name = Unicode('VREffectView').tag(sync=True)
    _model_name = Unicode('VREffectModel').tag(sync=True)
    #Renderer
    renderer = Instance(Renderer).tag(sync=True, **widget_serialization)

class VRControlsWidget(Controls):
    _view_module = Unicode(npm_pkg_name).tag(sync=True)
    _model_module = Unicode(npm_pkg_name).tag(sync=True)
    _view_name = Unicode('VRControlsView').tag(sync=True)
    _model_name = Unicode('VRControlsModel').tag(sync=True)

    # controlling will be set to an instance of Camera
    # Camera objects are of type Object3d
    controlling = Instance(Object3d, allow_none=True).tag(sync=True,
                                **widget_serialization)


________________________________________________________________________________

Frontend:

define(["jupyter-js-widgets", "underscore", "three"],
        function(widgets, _, THREE) {

    window.THREE = THREE;
    require("./examples/js/renderers/Projector.js");
    require("./examples/js/renderers/CanvasRenderer.js");
    require("./examples/js/controls/OrbitControls.js");
    require("./examples/js/controls/MomentumCameraControls.js");
    require("./examples/js/controls/TrackballControls.js");
    var $ = require("jquery");

    var VREffectView = widgets.DOMWidgetView.extend({
        render : function() {},
        /*other functions will go below here but render function
        is the most important one to override*/    
    });

    /*ThreeView is variable that extends widgets.WidgetView; this extension
    creates template for further PythreeJS variables.*/
    var VRControlsView =  ThreeView.extend({
        render : function() {},
        /*other functions will go below here*/
    });

    var VREffectModel = widgets.DOMWidgetModel.extend({
        defaults: _.extend({}, widgets.WidgetModel.prototype.defaults, {
            _view_module: 'jupyter-threejs',
            _model_module: 'jupyter-threejs',

            _view_name: 'VREffectView',
            _model_name: 'VREffectModel',
        })
    });

    /*Controls Model is a template for instances that control cameras,
    and other stances of Object3d alike.*/
    var VRControlsModel = ControlsModel.extend({
        defaults: _.extend({}, widgets.WidgetModel.prototype.defaults, {
            _view_module: 'jupyter-threejs',
            _model_module: 'jupyter-threejs',

            _view_name: 'VRControlsView',
            _model_name: 'VRControlsModel',
            controlling: null
        })
    }, {
        serializers: _.extend({
            controlling: { deserialize: widgets.unpack_models }
        }, widgets.WidgetModel.serializers)
    });

    return {
        VREffectView : VREffectView,
        VRControlsView : VRControlsView,
        VREffectModel : VREffectModel,
        VRControlsModel : VRControlsModel,
    };
};
________________________________________________________________________________

Jupyter Notebook: This code was made using Jupyter Notebook.

{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "#Import Statements\n",
    "import pythreejs as py3\n",
    "from PIL import Image\n",
    "from IPython.display import display\n",
    "from ipywidgets import HTML, Text, Controller\n",
    "from traitlets import link, dlink\n",
    "from traitlets.traitlets import _validate_link"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "def create_sphere(fileName):\n",
    "    \n",
    "    def createImageTexture(fileName):\n",
    "        texture = py3.ImageTexture(imageuri = fileName)\n",
    "        return texture\n",
    "        \n",
    "    def sphereMesh(texture):\n",
    "        sphereGeometry = py3.SphereGeometry(radius = 1)\n",
    "        sphereMaterial = py3.BasicMaterial(map = texture)\n",
    "        sphere = py3.Mesh(geometry = sphereGeometry, material = sphereMaterial)\n",
    "        return sphere\n",
    "    \n",
    "    image = createImageTexture(fileName)\n",
    "    sphere = sphereMesh(image)\n",
    "    \n",
    "    c = py3.PerspectiveCamera(position=[0,0,40])\n",
    "    s = py3.Scene(children=[sphere])\n",
    "    orbit_controls = py3.OrbitControls(controlling=c)\n",
    "    fly_controls = py3.FlyControls(controlling=c)\n",
    "    renderer = py3.Renderer(renderer_type='webgl',camera=c, scene= s, background='black', controls=[fly_controls])\n",
    "    display(renderer)\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {
    "collapsed": false
   },
   "outputs": [
    {
     "data": {
      "application/vnd.jupyter.widget-view+json": {
       "model_id": "47695249bb3d4f5389a9f9960d000dc4"
      }
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "create_sphere('4.png')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "#We could possibly test VREffect and VRControls before \n",
    "#the line display(renderer) or after\n",
    "effect = py3.VREffect(renderer)\n",
    "controls = py3.VRControls(c)"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.0"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}


~~~~


**Solution 2:**






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
