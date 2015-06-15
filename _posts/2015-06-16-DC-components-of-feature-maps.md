---
layout: post
title:  "DC components of feature maps"
comments: true
date:   2015-06-16 01:01:00
---

In this experiment we computed the slowness of DC components (means of feature) of all convolutional layers. The Euclidean distances are the following:

![]({{ site.url }}{{ site.baseurl }}/images/dc_e1.png)
![]({{ site.url }}{{ site.baseurl }}/images/dc_e2.png)
![]({{ site.url }}{{ site.baseurl }}/images/dc_e3.png)
![]({{ site.url }}{{ site.baseurl }}/images/dc_e4.png)
![]({{ site.url }}{{ site.baseurl }}/images/dc_e5.png)

It seems that DC components become more similar as the learning progresses regardless of the similarity of input images, which probably makes those DC components not an appropriate objective function for learning.

The angles between DC components look like this:

![]({{ site.url }}{{ site.baseurl }}/images/dc_a1.png)
![]({{ site.url }}{{ site.baseurl }}/images/dc_a2.png)
![]({{ site.url }}{{ site.baseurl }}/images/dc_a3.png)
![]({{ site.url }}{{ site.baseurl }}/images/dc_a4.png)
![]({{ site.url }}{{ site.baseurl }}/images/dc_a5.png)

The angles also show a general trend for becoming more similar (DC components vectors become more parallel), rather then a discriminative behaviour for different types of input images.
