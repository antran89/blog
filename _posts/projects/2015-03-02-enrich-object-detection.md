---
layout: article
title: "Enrich Object Detection (CVPR15)"
date: 2015-03-03T22:54:05-0800
modified: 2015-03-17T22:40:48-0700
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

## Video

<iframe width="560" height="315" src="https://www.youtube.com/embed/YKtioOXY8yQ" frameborder="0" allowfullscreen></iframe>

## Results

The algorithm can work as a standalone object detector or as a post-processing stage to enrich existing detection bounding boxes.

### Enrich Object Detection

<figure class="half">
	<img src="https://raw.githubusercontent.com/chrischoy/EnrichObjectDetectionResults/master/car_val_cnn/PASCAL12_car_val_init_0_car_each_27_lim_250_lam_0.150_a_48_e_4_y_1_f_1_scale_1.00_sbin_6_level_20_skp_n_sm_dvpv_server_104_tmp_3_prop_cnn_tuning_nv_8_img_15_dwot_obj_2.jpg">
	<img src="https://raw.githubusercontent.com/chrischoy/EnrichObjectDetectionResults/master/aeroplane_val_cnn/PASCAL12_aeroplane_val_init_0_aeroplane_each_7_lim_250_lam_0.150_a_24_e_5_y_5_f_1_scale_1.00_sbin_6_level_20_skp_n_sm_dvpv_server_capri7_tmp_1_prop_cnn_tuning_nv_8_img_201_dwot_obj_1.jpg">
	<figcaption>PASCAL 2012 dataset viewpoint estimation results. The blue bounding box indicates the detection results from R-CNN and purple box indicates the results from our system with 3D CAD model prediction overlaid (viewpoint, closest CAD model, focal length). The system is robust to input bounding box localization.</figcaption>
</figure>

### Standalone Object Detector

<figure>
	<img src="https://raw.githubusercontent.com/chrischoy/EnrichObjectDetectionResults/master/car_pascal12_det/PASCAL_car_val_init_0_Car_each_27_lim_250_lam_0.150_a_24_e_3_y_1_f_1_scale_2.00_sbin_6_level_15_skp_n_server_101_tmp_2_img_85.jpg">
	<figcaption>PASCAL12 dataset car detection and viewpoint estimation result. From the top left, the original image, true positive overlay, true positive detections with bounding boxes, top five false positive detections with bounding boxes. The score on the top of the bounding box indicates the confidence of detection.</figcaption>
</figure>
<figure>
	<img src="{{ site.url }}/images/research/eod_car.png">
	<figcaption>3D Object dataset detection and viewpoint estimation results. From the left, the original image, true positive detection, top two false positive detections. The score on the top of the bounding box indicates the confidence of detection.</figcaption>
</figure>

For more results, please visit,

- PASCAL 2012, Aeroplane
    <https://github.com/chrischoy/EnrichObjectDetectionResults/tree/master/aeroplane_val_cnn>

- PASCAL 2012, Car
    <https://github.com/chrischoy/EnrichObjectDetectionResults/tree/master/car_val_cnn>

- Object 3D, Bicycle
    <https://github.com/chrischoy/EnrichObjectDetectionResults/tree/master/bicycle_3dobject>

- Object 3D, Car
    <https://github.com/chrischoy/EnrichObjectDetectionResults/tree/master/car_3dobject>

## Codes

If you feel really brave, you can try my un-refactored codes are available at
Github : <http://github.com/chrischoy/EnrichObjectDetection2>


The refactored codes will soon be available at
Github : <http://github.com/chrischoy/EnrichObjectDetection>

## Dependencies

- OSGRenderer: <http://github.com/chrischoy/OSGRenderer>
- CUDA-FFT-Convolution: <http://github.com/chrischoy/CUDA-FFT-Convolution>
