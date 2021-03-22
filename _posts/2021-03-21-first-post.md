---
layout: post
title: "First post: A test"
description: "Learning how to use fastpages."
author: "Nathan Tsai"
toc: true
comments: false
image:
hide: false
search_exclude: true
show_tags: false
sticky_rank: 1
categories: [fastpages, markdown]
permalink: /first-post
---

# Title after TOC

## Introduction
The question to what is the difference between ML and AI is answered by Judea Pearl in The Book of Why.

Machine learning also has intimate ties to optimization: many learning problems are formulated as minimization of some loss function on a training set of examples. Loss functions express the discrepancy between the predictions of the model being trained and the actual problem instances (for example, in classification, one wants to assign a label to instances, and models are trained to correctly predict the pre-assigned labels of a set of examples).

The difference between optimization and machine learning arises from the goal of generalization: while optimization algorithms can minimize the loss on a training set, machine learning is concerned with minimizing the loss on unseen samples. Characterizing the generalization of various learning algorithms is an active topic of current research, especially for deep learning algorithms.


This is the formula: $$E=mc^2$$

$$y=xA^T+b$$


## This is it

Reasons:
- Machine learning and statistics are closely related fields in terms of methods, but distinct in their principal goal: statistics draws population inferences from a sample, while machine learning finds generalizable predictive patterns.

- Leo Breiman distinguished two statistical modeling paradigms: data model and algorithmic model, wherein "algorithmic model" means more or less the machine learning algorithms like Random forest.

Why:
1. A core objective of a learner is to generalize from its experience. Generalization in this context is the ability of a learning machine to perform accurately on new, unseen examples/tasks after having experienced a learning data set. The training examples come from some generally unknown probability distribution (considered representative of the space of occurrences) and the learner has to build a general model about this space that enables it to produce sufficiently accurate predictions in new cases.

1. The computational analysis of machine learning algorithms and their performance is a branch of theoretical computer science known as computational learning theory. Because training sets are finite and the future is uncertain, learning theory usually does not yield guarantees of the performance of algorithms. Instead, probabilistic bounds on the performance are quite common. The bias–variance decomposition is one way to quantify generalization error.

## Quote this
> Machine learning (ML) is the study of computer algorithms that improve automatically through experience and by the use of data. Machine learning algorithms are used in a wide variety of applications, such as email filtering and computer vision, where it is difficult or unfeasible to develop conventional algorithms to perform the needed tasks.

A subset of machine learning is closely related to computational statistics, which focuses on making predictions using computers; but not all machine learning is statistical learning [^1]. The study of mathematical optimization delivers methods, theory and application domains to the field of machine learning. Data mining is a related field of study, focusing on exploratory data analysis through unsupervised learning. In its application across business problems, machine learning is also referred to as predictive analytics.

The discipline of machine learning employs various approaches to teach computers to accomplish tasks where no fully satisfactory algorithm is available. In cases where vast numbers of potential answers exist, one approach is to label some of the correct answers as valid. This can then be used as `training data` for the **computer** to improve the algorithm(s) it uses to determine correct answers. For example, to train a system for the task of digital character recognition, the *MNIST dataset* of handwritten digits has often been used.

---

Machine learning approaches are traditionally divided into three broad categories, depending on the nature of the "signal" or "feedback" available to the learning system:

{% include alert.html text="Whoa!" %}

Other approaches have been developed which don't fit neatly into this three-fold categorisation, and sometimes more than one is used by the same machine learning system. For example topic modeling, dimensionality reduction or meta learning.

{% include info.html text="You should know..." %}

Modern day machine learning has two objectives, one is to classify data based on models which have been developed, the other purpose is to make predictions for future outcomes based on these models. A hypothetical algorithm specific to classifying data may use computer vision of moles coupled with supervised learning in order to train it to classify the cancerous moles. Where as, a machine learning algorithm for stock trading may inform the trader of future potential predictions.

## Image

![]({{ site.baseurl }}/images/logo.png "fast.ai's logo")


## Code
As a scientific endeavor, machine learning grew out of the quest for artificial intelligence. In the early days of AI as an academic discipline, some researchers were interested in having machines learn from data. They attempted to approach the problem with various symbolic methods, as well as what was then termed "neural networks"; these were mostly perceptrons and other models that were later found to be reinventions of the generalized linear models of statistics.:488

    # do something
    import torch
    weights = model.run()

Python

```python
# color code
import torch
weights = model.run()
for i in range(10):
    print(i)
    break
return ans
```
```
[1.0, 2.0, -3.4]
```

Shell
```shell
./run.sh --model_name "LinearRegression"
```

## Table

| Loss | Accuracy |
| ---- | -------- |
| 10.4 |  92.45%   |


## Tweet

{% twitter https://twitter.com/karpathy/status/1325154823856033793 %}

## Footnotes

[^1]: Do you even read this?
