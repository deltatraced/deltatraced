---
status: pend
---

You can find the paper [here](https://arxiv.org/abs/1706.03762) [^1](000%20Attention%20is%20all%20you%20need.md#1).

# 1 Journal

## 1.1 Reading 000

### 1.1.1 Evaluation Objective

Skim through the paper and note foundational next concepts to learn about to understand it, as well as terms to learn more about.

### 1.1.2 Concepts

### 1.1.3 Annotations

### 1.1.4 Keywords

 > 
 > dominant sequence transduction models
 > [^quote-paper](000%20Attention%20is%20all%20you%20need.md#quote-paper)

^keyword-000

 > 
 > Achieving {N} BLEU on the WMT 2014 English-to-German translation task
 > [^quote-paper-paraphrase](000%20Attention%20is%20all%20you%20need.md#quote-paper-paraphrase)

^keyword-001

 > 
 > sequence modeling and transduction problems such as language modeling and machine translation
 > [^quote-paper](000%20Attention%20is%20all%20you%20need.md#quote-paper)

^keyword-002

# 4 References

1. [paper: Attention Is All You Need](https://arxiv.org/abs/1706.03762) ^1

````bibtex
@misc{vaswani2023attentionneed,
      title={Attention Is All You Need}, 
      author={Ashish Vaswani and Noam Shazeer and Niki Parmar and Jakob Uszkoreit and Llion Jones and Aidan N. Gomez and Lukasz Kaiser and Illia Polosukhin},
      year={2023},
      eprint={1706.03762},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/1706.03762}, 
}
````

^quote-paper
^quote-paper-paraphrase

This paper will be annotated below for note taking purposes. All quotations will be marked to this paper.

2. [Understanding Self-Attention - A Step-by-Step Guide](https://armanasq.github.io/nlp/self-attention/) ^2

````mermaid
graph TD

%% Settings
classDef note fill:#f9f9a6,stroke:#333,stroke-width:1px,color:#000,font-style:italic;

%% Nodes
A1[^1 paper Attention is all you need]
A2[^2 article Understanding Self-Attention]
A4[^4 gh bertviz]
A5[^5 paper A Multiscale Visualization of Attention in the Transformer Model]

%% Connections
A2 --> |references| A4
A2 --> |explains| A1
A4 --> |has_paper| A5
````

3. [wiki: Softmax_function](https://en.wikipedia.org/wiki/Softmax_function) -> [ai faq softmax](http://www.faqs.org/faqs/ai-faq/neural-nets/part2/section-12.html) ^3

3. [gh jessevig/bertviz](https://github.com/jessevig/bertviz) ^4

3. [paper: A Multiscale Visualization of Attention in the Transformer Model](https://aclanthology.org/P19-3007.pdf) ^5

````bibtex
@inproceedings{vig-2019-multiscale,
    title = "A Multiscale Visualization of Attention in the Transformer Model",
    author = "Vig, Jesse",
    booktitle = "Proceedings of the 57th Annual Meeting of the Association for Computational Linguistics: System Demonstrations",
    month = jul,
    year = "2019",
    address = "Florence, Italy",
    publisher = "Association for Computational Linguistics",
    url = "https://www.aclweb.org/anthology/P19-3007",
    doi = "10.18653/v1/P19-3007",
    pages = "37--42",
}
````
