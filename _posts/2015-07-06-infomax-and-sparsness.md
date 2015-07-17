---
layout: post
title:  "Infomax on actual features"
comments: true
date:   2015-07-18 01:27:00
---

I checked the Infomax on actual features instead of mean feature maps. Activations at the same spatial locations in the features maps of a given layer are considered as an individual sample. In total there are (number of images in a batch) x (size of a feature map)^2 samples, which are used to estimate the covariance matrix. 

![]({{ site.url }}{{ site.baseurl }}/images/infomax_actual/1.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax_actual/2.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax_actual/3.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax_actual/4.png)
![]({{ site.url }}{{ site.baseurl }}/images/infomax_actual/5.png)

