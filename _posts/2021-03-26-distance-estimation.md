---
layout: post
title: "Learning Object-Specific Distance From a Monocular Image"
description: "Notes on distance estimation model by Zhu et al."
toc: true
comments: false
hide: false
search_exclude: false
show_tags: true
categories: [research-paper, machine-learning, computer-vision]
permalink: /distance-estimation
---

# Learning Object-Specific Distance From a Monocular Image
Research paper by Jing Zhu, Yi Fang, Husam Abu-Haimed, Kuo-Chin Lien, Dongdong Fu, and Junli Gu [^1].

## Abstract

TODO

## Introduction

In computer vision research for self-driving cars, researchers have not recognized the importance of environment perception in favor of more popular tasks, like object classification, detection, and segmentation. Object-specific distance estimation is important for car safety and collision avoidance, and the lack of deep learning applications is likely due to the *lack of datasets with distance measures for each object in the scene*.

Current self-driving systems predict object distance using the traditional **inverse perspective mapping (IPM) algorithm**. This method first locates a point on the object (usually on the lower edge of the bounding box), then projects the located point onto a bird's-eye view coordinate map using camera parameters, and finally estimates the object distance using the constructed bird's-eye view coordinate map. This simple method can provide reasonable distance estimates for objects close and in front of the camera, but it *performs poorly when objects are located to the sides of the camera or curved roads and when objects are over 40 meters away*.

The authors first sought "to develop an end-to-end learning based approach that directly predicts distances for given objects in the RGB images." *End-to-end* meaning that object detection and distance estimation parameters are trained jointly. The base model extracts features from RGB images, then utilizes region of interest (ROI) pooling to generate a fixed-size feature vector for each object, and feeds the ROI feature vectors into a distance regressor to predict a distance *(Z)* for each object. However, this method was not sufficiently precise for self-driving.

The authors then created an enhanced model with a keypoint regressor to predict *(X,Y)* of the 3D keypoint coordinates *(X,Y,Z)*. Leveraging the camera projection matrix, the authors defined a projection loss between the projected 3D point *(X,Y,Z)* and the ground truth keypoint (X\*,Y\*,Z\*). The keypoint regressor and projection loss are used for training only. During inference, the trained model takes in an image with bounding boxes and outputs the object-specific distance, without any camera parameters invervention.

The authors constructed an extended dataset from the `KITTI object detection dataset` and the `nuScenes mini dataset` by "computing the distance for each object using its corresponding LiDAR point cloud and camera parameters."

Enhanced model performs better at distance estimation, compared to the traditional IPM algorithm and the support vector regressor, is also more precise than the base model, and is twice as fast during inference than IPM algorithm.

### Summary

* Base model: use deep learning to predict distance from given objects on RGB image without camera parameter intervention
* Enhanced model: base model with keypoint regressor and new projection loss
* Dataset: extended `KITTI` and `nuScenes mini` object-specific distance datasets

## Related Work

### Distance Estimation

* **Inverse Perspective Mapping (IPM)**: convert a point or a bounding box in the image to the corresponding bird's-eye view coordinate
* **Support Vector Regressor**: predict object-specific distance given width and height of a bounding box
* **DistNet**: use YOLO for bounding boxes prediction instead of image features learning for distance estimation, the distance regressor studied geometric mapping from bounding box with certain width and height to distance value
    * In contrast, this paper directly predicts distances from learned image features.
* **Marker-based Methods**: use auxiliary information, create segmented markers in the image and estimate distance using the marker area and camera parameters
* **Calibration Patterns**: predict physical distance based on rectangular pattern, where 4 image points are needed to compute camera calibration

### 2D Visual Perception

* **R-CNNs**: boost accuracy, decrease processing time for object detection, classfiication, dsegmentation
* **SSD and YOLO**: end-to-end frameworks to detect and classify objects in RGB images
* **Monocular Depth Estimation**: predict dense depth maps for given monocular color images

## Methods

