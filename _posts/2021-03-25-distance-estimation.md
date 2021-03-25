---
layout: post
title: "Learning Object-Specific Distance From a Monocular Image"
description: "Description."
author: "Nathan Tsai"
toc: false
comments: false
# image: 
hide: false
search_exclude: true
show_tags: false
# sticky_rank: 1
categories: [self-driving, distance-estimation]
permalink: /distance-estimation
---

# Learning Object-Specific Distance From a Monocular Image

## Abstract

TODO

## Introduction

In computer vision research for self-driving cars, researchers have not recognized the importance of environment perception in favor of more popular tasks, like object classification, detection, and segmentation. Object-specific distance estimation is important for car safety and collision avoidance, and the lack of deep learning applications is likely due to the *lack of datasets with distance measures for each object in the scene*.

Current self-driving systems predict object distance using the traditional **inverse perspective mapping (IPM) algorithm**. This method first locates a point on the object (usually on the lower edge of the bounding box), then projects the located point onto a bird's-eye view coordinate map using camera parameters, and finally estimates the object distance using the constructed bird's-eye view coordinate map. This simple method can provide reasonable distance estimates for objects close and in front of the camera, but it *performs poorly when objects are located to the sides of the camera or curved roads and when objects are over 40 meters away*.

The authors first sought "to develop an end-to-end learning based approach that directly predicts distances for given objects in the RGB images." *End-to-end* meaning that object detection and distance estimation parameters are trained jointly. The base model extracts features from RGB images, then utilizes region of interest (ROI) pooling to generate a fixed-size feature vector for each object, and feeds the ROI feature vectors into a distance regressor to predict a distance for each object. However, this method was not sufficiently precise for self-driving.

The authors then created an enhanced model with a keypoint regressor to predict *(Z)* of the 3D keypoint coordinates *(X,Y,Z)*. Leveraging the camera projection matrix, the authors defined a projection loss between the projected 3D point and the ground truth keypoint. The keypoint regressor and projection loss are used for training only. During inference, the trained model takes in an image with bounding boxes and outputs the object-specific distance, without any camera parameters invervention.

The authors construct an extended dataset from the `KITTI object detection dataset` and the `nuScenes mini dataset` by "computing the distance for each object using its corresponding LiDAR point cloud and camera parameters."

Enhanced model performs better at distance estimation, compared to the traditional IPM algorithm and the support vector regressor, is also more precise than the base model, and is twice as fast during inference than IPM algorithm.

### Summary

* Base model: use deep learning to predict distance from given objects on RGB image without camera parameter intervention
* Enhanced model: base model with keypoint regressor and new projection loss
* Dataset: extended `KITTI` and `nuScenes mini` object-specific distance datasets

## Related Work

### Distance Estimation

* **Inverse Perspective Mapping (IPM)**: convert a point or a bounding box in the image to the corresponding bird's-eye view coordinate
* **Support Vector Machine Regressor**: predict object-specific distance given width and height of a bounding box
* **DistNet**: use YOLO for bounding boxes prediction instead of image features learning for distance estimation, the distance regressor studied geometric mapping from bounding box with certain width and height to distance value
    * In contrast, this paper directly predicts distances from learned image features.
* **Marker-based Methods**: use auxiliary information
* **Calibration Patterns**: predict physical distance based on rectangular pattern, where 4 image points are needed to compute camera calibration

### 2D Visual Perception

* **R-CNNs**: boost accuracy, decrease processing time for object detection, classfiication, dsegmentation
* **SSD and YOLO**: end-to-end frameworks to detect and classify objects in RGB images
* **Monocular Depth Estimation**: predict dense depth maps for given monocular color images

## Methods

The authors propose a *base model* that predicts physical, object-specific distance from given RGB images and object bounding boxes and an *enhanced model* with keypoint regressor for improved distance estimation.

### Base Method

![Base Model Diagram](/images/distance-estimation/base-model.png)

#### Feature Extractor

* Extract feature map for entire RGB image using image feature learning network.
* Use existing architecture (e.g. `vgg16`, `resnet50`) as feature extractors.
* Output of last layer is max-pooled and extracted as feature map for input RGB image.

#### Distance Regressor and Classifier

* Feed feature map and object bounding boxes into ROI pooling layer to generate fixed-size feature vector $$\boldsymbol{F_i}$$ for each object in the image.
* Pass pooled object feature vector into distance regressor for predicted distance $$D(\boldsymbol{F_i})$$ and object classifier for predicted category label $$C(\boldsymbol{F_i})$$.
* **Distance Regressor**: 3 fully-connected (FC) layers, `{2048, 512, 1} for vgg16` and `{1024, 512, 1} for resnet50`.
    * Softplus activation layer applied on output of last FC layer to ensure predicted distance $$D(\boldsymbol{F_i})$$ is positive.
