---
layout: post
title: Review Score Prediction - Ordinal Regression VS Multiclass Classification
date: 2020-07-16
tags: [DataScience]
---

#### Problem ####

One project I am working on is to predict review scores, with a range from 1 to 5.
1 is strong disagree while 5 is strong agree.

We have tried modeling using ordinal regression and multiclass classification.
By comparing the performance, we found that multiclass models achieved a much better recall/F1 scores than ordinal models, especially for the neutral response classes (people who scoring 3). However, this is a very classic ordinal regression problem.

So, I decide to do some research about these two types of models, here are my notes.

#### From Wikipedia ####

* Ordinal Regression


It is a type of regression analysis used for predicting an ordinal variable, i.e. a variable whose value exists on an arbitrary scale where only the relative ordering between different values is significant. It can be considered an intermediate problem between regression and classification.

Ordinal regression turns up often in the social sciences, for example in the modeling of human levels of preference (on a scale from, say, 1–5 for "very poor" through "excellent")


* Multicalss Classification


Multiclass or multinomial classification is the problem of classifying instances into one of three or more classes (classifying instances into one of two classes is called binary classification).

<br />
<br />
<br />
<br />
