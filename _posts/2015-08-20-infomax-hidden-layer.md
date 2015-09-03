---
layout: post
title:  "Infomax with a hidden layer"
comments: true
date:   2015-08-20 19:40:00
---

I tried to train with Infomax a network with a hidden convolutional layer, namely the first three layer of VGG-network (conv1_1, conv2_2 and pool1). The Infomax was only computed for pool1 and receptive fields were visualised by backpropagationg the gradients.

For a fully trained VGG-network, conv1_1, conv1_2 and pool1 look like this:

![]({{ site.url }}{{ site.baseurl }}/images/vgg3/actual1.png)
![]({{ site.url }}{{ site.baseurl }}/images/vgg3/actual2.png)
![]({{ site.url }}{{ site.baseurl }}/images/vgg3/actual3.png)

For the Infomax-trained network, conv1_1 looks like this (depending on the amount of regularization):

![]({{ site.url }}{{ site.baseurl }}/images/vgg3/c1-1.png)
![]({{ site.url }}{{ site.baseurl }}/images/vgg3/c1-2.png)
