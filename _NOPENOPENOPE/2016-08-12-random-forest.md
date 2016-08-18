---
layout: post
title:  "Dafuq Is Random Forests?"
date:   2016-08-12
category:
  - Machine Learning
tags:
  - Model
  - Dafuq
---

Random forests is an ensemble model consists of decision trees, each tree in the ensemble is designed with the goal of increasing the randomness of its generation, these randomness originates from:-

* Training sample is sample drawn with replacement from original training set, i.e. a bootstrap sample;
* Feature selected for a node is chosen among a random subset of all the features;


Due to the effect of increased randomness, the bias of the model is usually slightly increased with respect to a single non-random tree, but since it takes the average among many trees, its variance is decreased, usually resulting in an overall better model.
