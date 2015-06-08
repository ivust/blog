---
layout: post
title:  "Weights preinitialization in a ConvNet"
date:   2014-12-10 18:02:00
categories: preinitialization convnet pca
---
One way to look at the transformations performed by deep neural networks is that they somehow extract useful information from the data and at the same time disregard redundant information and noise. So, if the input data are images then the output of the network, that is the activations of the last layer, hopefully consists of some meaningful features of those images which could be used for further processing.

Getting rid of redundant information in the data is in some sense constructing a transformation of the input space in such a way that components of the input data vectors are statistically independent in a transformed space. If we assume that our data set $D \in \mathbb{R}^n$ is made of independent samples from the random vector x=(x1,…,xn) then that would be a function f:Rn→Rm and y:=f(x)=(y1,…,ym) with yi and yj being independent for any i≠j. Under the assumption that at each layer of the network a similar transformation is applied to its input, we may wonder what could be a good way to preinitialize weights in each layer of the network, so that this preinitialization corresponds to a somewhat similar function. A simple way to do that is to make each layer perform a principal component analysis of its input.
