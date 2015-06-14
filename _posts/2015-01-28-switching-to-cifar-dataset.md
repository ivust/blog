---
layout: post
title:  "Switching to CIFAR dataset"
comments: true
date:   2015-01-28 10:11:00
---

I have tried using a more complex [CIFAR](http://www.cs.toronto.edu/~kriz/cifar.html) dataset, which contains 50000 colored images of size 32x32, each belonging to one of ten classes. The network I used had 3 layers (6, 16 and 25 feature maps respectively). The overall is not good, which suggests that a more complex architecture and probably longer training should be used. The preinitialization helps, but not as much as for the MNIST.

![]({{ site.url }}{{ site.baseurl }}/images/plot_2.png)
