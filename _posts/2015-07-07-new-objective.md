---
layout: post
title:  "New objective"
comments: false
date:   2015-07-07 11:00:00
---

New objective ($$ N $$ is the number of feature maps in a given layer):

$$ E = \sum\limits_{j = 1}^{N} {< e^{2 \beta_j} \times ( \beta_j - \log{< e^{\beta_j}> )^2 } )>} $$

Sum is taken over the feature maps in a given layer. $$ \beta_j $$ is a single activation in the $$ j $$-th feature map. Averages are taken over both the images and the spatial dimensions of the feature maps.

I did the evaluation of this new objective function with exponents in it. The biggest problem was the large differences in activations of individual neurons, therefore taking the exponents was problematic. Float128 allows to take the exponents of values only up to ~11000, therefore I used the package "decimal", which allows to work with arbitrary large numbers but it is very slow.

I also renormalized the snapshots so that the feature maps have the mean of 1/1000 (instead of 1) to have fewer large values and speed up the computation by not having to deal with the exponents of them. However, it creates problems with really small numbers, which are all being essentially mapped to 1 by the exponents, and since the resulting values of the objective are also quite small, this could matter. However I haven't checked if the potential numerical problems with those numbers affect the result.

![]({{ site.url }}{{ site.baseurl }}/images/new_objective/pool1.png)

Pool2 diverged to very large numbers for some snapshots.

For pool3, pool4 and pool5 the estimate for the random (iteration 0) snapshot diverged:

![]({{ site.url }}{{ site.baseurl }}/images/new_objective/pool3.png)
![]({{ site.url }}{{ site.baseurl }}/images/new_objective/pool4.png)
![]({{ site.url }}{{ site.baseurl }}/images/new_objective/pool5.png)

