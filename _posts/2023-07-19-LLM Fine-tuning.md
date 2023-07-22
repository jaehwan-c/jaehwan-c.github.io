---
title:  "[NLP] Parameter Efficient Fine-Tuning Methods"
excerpt: "Brief Introduction of Low-Rank Adaptation (LoRA) / Infused Adapter by Inhibiting and Amplifying Inner Activations (IA3)"
layout: single
author_profile: true

categories:
  - NLP
tags:
  - [NLP, Techniques]
 
date: 2023-07-19
last_modified_at: 2023-07-19

use_math: true

sidebar:
  nav: "docs"
---

With the emergence of Large Language Models, there were different attempts for fine-tuning for the correct answers in the given texts. Previously, the parameters changed with updated gradients along with different examples step-by-step. However, the in-context learning (ICL) method does not require and previous actions for updating gradients.

There are three types of ICLs that are used.

1. <b>Zero-shot learning</b>: the model predicts the answer with only the description of the task given,
2. <b>One-shot learning</b>: the model predicts the answer with the description and <b>one</b> example given,
3. <b>Few-shot learning</b>: the model predicts the answer with the description and <b>few</b> examples.

However, this method necessitates additional memory, calculation, and storage for input of the example. In addition, the model is expected still able to provide correct answers when wrong examples are provided in some circumstances. 

Parameter Efficient Fine-Tuning (PEFT) is one of the ways to deal with the disadvantages mentioned above. The idea is to train relatively small percentage of parameters (~ 0.01%) and handle the problems in a short period of time with high accuracy / precision. One of the most popular PEFT ways is Low-Rank Adaptation (LoRA).

<h2>Brief Introduction of PEFT</h2>

There were different methods for PEFT. One of the ways was to introduce adapters. Adapters are the structures of adding trainable feed-forward networks in between pre-trained models. The weights of pre-trained models are fixed. In addition to adapters, there are different methods that were introduced, including prompt engineering, prefix tuning, and stable diffusion.

<h2>Low-Rank Adaptation</h2>

LoRA is a PEFT technique that has been introduced by Microsoft, and is applied in different language models including LLaMa and Alpaca. Here, trainable rank decomposition matrices are added into the pre-trained model with fixed weights.

![LoRA](https://user-images.githubusercontent.com/7252598/230259439-fe58295d-9879-41c8-9454-0ecbe27cacde.png "LoRA"){: .align-center}

This diagram demonstrates how LoRA functions. For each layer, there is a parameter that can be added to hidden state $h$. Since the pre-trained model is based on matrix, the matrix is added to the existing pretrained weights directly after the rank decomposition matrices are calculated with GPU. 

<h2>Infused Adapter by Inhibiting and Amplifying Inner Activations</h2>

Infused Adapter by Inhibiting and Amplifying Inner Activations (IA3) is a method for tuning two vectors:

1. The vector that rescales Key / Value in Self-Attention and Cross-Attention,
2. The vector that rescales position-wise feed-forward network.

![IA3](https://pbs.twimg.com/media/FTC_Q1FWIAEzmN1.jpg "IA3"){: .align-center}

The diagram above demonstrates how this method is applied. IA3 is introduced on the left. The key parameters in transformers are manipulated by rescaling the key and values with vectors $l_k, l_v,$ and $l_{ff}$. This method is known for using less parameters and having less calculations than LoRA method. T-few model, which is demonstrated on the right, is the recipe that is based on T0 model with fine-tuning method of IA3. This model utilized two additional loss terms for model for lower probabilities for incorrect terms and consider the difference in lengths of answer choices.

<h2>Conclusion</h2>

With the emergence of Large Language Models, there have been attempts to fine-tune the model with less effort. These two techniques are some of the popular examples on how to fine-tune such large models.

The overall papers can be found here:
* [LoRA](https://arxiv.org/pdf/2106.09685.pdf)
* [IA3](https://arxiv.org/pdf/2205.05638.pdf)