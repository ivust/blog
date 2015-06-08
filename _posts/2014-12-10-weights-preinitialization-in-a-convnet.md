---
layout: post
title:  "Weights preinitialization in a ConvNet"
date:   2014-12-10 18:02:00
categories: preinitialization convnet pca
---
One way to look at the transformations performed by deep neural networks is that they somehow extract useful information from the data and at the same time disregard redundant information and noise. So, if the input data are images then the output of the network, that is the activations of the last layer, hopefully consists of some meaningful features of those images which could be used for further processing.
