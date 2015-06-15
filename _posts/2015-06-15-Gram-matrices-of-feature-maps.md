---
layout: post
title:  "Slowness learning"
comments: true
date:   2015-06-15 22:51:00
---

Another objective function for an unsupervised training of a convolutional net could be related to some kind of "slowness" of the feature maps of hidden layers. Here we computed the Euclidean distances between Gram matrices of feature maps in convolutional layers for similar images taken from a movie,  imagenet images and their phase-scrambled versions, and between random images taken from the imagenet.

![]({{ site.url }}{{ site.baseurl }}/images/gram1.png)
![]({{ site.url }}{{ site.baseurl }}/images/gram2.png)
![]({{ site.url }}{{ site.baseurl }}/images/gram3.png)
![]({{ site.url }}{{ site.baseurl }}/images/gram4.png)
![]({{ site.url }}{{ site.baseurl }}/images/gram5.png)