* **Distance Classifier**: A FC layer with *(number of categories in dataset)* neurons, then softmax function to get predicted category label $$C(\boldsymbol{F_i})$$.
* **Loss Functions**
    * *Regressor Loss*:

        $$L_{dist}=\frac{1}{N}\sum_{i=1}^{N} \text{smooth}_{L1}(d^{*}_{i}-D(\boldsymbol{F_i}))$$

    * *Classifier Loss*:

        $$L_{cla}=\frac{1}{N}\sum_{i=1}^{N} \text{cross-entropy}_{L1}(y^{*}_{i},C(\boldsymbol{F_i}))$$

    * $$N$$: number of objects in the image
    * $$d^{*}_{i}$$: ground truth distance of the $$i$$-th object
    * $$y^{*}_{i}$$: ground truth category label of the $$i$$-th object

#### Model Learning and Inference

* Train feature extractor, distance regressor, and object classifier simultaneously using:

    $$\text{min}L_{base}=L_{cla}+\lambda_{1}L_{dist}$$

    * Set $$\lambda_{1}=1.0$$ during training.
* Use `ADAM` optimizer with `beta` value $$\beta=0.5$$, `learning rate` of 0.001, exponentially decayed after 10 `epochs`.
* Classifier encourages model to learn features used in estimating more accurate distances, only used during training.
* After training, base model can predict object-specific distances given RGB images and object bounding boxes as input.

### Enhanced Method

![Enhanced Model Diagram](/images/distance-estimation/enhanced-model.png)

Add keypoint regressor to optimize base model by introducing projection constraint for better distance prediction.

#### Feature Extractor

* Same architecture as base model: `vgg16` or `resnet50` into ROI pooling layer for object specific features $$\boldsymbol{F_i}$$.

#### Keypoint Regressor

* Keypoint regressor $$K$$ learns to predict approximate keypoint position in 3D camera coordinate system.
* Distance regressor predicts *Z* coordinate, while keypoint regressor predicts *(X, Y)*.
* **Keypoint Regressor**: 3 fully-connected (FC) layers, `{2048, 512, 1} for vgg16` and `{1024, 512, 1} for resnet50`.
* Since there is no ground truth of 3D keypoint available, project the generated 3D point $$(X,Y,Z)=([K(\boldsymbol{F_i}), D(\boldsymbol{F_i})])$$ back to image plane using camera projection matrix $$P$$.
* Compute errors between ground truth 2D keypoint $$k^{*}_{i}$$ and projected point $$P\cdot([K(\boldsymbol{F_i}), D(\boldsymbol{F_i})])$$.
* *Keypoint Function*

    $$L_{3Dpoint}=\frac{1}{N}\sum_{i=1}^{N} \frac{1}{d^{*}_{i}} \| P\cdot([K(\boldsymbol{F_i}), D(\boldsymbol{F_i})]) - k^{*}_{i} \|_{2}$$

    * Use weight with regard to ground truth distance to encourage better predictions for closer objects.

#### Distance Regressor and Classifier

* Same architecture as base model and training losses $$L_{dist}$$ and $$L_{cla}$$.
* Distance regressor parameters also optimized by projection loss $$L_{3Dpoint}$$.

#### Network Learning and Inference

* Train feature extractor, keypoint regressor, distance regressor, and object classifier simultaneously using:

    $$\text{min}L_{base}=L_{cla}+\lambda_{1}L_{dist}+\lambda_{2}L_{3Dpoint}$$

    * Distance loss weight constant: $$lambda_{1}=10.0$$
    * Keypoint loss weight constant: $$lambda_{2}=0.05$$
* Use same `optimizer`, `beta`, and `learning rate` as the base model.
* *Training Only*: use camera projection matrix $$P$$, keypoint regressor, and object classifier.
* *Testing*: Given RGB image and bounding boxes, directly predicts object-specific distances without any camera parameter intervention.

## Dataset Construction

`KITTI` and `nuScenes mini` both provide RGB images, 2D/3D bounding boxes, category labels for objects in the images, and the corresponding velodyne point cloud for each image.

### Object Distance Ground Truth Generation

1. Use 3D bounding boxes to segment velodyne point cloud for each object.
1. Sort the segmented points based on depth values.
1. Extract the *n*-th depth value as **object-specific ground truth distance**, where $$n=0.1*(number of segmented points)$$ to avoid extracing depth values from noise points.
1. Project velodyne points to corresponding RGB image planes and get their image coordinates as **keypoint ground truth distance**.
1. Append both ground truths to the object detection dataset labels.

### KITTI

* Split `KITTI` into training (`3,712 RGB images, 23,841 objects`) and validation sets (`3,768 RGB images, 25,052 objects`) using 1:1 split ratio.
* All `KITTI` objects are categorized into 9 classes, i.e. *Car, Cyclist, Pedestrian, Misc, Person_sitting, Tram, Truck, Van, DontCare*.
* Generated `KITTI` ground truth distances should be between [0, 80] meters.

### nuScenes mini

* Split `nuScenes mini` into training (`200 images, 1,549 objects`) and validation sets (`199 images, 1,457 objects`).
* All `nuScenes mini` objects are categorized into 8 classes, i.e. *Car, Bicycle, Pedestrian, Motorcycle, Bus, Trailer, Truck, Construction_vehicle*.
* Generated `nuScenes mini` ground truth distances should be between [2, 105] meters.

## Evaluation

### Evaluation Metrics

* 

### Compared Approaches

* 

### Results on KITTI

* 

### Results on nuScenes

* 

## References
[^1]: Footnote