The authors propose a *base model* that predicts physical, object-specific distance from given RGB images and object bounding boxes and an *enhanced model* with keypoint regressor for improved distance estimation.

### Base Method

![Base Model Diagram]({{ site.baseurl }}/images/distance-estimation/base-model.png "Base Model Diagram")

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

        $$L_{dist}=\frac{1}{N}\sum_{i=1}^{N} L_{\text{1;smooth}}(D(\boldsymbol{F_i}), d^{*}_{i})$$

        $$L_{\text{1;smooth}} = \begin{cases}0.5(x_{i} - y_{i})^2/\beta, & \text{if $|x_{i}-y_{i}|<\beta$} \\ |x_{i}-y_{i}|-0.5*\beta, & \text{otherwise}\end{cases}$$

    * *Classifier Loss*:

        $$L_{cla}=\frac{1}{N}\sum_{i=1}^{N} L_{\text{cross-entropy}}(C(\boldsymbol{F_i}), y^{*}_{i})$$

        $$L_{\text{cross-entropy}}=-\sum_{i=1}^{M} y^{*}_{i} * log(C(\boldsymbol{F_i}))$$

    * $$N$$: number of objects in the image
    * $$M$$: number of categories
    * $$\beta$$: smooth loss scaling factor, usually $$1.0$$
    * $$D(\boldsymbol{F_i})$$: predicted distance
    * $$C(\boldsymbol{F_i})$$: predicted category label
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

![Enhanced Model Diagram]({{ site.baseurl }}/images/distance-estimation/enhanced-model.png "Enhanced Model Diagram")

Add keypoint regressor to optimize base model by introducing projection constraint for better distance prediction.

#### Feature Extractor

* Same architecture as base model: `vgg16` or `resnet50` into ROI pooling layer for object specific features $$\boldsymbol{F_i}$$.

#### Keypoint Regressor

* Keypoint regressor $$K$$ learns to predict approximate keypoint position in 3D camera coordinate system.
* Distance regressor predicts *Z* coordinate, while keypoint regressor predicts *(X, Y)*.
* **Keypoint Regressor**: 3 fully-connected (FC) layers, `{2048, 512, 2} for vgg16` and `{1024, 512, 2} for resnet50`.
* Since there is no ground truth of 3D keypoint available, project the generated 3D point $$(X,Y,Z)=([K(\boldsymbol{F_i}), D(\boldsymbol{F_i})])$$ back to image plane using camera projection matrix $$P$$.
* Compute errors between ground truth 2D keypoint $$k^{*}_{i}$$ and projected point $$P\cdot([K(\boldsymbol{F_i}), D(\boldsymbol{F_i})])$$.
* *Keypoint Function*

    $$L_{3Dpoint}=\frac{1}{N}\sum_{i=1}^{N} \frac{1}{d^{*}_{i}} \| P\cdot([K(\boldsymbol{F_i}), D(\boldsymbol{F_i})]) - k^{*}_{i} \|_{2}$$

    * Use weight with regard to ground truth distance to encourage better predictions for closer objects.

#### Distance Regressor and Classifier

* Same architecture as base model and training losses $$L_{dist}$$ and $$L_{cla}$$.
* Distance regressor parameters also optimized by projection loss $$L_{3Dpoint}$$.

#### Model Learning and Inference

* Train feature extractor, keypoint regressor, distance regressor, and object classifier simultaneously using:

    $$\text{min}L_{base}=L_{cla}+\lambda_{1}L_{dist}+\lambda_{2}L_{3Dpoint}$$

    * Distance loss weight constant: $$lambda_{1}=10.0$$
    * Keypoint loss weight constant: $$lambda_{2}=0.05$$
* Use same `optimizer`, `beta`, and `learning rate` as the base model.
* *Training Only*: use camera projection matrix $$P$$, keypoint regressor, and object classifier.
* *Testing*: Given RGB image and bounding boxes, directly predicts object-specific distances without any camera parameter intervention.
* Both models trained for `20 epochs` with `batch size of 1` on the training subset, augmented with horizontally-flipped training images.
* After training, input RGB image with bounding boxes into trained model to get the output of the distance regressor as the estimated object-specific distance in the validation subset.

