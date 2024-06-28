---
layout: distill
title: ‘Yet another set of notes on self-attention’
date: 2024-06-27 19-12-28 +0100
category: 
tags:
---



This is a living document that I’ll extend with more information as I go. As attention is dead-center when it comes to LLMs, it’s not weird that a lot has been written about it. Here I wanted to collect some of my own questions with their answers (for easy reference). 

Parts of these notes are based on the new book "Deep Learning" by Bishop & Bishop[^3], which I recommend. They also have some comments from me (and, of course, all mistakes / typos are my own).

```toc
```
# Dot-product self-attention 

Let’s get started. We have a matrix $$X\in\mathbb{R}^{n\times m}$$ which represents a sequence of $$n$$ $$m$$-dimensional vectors. For the language modeling example, each one of those could be the embedding of a single token. 

Let’s suppose that we are in the business of modeling, so we would like to map $$X$$ to a $$Y\in \mathbb{R}^{n\times m}$$ such that each m-dimensional vector in $$Y$$ contains information from all $$X_j$$ ($$X_j$$ being the $$j$$-th column vector). Perhaps the simplest way is $$ Y_i=\sum_jA_{ij}X_j, $$ where we assume that $$A_{ij}\in [0,1]$$ for all $$i,j$$. Restricting $$A_{ij}$$ such that $$\sum_{j}A_{ij}=1$$ for all $$i$$ has some nice properties, we can now think of $$Y_i$$ as a weighted mean of the $$X_j$$, and we just get to decide how much of each $$X_j$$ to use.

Following this recipe further, we can pick $$A_{ij}$$ according to how relevant each $$X_j$$​ is to every other. One way to capture relevance is through similarity, leading to “*dot-product self-attention*”[^2]:



$$A_{ij}=\text{softmax}(XX^T)_{ij}=\frac{\exp(X_i^TX_j)}{\sum_{k}\exp(X_i^TX_k)}.$$​



As $$X\in\mathbb{R}^{n\times m}$$, $$XX^T$$ has dimensions $$n^2$$​, i.e., it is quadratic on sequence size. 

To get $$Y$$, we can just do $$Y=\text{softmax}(XX^T)X.$$​

This is fine, but: 

1. there’s nothing learnable here (how do we know that the raw $$X$$ is in the right representation to get the best possible $$Y$$ for our task?) and
2. every dimension of an $$X_i$$ gets the same weight. 

We can address these points by introducing a new matrix, $$U\in\mathbb{R}^{D\times D}$$, with learnable parameters such that $$\tilde{X} = XU$$, and so



 $$ Y=\text{softmax}(\tilde{X}\tilde{X}^T)\tilde{X}=\text{softmax}(XUU^TX^T)XU. $$ 



Progress, but now $$\tilde{X}\tilde{X}^T$$ is always a symmetric matrix regardless of $$U$$, so we cannot capture asymmetric relationships. This motivates using different parameters for the parts of the attention matrix and the final mapping: 

$$ \begin{align} Q&=XW_Q,\ W_Q\in\mathbb{R}^{m\times D_K},\\ K&=XW_k,\ W_K\in\mathbb{R}^{m\times D_K},\\ V&=XW_V,\ W_V\in\mathbb{R}^{m\times D_V}. \end{align} $$

Those are the celebrated query, key, and value matrices[^1], respectively, all learnable. Typically, $$D=D_K=D_V$$ makes it easier to work things out. With those matrices, we adjust attention as $$ Y=\text{softmax}(QK^T)V. $$​​ Quick dimensionality check:

- The $$\text{softmax}(QK^T)$$ part is a matrix with dimensions $$n\times n$$, where $$n$$: number of elements in sequence. 
- The matrix $$V$$ has dimensions $$n\times D_V$$. 
- So the matrix $$Y$$ has dimensions $$n\times D_V$$. 

## Scaling self-attention 

While $$Y=\text{softmax}(QK^T)V$$ is very close to the usual self-attention, we are missing a scaling constant. Let’s derive this here. 

Suppose that you have two $$D_K$$-dimensional vectors $$q,k$$, each one with elements that have zero mean and unit variance and are independent. Then: $$ \text{Var}[(q,k)]=\sum_{i=1}^{D_K} \text{Var}[q_ik_i]=\sum_{i=1}^{D_K}1=D_K. $$We used independence to split up $$\text{Var}[(q,k)]$$. Therefore, the standard deviation of $$(q,k)$$ is $$\sqrt{D_K}$$. This is what we need to make sure that the parts of $$QK^{T}$$ have unit variance. This helps us control how big the products are, which makes learning easier. So, finally $$ Y=\text{softmax}(QK^T/\sqrt{D_K})V, $$ which is the usual form of dot-product attention.

### Does this make sense?

Zero mean and unit variance are a matter of pre-processing, but independence is not. In fact, you would hope a sequence would not have independent elements, as otherwise there is no information to use to predict the next element. I now see this scaling as a way to control the size of the terms and help with learning, but it’s important to remember this point as it not always explicitly stated[^4]. 

# Computational costs of attention

We need to calculate the matrix product $$QK^T$$ which has computational cost $$O(nD^2)$$ if we assume $$D=D_V=D_K$$ and a sequence of length $$n$$. Then, the matrix product $$QK^{T}V$$ has cost $$O(n^2 D)$$ (I’m ignoring the application of the softmax here and the scaling). 

After this part, when dealing with a transformer block, we have an MLP layer that takes as input each output from the attention layer ($$n$$ of them in total). This layer has cost $$O(n^2D)$$. Therefore, the total cost is $$\max\{O(nD^2), O(n^2D)\}$$.

 $$D$$ is fixed at the time the transformer is designed, whereas $$n$$​ is the length of the input sequence, so you can see which of the two is going to be a challenge during inference with large inputs. 





##### Footnotes


[^1]: Query / Key / Value is a retrieval reference; [see for example on cross-validated.](https://stats.stackexchange.com/questions/421935/what-exactly-are-keys-queries-and-values-in-attention-mechanisms)
[^2]: There are so many variants of attention now: [grouped attention](https://arxiv.org/pdf/2305.13245), [linearised attention](https://arxiv.org/abs/2006.16236), etc. 
[^3]: Bishop, C.M. and Bishop, H., 2023. _Deep learning: Foundations and concepts_. Springer Nature.
[^4]: Though, the authors of [“Attention is all you need”](https://arxiv.org/pdf/1706.03762) do mention this assumption in the celebrated “footnote 4”.