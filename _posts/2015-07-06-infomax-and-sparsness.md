---
layout: post
title:  "Infomax and sparseness"
comments: true
date:   2015-07-06 23:48:00
---

For this experiment I switched from AlexNet to a VGG-like net (but smaller). It has seven convolutional layers with $$ 3 \times 3 $$ filters followed by ReLU nonlinearities and five pooling layers.

I considered two nets of that architecture: one was trained just like any regular net with gradients propagated from the topmost layer down through other layers; and the other where the mean feature maps were computed for each pooling layer and those mean features were the inputs to the fully connected layers on top of the net. That allowed to pass the gradients directly in the middle of the net, which facilitated learning.

<!--more-->

## Infomax

First I computed the Gaussian entropies of vectors of mean feature maps of pool layers for those two nets. The one trained with mean features computed during training has the following log-eigenvalue spectra (the Gaussian entropy is the sum of those values):

![]({{ site.url }}{{ site.baseurl }}/images/infomax/mean2.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax/mean3.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax/mean4.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax/mean5.png)

And those are for the net trained as usual:

![]({{ site.url }}{{ site.baseurl }}/images/infomax/pool1.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax/pool2.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax/pool3.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax/pool4.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax/pool5.png)

## Sparseness

To estimate the sparseness of mean feature maps, I computed the ratio of the $$ L_1 $$ norm to the $$ L_2 $$ norm (the close this ratio to one, the sparser the activations) and their entropy. Fisrt, for the net train with mean feature maps:

![]({{ site.url }}{{ site.baseurl }}/images/infomax/mean_l1l2.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax/mean_entropy.png)

And for the regular net:

![]({{ site.url }}{{ site.baseurl }}/images/infomax/pool_l1l2.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax/pool_entropy.png)
