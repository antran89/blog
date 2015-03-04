---
layout: article
title: "Enrich Object Detection (CVPR15)"
date: 2015-03-03T22:54:05-0800
modified:
categories: projects
excerpt:
comments: true
tags: [detection, 2D-3D registration]
ads: true
image:
  feature: research/front.png
  teaser: research/front.png
---

Christopher B. Choy, Michael Stark, Sam Corbett-Davies, Silvio Savarese, **Enrich Object Detection with 2D-3D Registration and Continuous Viewpoint Estimation**, CVPR 2015.

## Abstract

A large body of recent work on object detection has focused on exploiting 3D CAD model databases to improve detection performance. Many of these approaches work by aligning exact 3D models to images using templates generated from renderings of the 3D models at a finite set of discrete viewpoints. The training procedures for these approaches, however, are very expensive and require gigabytes of memory and storage, and the viewpoint discretization hampers pose estimation performance. 
We propose an efficient method for synthesizing templates from 3D models that runs on the fly -- that is, it quickly produces detectors for an arbitrary viewpoint of a 3D model without expensive dataset-dependent training or template storage. Given a 3D model and an arbitrary continuous detection viewpoint, our method synthesizes a discriminative template by extracting features from a rendered view of the object and decorrelating spatial dependences among the features. Our decorrelation procedure relies on a gradient-based algorithm that is more numerically stable than standard decomposition-based procedures, and we efficiently search for candidate detections by computing FFT-based template convolutions. Due to the speed of our template synthesis procedure, we are able to perform joint optimization on scale, translation, continuous rotation, and focal length. We provide an efficient GPU implementation of our algorithm, and we validate its performance on 3D Object Classes and PASCAL3D+ datasets.

## Results


## Codes


