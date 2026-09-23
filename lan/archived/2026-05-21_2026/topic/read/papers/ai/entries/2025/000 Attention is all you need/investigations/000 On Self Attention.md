# 1 Journal

We can find an explanation in  [Understanding Self-Attention - A Step-by-Step Guide](https://armanasq.github.io/nlp/self-attention/) [^2](000%20On%20Self%20Attention.md#2).

The math there is similar to the one used in the [paper](https://arxiv.org/abs/1706.03762) [^1](000%20On%20Self%20Attention.md#1). As explained in the [guide](https://armanasq.github.io/nlp/self-attention/) [^2](000%20On%20Self%20Attention.md#2),

$$
\vec{Y} = \text{softmax}\lparen \frac{\vec{Q}\vec{K}^{T}}{\sqrt{d_k}} \rparen
$$

where

* $\text{softmax}$ is defined as in [3.2 On Softmax](000%20On%20Self%20Attention.md#32-on-softmax)
* $d_k$ is the dimension of the vectors

### 1.1.1 Pend
