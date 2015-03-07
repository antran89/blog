---
layout: article
title: "Occlusion robust pose and model estimation using Gaussian Process Latent Variable Model"
date: 2013-11-23T22:58:48-08:00
modified:
categories: projects
comments: true
excerpt:
toc: true
tags: []
ads: true
image:
  feature: ../external/gplvm_occlusion_robust/feature.png
  teaser: ../external/gplvm_occlusion_robust/feature.png
---


Objective: finding the right pose and the model

Finding the right CAD model that fits an object in an image is essentially discrete in nature. We have to go through all the models in the training set CAD models and match using a metric and choose the CAD model that maximizes the metric. However, such nearest neighbor scheme would only work if we have the right model in the training. Instead, we can find a subspace that spans the CAD models and relax the problem to continuous domain. In this project, we use Gaussian Process Latent Variable Model to find the low dimensional subspace that spans CAD models and fits the estimated model to an object by maximzing the overlap between the foreground of the object and the projection of the estimated model.

## Result

<table>
<thead>
<tr><td>Initialization</td><td>Result</td>
</tr>
</thead>
<tbody>
<tr>
<td><img src="{{ site.url }}/external/gplvm_occlusion_robust/multi_init.png" ></td>
<td><img src="{{ site.url }}/external/gplvm_occlusion_robust/multi_const.png" ></td>
</tr>
<tr>
<td><img src="{{ site.url }}/external/gplvm_occlusion_robust/multi3_init.png" ></td>
<td><img src="{{ site.url }}/external/gplvm_occlusion_robust/multi3_final_const.png" ></td>
</tr>
</tbody>
</table>

## Poster

<a href="{{site.url}}/external/gplvm_occlusion_robust/occlusion_robust_pose_model_estimation_using_GPLVM_poster.pdf">poster</a>

## Slides

<a href="{{site.url}}/external/gplvm_occlusion_robust/occlusion_robust_pose_model_estimation_using_GPLVM.pdf">slides</a>

## Videos

<iframe width="420" height="315" src="https://www.youtube.com/embed/B2fMmRO_l1Q" frameborder="0" allowfullscreen></iframe>

{% comment %}

## Abstract

Pose and model estimation in the past literature did not include object context. In this project, we propose a multi-object pose and model estimation algorighm that is robust to occlusion. We use 3D voxelized models to and foreground background segmentation to detect 3D pose and model variation. In the relevant literature, [4], the authors used gradient descent to minimize the optimization energy function which measures the foreground mask and the projection of the estimated model and jointly optimize the pose and the model. the 6 degree location and rotations and deformation of model. The system is gen- erally stable given prior mask and pose and works faster than state-of-the-art 3D pose estimation algorithm such as Zia and Stark [7]. However, the system requires prior mask which is hard to get automatically. In this project, we pro- pose automatic segmentation mechanism that make use of object relation and alternating multi-object newton step. Thus, we improve the system by making it robust to tex- ture and background color and especially occlusion from other object. The pipeline is first, we use [2] object detec- tor to find location of objects and use these bounding boxes for contour detection techniques such as hierarchical image segmentation [8] or Grab Cut to capture the contour of ob- jects which is essential for correct pose estimation. Then we run newton step for pose and model estimation for multiple objects.
{% endcomment %}
