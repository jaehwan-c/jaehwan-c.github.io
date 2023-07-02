---
title:  "[Computer Vision] Visual Transformer"
excerpt: "Manipulation of Transformer in Computer Vision"
layout: single
author_profile: true

categories:
  - Computer Vision
tags:
  - [Computer Vision, Transformer]
 
date: 2023-07-02
last_modified_at: 2023-07-02

use_math: true

sidebar:
  nav: "docs"
---

With the introduction of Transformers, the field of Natural Language Processing has improved and the algorithms for NLP have been based on transformer mechanism. This mechanism is also implemented in the field of computer vision.

<h2>Basic Introduction</h2>

With the ability of tranformer regarding efficiency and computationability, the training process with more than 100 bilion parameters has become available. In addition, the number of parameters keeps increasing.

There have been attempts to apply self-attention in CNN-like architectures or replace the convolutions entirely. For the replacement, due to limited availablility of hardware, the traditional attempts have been widely used for large images.

Just as how transformers are applied in NLP with large dataset, they are also efficient in larger dataset when compared with existing algorithms. This is inferred with lack of inductive bias compared to CNN. 

> Inductive Bias refers to a set of assumptions made by a learning algorithm in order to perform induction - to generalize a finite set of observation into a general model of the domain.

<h2>Architecture</h2>

![Basic Structure](https://github.com/jaehwan-c/jaehwan-c.github.io/assets/102342190/e612a80f-1a2a-4e83-9f52-b72ff4d09c7c "Basic Structure"){: .align-center}

This is the basic structure figure presented by the paper. There are some features that are likely to be mentioned and highlighted.

<h3>Transformer</h3>

The <b>standard</b> transformer receives a 1D sequence of token embeddings. For 2D images, the image $\text{X} \in \mathbb{R} ^{H \text{x} W \text{x} C}$ is transformed to flattened 2D patches of $\text{X}_p \in \mathbb{R}^{N \text{x} (P^2 * C)}$, where 

* $(H, W)$ is the resolution of the original image,
* $C$ is the number of channels,
* $(P, P)$ is the resolution of each image patch,
* and $N = HW / P^2$ is the resulting number of patches.

The transformer also uses $D$, the constant latent vector size, through all of its layer. 

Another noteworthy feature is token in front of embedding. Just as [class] token in BERT, the trainable embedding batch $z^0_0$ is added, whose state at the output of the Transformer encoder $z^0_L$ serves as the image representation $y$. The classification head is attached for pre-training and fine-tuning. In addition, Positional embedding is added for identifying the position of patch embedding. 

The repetition is done for $L$ times.

<h3>Inductive Bias</h3>

Vision Transformer has less image-specific inductive bias than CNNs. In CNNs, locality, 2D neighborhood structure, and translational equivalance are used for reference for each layer. However, in ViT, only MLP layers are local and translationally equivarent. In addition, the 2D neighborhood structure is used very sparingly.

<h3>Hybrid Architecture</h3>

For alternative of raw image patches, the input sequence can be formed from feature maps of a CNN. 

<h2>Results</h2>

The experiments were done for representation learning capabilities. The datasets include: ImageNet Dataset, ImageNet-21K, and JFT. For the transfer learning, ImageNet, CIFAR 10/100 and Oxford-IIIT Pets are used. The models of different sizes are used with different layers, hidden size $D$, MLP Size, Heads, and overall, the parameters.

![Results](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbB8Zzy%2Fbtq5M4kizYF%2Fc0B1N2SxukFTf2ikAYm1C1%2Fimg.png "Results"){: .align-center}

For the results, ViT-H/14 has outperformed the other models.

![Data Size](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqMKrY%2Fbtq5TsiXtVQ%2FopkJsUvfEFnwvqqIKXohwk%2Fimg.png "Data Size"){: .align-center}

This graph demonstrates how the size of dataset may be related to the performance. As mentioned in the introduction, the larger the dataset, the higher the accuracy becomes.

![Scaling Study](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbBRTAj%2Fbtq5Q7GTtyi%2FKiG1wfPpAoTHeEBDSP5MrK%2Fimg.png "Scaling Study"){: .align-center}

This is the graph for performance versus cost. There are three things that are revealed:

1. ViT requires computing for performing approximately similar to ResNet,

2. Hybrid model outperforms ViT at lower cost, but cannot at higher cost,

3. ViT does not saturate and is able to be scaled.

<h2>Conclusion</h2>

This paper shows how transformer architecture can also be applied in computer vision.

There are still several challenges, including
- Applying in other tasks including detection / segmentation,
- For more efficient pre-training
- Further scaling.

The overall paper can be found [here](https://arxiv.org/pdf/2010.11929.pdf).