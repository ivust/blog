---
layout: post
title:  "Continuing with weights preinitialization"
date:   2015-01-05 18:55:00
---

I have added an additional computational layer to the network to see if preinitialization becomes more advantageous as the network gets deeper. I have also made a network with less feature maps in each layer (6, 16 and 25 compared to 20 and 50 before) to be able to train it for a longer time. This time the preinitialization was also done using random orthogonal matrices (closest orthogonal matrix to a random matrix) to see if the effect of preinitialization using PCA is just due to the orthogonality of the matrices computing the PCA.

![]({{ site.url }}{{ site.baseurl }}/images/preinitialization_3.png)

The preinitialization clearly helps. Though it is not exactly clear if PCA helps compared to random orthogonal matrices: it performed a bit better (converged to an approximately final value (after 100 epochs) of 1.1% error after ~40 epochs, while the other net has been learning over the course of all 100 epochs and the final value is ~1.6% error), though this may happen because of bad random matrix (close to a bad local minimum) or be architecture-specific. So, probably it makes sense to do these experiments using more complex data set to try larger networks.
