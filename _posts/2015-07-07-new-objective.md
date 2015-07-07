---
layout: post
title:  "New objective"
comments: false
date:   2015-07-07 11:00:00
---

New objective:

$$ E = \sum\limits_{j = 1}^{64} {< e^{2 \beta_j} \times ( \beta_j - \log{< e^{\beta_j}> )^2 } )>} $$

Sum is taken over the feature maps in a given layer. $$ \beta_j $$ is a single activation in the j-th feature map. Averages are taken over the images and the spatial dimensions of the feature maps.
