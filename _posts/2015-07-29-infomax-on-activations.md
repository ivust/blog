---
layout: post
title:  "Infomax on actual activations"
comments: true
date:   2015-07-29 21:36:00
---

I started training a one-layer ConvNet using Infomax on actual activations and not on the mean feature maps. If there are $$ k $$ feature maps in a given layer of size $$ n \times n $$, then we consider vectors of dimensionality $$ k $$, each element of which is an activation in a corresponding feature map located at the same position. In that case we get $$ b \times n \times n $$ ($$ b $$ is the batch size) vectors to estimate the covariance matrix $$ C $$.

The objective functions is then 

$$ -\log{\text{det}(C)} + \alpha \times (\text{Trace}(C) - k)^2 $$

The second term is to limit the variances of activations. $$ \alpha $$ sets the importance of this regularization criterion. The following figure shows the convolutional filters, $$ \alpha $$ was set to $$ 10^{-12} $$ (though optimization was performed without momentum and weight decay):

![]({{ site.url }}{{ site.baseurl }}/images/infomax_activations/50000.png)

And those are the filters obtained without limiting the variances of activations ($$ \alpha = 0 $$):

![]({{ site.url }}{{ site.baseurl }}/images/infomax_activations/20000.png)
