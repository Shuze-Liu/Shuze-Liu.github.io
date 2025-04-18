---
layout: post
title:  "Attention: in search of efficient and performant seq2seq encoder"
categories: jekyll update
published: false
---


> Date: 12.04
> 
> Reference: https://lilianweng.github.io/posts/2018-06-24-attention/

#### Attention

##### Limitation of RNN

Sequence-based tasks like translation are based on classification per token. We know DL is about information retrieval, we need enough context information to do classification, hence require mechanism to encode all context of current token. 

Before attention, the mainstream model is RNN. To encode all context, RNN will fuse together current step input and latent token that capture useful information on previous steps with the formula below:

$$
h_{t+1}=ReLU(W[x;h_t]+b)
$$


However, it is neither performant nor efficient. Information through RNN is essentially filtered per step, and due to gradient problems caused by non-linearity of sigmoid, RNN doesn't perform well in long sequences. Also, computing RNN requires multiplying the same weight matrix over and over again, hence concurrency is impossible. Attention beats RNN in both of these perspectives.


##### Attention: bring in all steps

Intuition of Attention is, when it's hard to encode information through RNN, how about bring all latent steps together, and use attention score to select relevant part? Based on this idea, current context is encoded with an weighted average of per-step embedding of all steps
$$
c_{t+1}=\sum^T_i a_i h_i
$$
Where $a_i$ is SoftMax-normalized attention score, measuring relevance of that step and current step.

##### Attention score: measuring relevance

Major attention score approaches are dot product-based: $\text{score}(\boldsymbol{s}_t, \boldsymbol{h}_i) = \boldsymbol{s}_t^\top\boldsymbol{h}_i$

Now attention scores are usually scaled by $1/\sqrt{dim}$. Because when the input is large, the softmax function may have an extremely small gradient, hard for efficient learning.

To capture more latent connection, current embedding and each steps can be linear transformed before computing similarity, leading to transformer attention:
$$
\text{Attention}(\mathbf{Q}, \mathbf{K}, \mathbf{V}) = \text{softmax}(\frac{\mathbf{Q}\mathbf{K}^\top}{\sqrt{n}})\mathbf{V}
$$
**Note**: because V of transformer attention is linear transformed input, it's similar to an unbiased linear layer before attention. 

#### Transformer

- positional encoding

- encoder: attention, residual connection, layernorm
  - multi-head attention (concat of multiple attention + linear mapping back to dim)

- decoder
  - masked attention to avoid observing steps later
  - cross-attention: Query from outputs and KV from inputs


$$
\begin{aligned}
\text{MultiHead}(\mathbf{Q}, \mathbf{K}, \mathbf{V}) &= [\text{head}_1; \dots; \text{head}_h]\mathbf{W}^O \\
\text{where head}_i &= \text{Attention}(\mathbf{Q}\mathbf{W}^Q_i, \mathbf{K}\mathbf{W}^K_i, \mathbf{V}\mathbf{W}^V_i)
\end{aligned}
$$


Computing process

- encode process: whole sequence have been encoded into per-sequence embedding
- decode process: auto-regressive generation

 

![Txn](https://lilianweng.github.io/posts/2018-06-24-attention/transformer.png)

