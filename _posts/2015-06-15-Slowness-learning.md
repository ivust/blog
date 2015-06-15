---
layout: post
title:  "Slowness learning"
comment: true
date:   2015-06-15 22:07:00
---

The idea of Slowness learning is to (unsupervisely) train the convolutional net by making it capture the similarities between perceptually similar images. For this experiment we would take neighboring (with sampling rate of 2 for the figure below) frames from a movie, which are assumed to be perceptually similar. However, the distance in pixel space between such frames is very large, that is why some higher level objective function is needed.

One such function could be the class labels outputs, which are forced to be equal for similar images. The labels assignment could be random or more meaningful by using the classes predicted by the net. We tried taking batches of 1000 images, pushing them through the net and coputing the class labels in the following way:

* For each image, class labels were sorted according to their activations
* A given image was assigned a class label correspoding to a highest activation if it was not taken before (assignment is unique: 1000 images --- 1000 classes). If it was, then the second highest and so on.
* This procedure was repeated to the tenth highest label and after that all images that have not been assigned a label so far were given random labels (adding some random labels helps to prevent the net collapsing to outputing the same labels for any image)

After showing 2.56 million images (256 iterations of 10 batches of 1000 image each) the receptive fieds in conv1 looked like this:

![]({{ site.url }}{{ site.baseurl }}/images/slowness_iter256.png)

We also tried different labels assignments (for a given label finding an movie which prdocues highest activation, for example) and different number of labels (100 and 500), but the results were not any better.
