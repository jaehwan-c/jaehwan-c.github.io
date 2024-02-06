---
title:  "[Computer Vision] Faster R-CNN"
excerpt: "Brief Introduction of Faster R-CNN"
layout: single
author_profile: true

categories:
  - Computer Vision
tags:
  - [Computer Vision, RCNN]
 
date: 2024-02-06
last_modified_at: 2024-02-06

use_math: true

sidebar:
  nav: "docs"
---

R-CNN is a model that is used for object detection, and faster R-CNN, as its name demonstrates, is an improved model based on R-CNN model. 

<h3>R-CNN</h3>

![R-CNN](https://github.com/jaehwan-c/jaehwan-c.github.io/assets/102342190/12cf0ee2-93a6-4ed1-aad1-68a384aab0fb)

This diagram above demonstrates the underlying algorithm of R-CNN model. R-CNN initially suggests an "area" that the object is most likely to be apparent using selective search which is done between processes 1 and 2 in the image. There are around 2,000 boxed areas suggested for each image. Later, the areas are "cropped" to form warped region of the same size, for further classification and regression processes. Then, the features are extracted under same CNN.

The problem for this method is that for each image, CNN calculations are done for 2k number of areas. This requires excessive amount of time and effort for computation. 

<h3>Fast R-CNN</h3>

To handle the time-consuming issue from R-CNN model, Fast R-CNN extracts feature of the suggested region from the feature map, not from the original image.

Fast R-CNN applies RoI pooling. RoI pooling is the process of computing max-pooling within the area of interest (area for CNN to be computed). Max pooling results in feature map of size h by w for the processing to fully connected layer. Regardless of the size of the vectors before the pooling, this process forces the outcome to be of equal size.

<h3>Faster R-CNN</h3>

Faster R-CNN model aims to to the following processes at once: CNN traits extraction, classification, and bounding box regression. However, selective search is not applied; instead, regional proposal network is applied to retrieve RoI.

![Faster R-CNN](https://github.com/jaehwan-c/jaehwan-c.github.io/assets/102342190/cd3397ba-f3a7-4e6d-af8e-e7a49ce00b13)

Region Proposal Network takes an image of any size as input and outputs a set of rectangular object proposals with objectness score. The input of RPN is the feature map that has been passed through CNN.

![Region Proposal Network](https://github.com/jaehwan-c/jaehwan-c.github.io/assets/102342190/b71f2d7b-7173-4729-925d-535d067e413f)

This diagram demonstrates how the feature map extracts region from RPN. k Anchor Boxes of different sizes are defined with each cell using slinding window methodwith 3 by 3 kernel size. Then, using the encoder, convert feature map of (channel, 7, 7) to (256, 7, 7) by applying padding and convolutional layer. For the classification of the object in the image, there are 2k channels; while there are 4k coordinates (x, y, w, h) for regression for expecting the location of bounding box.

There are four steps for the model to be trained.

1. ImageNet pretrained model is retrieved. Region proposal network is trained end-to-end.
2. Train Fast R-CNN with region proposal layer which excludes CNN from the trained RPN.
3. Fine-tune 

The overall paper can be found [here](https://welcome-to-dewy-world.tistory.com/110).