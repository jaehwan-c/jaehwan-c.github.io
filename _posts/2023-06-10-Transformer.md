---
title:  "[NLP] Attention is all you need"
excerpt: "Part. 2, Transformer"
layout: single
author_profile: true

categories:
  - NLP
tags:
  - [NLP, Attention, Transformer]
 
date: 2023-06-10
last_modified_at: 2023-06-10

use_math: true

sidebar:
  nav: "docs"
---

With attention mechanism described [here](https://jaehwan-c.github.io/nlp/attention/Attention/), this model uses attention, not other mechanisms like RNN. 

<h2>Hyperparameters</h2>

There are several hyperparameters that are to be used to explain the model.

$$d_{model} = 512$$

$d_{model}$ is the dimension of the embedding vector. For the values to be forwarded, the dimension stays the same.

$$num\,layers = 6$$

The number of layers for the encoder and decoder.

$$num\,heads = 8$$

This is the number of processes that are used in parallel.

$$d_{ff} = 2048$$

This is the size of hidden layers for the feed-forward neural network in the transformer.

<h2>Transformer</h2>

![Transformer](https://wikidocs.net/images/page/31379/transformer2.PNG "Transformer"){: .align-center}

This is the visual interpretation of the transformer model.

![Transformer 2](https://wikidocs.net/images/page/31379/transformer4_final_final_final.PNG "Transformer 2"){: .align-center}

This is the basic interpretation, with input and output shown. \<sos\> is indicating the start, and \<eos\> is indicating the end of the sentence for the prediction.

The overall process is shown in the following diagram.

![Overall Process](https://blog.promedius.ai/content/images/2021/03/t03-1.png "Overall Process"){: .align -center}

<h2>Positional Encoding</h2>

RNN was advantageous in natural language processing for having positional information. As Transformer does not receive the input of words sequentially, positional encoding is done on the input.

![Positional Embedding](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcbSURP%2Fbtrkr4KgZCc%2FPuUKLnd5aaO8eKaEP4MkqK%2Fimg.png "Positional Embedding"){: .align-center}

With the embedding vectors for each word, positional encoding is added. For providing positional values, there are two functions used:

$$PE_{(pos, 2i)} = sin(pos / 10000)^{2d/d_{model}}$$
$$PE_{(pos, 2i + 1)} = cos(pos / 10000)^{2i/d_{model}}$$

![Index](https://wikidocs.net/images/page/31379/transformer7.PNG "Index"){: .align-center}

This is the diagram for better understanding. When the index is even, sin function is used. If the index is odd, cosine function is used.

<h2>Attentions used</h2>

There are three types of attention mechanisms used.

1. Encoder Self-Attention

Self-attention is the processing of attention mechanism on oneself. This means that query, keys and values are from the same type of cell.

Encoder Self-Attention can be interpreted as using Q, K and V as the word vectors of the input.

For this step, Q, K, and V vectors are to be retrieved from the word vectors. The words have dimensions less than $d_{model}$.

The number of dimensions is decided by $num\, heads$. Here, the size is 64 for dividing $d_{model}$ with $num \, heads$.

![Q Word Vector](https://wikidocs.net/images/page/31379/transformer11.PNG "Q Word Vector"){: .align-center}

Each weight matrix has size of $d_{model} * (d_{model} / num \, heads)$. For each weight matrix, the word vector is multiplied to get Q, K, and V vectors.

After getting the vectors, attention score is calculated with scaled dot-product attention. The scaling is done with softmax function executed on each attention score calculated using $q * k / \sqrt{d_k}$.

![Scaled Attention](https://wikidocs.net/images/page/31379/transformer14_final.PNG "Scaled Attention"){: .align-center}

The overall process is seen through the image above.

<h2>Multi-head Attention</h2>

With the parallel processing, the efficiency increases with the model. 

![Parallel Processing](https://wikidocs.net/images/page/31379/transformer17.PNG "Parallel Processing"){: .align-center}

This is the visual interpretation. The values for weight matrices are different for each attention head.

This will be further explained with another post.

<h2>Padding Mask</h2>

Under scaled dot-product attention, masking is done. This is done to do not consider the words that are not present at the decoded time.

<h2>Position-wise Feed-Forward Neural Network</h2>

There is a neural network, with the number of layers $d_{ff}$. The equation is as follows:

$$FFNN(x) = MAX(0, xW_1, b_1)W_2 + b_2$$

$x$ is the result of multi-head attention (seq_len, $d_model$). $W_1$ has size of ($d_{model}, d_{ff}$), and $W_2$ has size of ($d_{ff}, d_{model}$). Within the encoder, the parameters remain the same. However, for other encoders, the values change.

<h2>Residual Connection & Layer Normalization</h2>

Both are applied for Multi-head Self-Attention and FFNN. Residual connection refers to adding the input and output of certain function to create a new function.

![Residual Connection for Multi-head Attention](https://wikidocs.net/images/page/31379/residual_connection.PNG "Residual Connection"){: .align-center}

This is residual connection for Multi-head Attention.

After such a process, layer normalization is done.

![Layer Normalization](https://wikidocs.net/images/page/31379/layer_norm_new_2_final.PNG "Layer Normalization"){: .align-center}

Each row is normalized for the size of $d_{model}$

After the transformation, the output is as follows:

$$ln_i = \gamma\hat{x}_i + \beta = LayerNorm(x)$$

where $\gamma$ is initialized with 1 and $\beta$ is initialized with 0.

<h2>Decoder</h2>

For decoders, the sentence matrix after embedding and positional encoding is provided as input.

The problem is that the input is with the whole sentence. This forces decoders to utilize look-ahead mask. This is done at the first sub-layer of decoders.

![Look-ahead mask](https://wikidocs.net/images/page/31379/%EB%A3%A9%EC%96%B4%ED%97%A4%EB%93%9C%EB%A7%88%EC%8A%A4%ED%81%AC.PNG "Look-ahead mask"){: .align-center}

The second sub-layer, multi-head attention, transfers padding mask., instead of look-ahead mask.

For encoder-decoder attention, this is not a self-attention described previously. Q matrix is from the decoder, but K and V are from the encoder.

![Encoder-Decoder](https://wikidocs.net/images/page/31379/%EB%94%94%EC%BD%94%EB%8D%94%EB%91%90%EB%B2%88%EC%A7%B8%EC%84%9C%EB%B8%8C%EC%B8%B5%EC%9D%98%EC%96%B4%ED%85%90%EC%85%98%EC%8A%A4%EC%BD%94%EC%96%B4%ED%96%89%EB%A0%AC_final.PNG "Encoder-Decoder"){: .align-center}

<h2>Conclusion</h2>

This model is still in use for different language models, and this is set as standard for such a process. Understanding and implementing transformer can be a great exercise for the further knowledge acknowledgement.