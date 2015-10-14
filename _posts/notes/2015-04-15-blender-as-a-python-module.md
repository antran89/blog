---
layout: article
title: "Blender as a Python Module"
date: 2015-04-15T14:33:32-0700
modified: 2015-10-14T01:40:22-0700
categories: research
excerpt:
comments: true
tags:
toc: true
ads: true
image:
  feature:
  teaser:
---

Blender is an open-source 3D animation suite that has a large community. It
allows a python scripting inside the tool. It supports importing the rendering
engine in a system python outside the blender python [^1].

A stackoverflow answer summarized the steps you have to take to compile the engine [^2].

As a side note, if you are importing a wavefront (.obj) file, you might want to
enable texture shading with <kbd>Alt</kbd> + <kbd>z</kbd> [^3].

[^1]: http://wiki.blender.org/index.php/User%3aIdeasman42/BlenderAsPyModule
[^2]: http://stackoverflow.com/questions/10972637/how-can-i-access-bpy-in-standard-python-console-bpy-is-the-blender-python-thin#answer-11102681
[^3]: http://blender.stackexchange.com/questions/5110/how-to-import-a-mtl-file
