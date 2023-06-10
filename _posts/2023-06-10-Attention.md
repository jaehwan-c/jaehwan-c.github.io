---
title:  "[NLP] Attention is all you need"
excerpt: "Part. 1, Attention Mechanism"
layout: single
author_profile: true

categories:
  - NLP
tags:
  - [NLP, Attention]
 
date: 2023-06-10
last_modified_at: 2023-06-10

use_math: true

sidebar:
  nav: "docs"
---

Although Sequence-to-Sequence models were successful in different tasks just as mentioned [here](https://jaehwan-c.github.io/nlp/seq2seq/Seq2Seq/), there were some problems, including:

1. The information stored as a context vector is with certain size,and there may be loss of information,
2. Vanishing gradient may occur for using RNN.
3. Parallelization is not able.

This resulted in bad performance in tasks with longer sentences. Although there were attempts for improving the efficiency using factorizaion trick and conditional computation, the sequential computation has remained as a problem.

Therefore, the suggestion from scholars from Google was to apply the concept of "attention mechanism." The model is known as Transformer. 

This structure only utilizes attention mechanism. There were several advantages for this model:

1. Translation,
2. Parallelization while training,
3. Constituency Parsing, in defining structure of words in a sentence.

<h2>Basic Mechanism</h2>

The concept that is applied here is the "<b>attention mechanism</b>". This mechanism involves encoder-decoder system just like seq2seq mechanism. Simply saying, this mechanism refer back to the input sentence given from the encoder at every moment of prediction at the decoder. The referance is done with priority to the words that are relateed more.

The atttention value is calculated with inputs of <b>Q</b>uery, <b>K</b>eys, and <b>V</b>alues.

![Attention Value](https://wikidocs.net/images/page/22893/%EC%BF%BC%EB%A6%AC.PNG "Attention Value"){: .align-center}

<b>Q</b>uery refers to hidden state at decoder cell state $t$. <b>K</b>eys are multiplied using dot-product with Query and are from encoder, and <b>V</b>alues are multiplied with the weight and also are from encoder.

<h3>Dot-Product Mechanism</h3>

To undestand the true mechanism, a sample method is to be explained: dot-product mechanism.

![Process of Attention Mechanism](https://wikidocs.net/images/page/22893/dotproductattention1_final.PNG "Attention Mechaism"){: .align-center}

Imagine the first and second cells of decoder are predicted using attention algorithm. The third cell of decoder regers back to every  input from the encoder for the prediction. The <span style="color: red">red</span> rectangles represent the softmax value of encoder values. The higher the value, the more important for the prediction.

1. Scoring Attention Score

![Scoring Attention Score](https://wikidocs.net/images/page/22893/dotproductattention2_final.PNG "Attention Score"){: .align-center}

Some of the preconditions include:

- $h_n$ is hidden state at time step $n$,
- $S_t$ is hidden state for the decoder at time $t$,
- The assumption is made that the dimension is the same.

To get the attention value, $S_t$ is transposed and is multiplied using dot-product to each hidden state of encoder. This results in the sets of attention scores:

$$e^t = [S_th_1, S_th_2, ... S_th_n]$$

Since this is dot product, the mechanism is known as dot-prduct attention.

2. Attention Distribution using Softmax Function

![Softmax Function](https://wikidocs.net/images/page/22893/dotproductattention3_final.PNG "Softmax Function"){: .align-center}

Applying softmax function in $e^t$ provides a probability distribution with summation of 1. The result is called attention distribution, with each value known as attention weight.

3. Getting Attention Value

![Getting Attention Value](https://wikidocs.net/images/page/22893/dotproductattention4_final.PNG){: .align-center}

It is necessary to get the weighted sum, $a_t = \sum_{t=1}^N a_i^th_i$. The attenention value is knwon as context vector for it containing the context of the encoder.

4. Concenate Attention Value and Decoder Hidden State at Time $t$

![Concentenate](https://wikidocs.net/images/page/22893/dotproductattention5_final_final.PNG "Concetanate")

After getting attention score $a_t$, the $a_t$ is concetanated with $s_t$. This new vector is $v_t$, and this is used as an input for $\hat{y}$ for better prediction.

5. $\widetilde{S}_t$ for Input of Output Layer Calculation

The new vector $\widetilde{S}_t$ is calculated as:

$\widetilde{S}_t = tanh(W_c[a_t; s_t] + b_c)$

$W_c$ is trainable weight matrix, and $b_c$ is bias. For the input of further cells, Seq2Seq mechanism used hidden state $s_t$, but attention mechanism uses $\widetilde{S}_t$.

6. $\widetilde{S}_t$ for Prediction Vector

$$\hat{y}_t = Softmax(W_y\widetilde{S}_t + b_y)$$

<h2>Conclusion</h2>

This post demonstrates how attention mechanism works. The transformer model, which was described in the paper will be further examined in another post.