## Dataset Construction

`KITTI` and `nuScenes mini` both provide RGB images, 2D/3D bounding boxes, category labels for objects in the images, and the corresponding velodyne point cloud for each image.

### Object Distance Ground Truth Generation

1. Use 3D bounding boxes to segment velodyne point cloud for each object.
1. Sort the segmented points based on depth values.
1. Extract the *n*-th depth value as **object-specific ground truth distance**, where $$n=0.1*(\text{number of segmented points})$$ to avoid extracing depth values from noise points.
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

Use the same metrics as depth prediction. Let $$d^{*}_{i}$$ and $$d_{i}$$ denote the ground truth distance and the predicted distance, respectively.

* **Threshold**

    $$\%\text{ of } d_i \text{ s.t. max}(d_i/d^{*}_{i},d^{*}_{i}/d_i)=\delta<\text{threshold}$$

* **Absolute Relative Difference**

    $$\text{Abs Rel}=\frac{1}{N}\sum_{i=1}^{N}|d_{i}-d^{*}_{i}|/d^{*}_{i}$$

* **Squared Relative Difference**

    $$\text{Squa Rel}=\frac{1}{N}\sum_{i=1}^{N}\|d_{i}-d^{*}_{i}\|^{2}/d^{*}_{i}$$

* **RMSE (linear)**

    $$\text{RMSE}_{\text{linear}}=\sqrt{\frac{1}{N}\sum_{i=1}^{N}\|d_{i}-d^{*}_{i}\|^{2}}$$

* **RMSE (log)**

    $$\text{RMSE}_{\text{log}}=\sqrt{\frac{1}{N}\sum_{i=1}^{N}\|\log{d_{i}}-\log{d^{*}_{i}}\|^{2}}$$

Note: don't include distances predicted for *DontCare* objects when calculating errors.

### Compared Approaches

* **Inverse Perpective Mapping Algorithm (IPM)**: predicts object-specific distance by approximating a transformation matrix between a normal RGB image and its bird's-eye view image using camera parameters
    * Use IPM from MATLAB's computer vision toolkit to get transformation matrices for RGB images from the validation subset.
    * Project middle points of the lower edges of the object bounding boxes into bird's-eye view coordinates using the transformation matrices.
    * Take values along forward direction as estimated distances.

* **Support Vector Regressor (SVR)**: predicts object-specific distance given width and height of a bounding box
    * Compute width and height of each bounding box in the training subset.
    * Train SVR with the ground truth distance.
    * Input widths and heights of each bounding box in the validation set to get estimated object-specific distances.

### Results

* Both proposed models have much lower relative errors and higher accuracies, compared to IPM and SVR, on `KITTI`.
* Enhanced model performs the best, implying effectiveness of keypoint regressor and projection constraint, on both `KITTI` and `nuScenes mini`.

## References
[^1]: J. Zhu, Y. Fang, H. Abu-Haimed, K. Lien, D. Fu, and J. Gu. *Learning Object-Specific Distance from a Monocular Image.* [arXiv:1909.04182](https://arxiv.org/abs/1909.04182), 2019.

[^2]: A. Geiger, P. Lenz, and R. Urtasun. *Are we ready for Autonomous Driving? The KITTI Vision Benchmark Suite.* [CVPR, pgs. 3354–3361](http://www.cvlibs.net/publications/Geiger2012CVPR.pdf), 2012.

[^3]: H. Caesar, V. Bankiti, A. H. Lang, S. Vora, V. E. Liong, Q. Xu, A. Krishnan, Y. Pan, G. Baldan and O. Beijbom. *nuScenes: A multimodal dataset for autonomous driving.* [arXiv:1903.11027](https://arxiv.org/abs/1903.11027), 2019.