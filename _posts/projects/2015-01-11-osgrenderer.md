---
layout: article
title: "OSG Renderer"
date: 2015-01-12T22:58:48-08:00
modified: 2015-01-22T01:15:04-0800
categories: projects
comments: true
excerpt:
toc: true
tags: [OpenSceneGraph, Renderer]
ads: true
image:
  feature:
  teaser:
---

*Efficient object rendering engine for offscreen rendering using* [OpenSceneGraph 3](https://github.com/openscenegraph/osg)

The renderer contains `python` and `matlab` bindings. The renderer is easy to use and caches CAD models so that it does not load the CAD model every time it renders.

The C++ object is cached using [Mexplus](https://github.com/kyamagu/mexplus), and the matlab wrapper holds the C++ class instance until a user destroys the instance. The Renderer object remains on the memory, and it contains all the loaded CAD models and returns data to MATLAB or python directly. Thus it is optimal for on-the-fly rendering or visualization.

There are two modes for installation: one that require OSG installation, which is recommended, and one that works without OSG installation using prebuilt OSG libraries provided in this repo.

Example
=======

IPython Notebook Demo
---------------------

<http://nbviewer.ipython.org/gist/chrischoy/19f7765c401973d28243>


Python
------

{% highlight python %}
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
from PyRenderer import Renderer

viewport_size_x = 700
viewport_size_y = 700

azimuth = 0
elevation = 0
yaw = 0
distance_ratio = 1
field_of_view = 25

x = Renderer()
x.initialize(['mesh/2012-VW-beetle-turbo.3ds','mesh/Honda-Accord.3ds'],
              viewport_size_x, viewport_size_y)

# Render 
x.setViewpoint(azimuth, elevation, yaw, distance_ratio, field_of_view)
rendering, depth = x.render()

# Flip dimension
rendering = rendering.transpose((2,1,0))
depth = depth.transpose((1,0))

# image show
plt.imshow(rendering)
plt.show()

# depth show
plt.imshow(1-depth)
plt.show()
{% endhighlight %}

Matlab
-----

{% highlight matlab %}
% Add binary path
addpath('./bin');

% Initialize the Matlab object.
rendering_size_x = 700; rendering_size_y = 700; % pixels

azimuth = 90; elevation = 10; yaw = 0;
% Modify distance ratio to control distance from the object
% distance_ratio = 1 is the default
distance_ratio = 0; field_of_view = 25; 

% Setup Renderer
renderer = Renderer();
renderer.initialize({'mesh/2012-VW-beetle-turbo.3ds', ...
        'mesh/Honda-Accord.3ds'},...
        rendering_size_x, rendering_size_y)

% If the output is only the rendering, it renders more efficiently
renderer.setModelIndex(1);
renderer.setViewpoint(azimuth,elevation,yaw,distance_ratio,field_of_view);
[rendering]= renderer.render();
subplot(221);
imagesc(rendering); axis equal; axis tight; axis off;


% Once you initialized the renderer, you can just set 
% the viewpoint and render without loading CAD model again.
renderer.setModelIndex(2);
renderer.setViewpoint(azimuth,elevation,yaw,distance_ratio,field_of_view);
[rendering]= renderer.render();
subplot(222);
imagesc(rendering); axis equal; axis tight; axis off;



% If you give the second output, it renders depth too.
renderer.setModelIndex(1);
renderer.setViewpoint(45,20,0,2,25);

[rendering, depth]= renderer.render();
subplot(223);imagesc(rendering); axis equal; axis off;
subplot(224);imagesc(1-depth); axis equal; axis off; colormap hot;

% Return viewmatrix
renderer.getViewMatrix()

% Return projection matrix
renderer.getProjectionMatrix()

% You must clear the memory before you exit
renderer.delete(); clear renderer;
{% endhighlight %}

Output 

![](https://dl.dropboxusercontent.com/u/57360783/MatlabRenderer/rendering_with_depth.png)


Download
-------

<http://github.com/chrischoy/OSGRenderer>

