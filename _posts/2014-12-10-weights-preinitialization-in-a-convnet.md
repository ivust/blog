---
layout: post
title:  "Weights preinitialization in a ConvNet"
date:   2014-12-10 18:02:00
categories: preinitialization convnet pca
---
One way to look at the transformations performed by deep neural networks is that they somehow extract useful information from the data and at the same time disregard redundant information and noise. So, if the input data are images then the output of the network, that is the activations of the last layer, hopefully consists of some meaningful features of those images which could be used for further processing.

Getting rid of redundant information in the data is in some sense constructing a transformation of the input space in such a way that components of the input data vectors are statistically independent in a transformed space. If we assume that our data set $$D \in \mathbb{R}^n$$ is made of independent samples from the random vector $$x=(x_1, \ldots ,x_n)$$ then that would be a function $$f: \mathbb{R}^n \to \mathbb{R}^n$$ and $$y:=f(x)=(y_1,\ldots,y_m)$$ with $$y_i$$ and $$y_j$$ being independent for any $$i \neq j$$. Under the assumption that at each layer of the network a similar transformation is applied to its input, we may wonder what could be a good way to preinitialize weights in each layer of the network, so that this preinitialization corresponds to a somewhat similar function. A simple way to do that is to make each layer perform a principal component analysis of its input.

**PCA**

PCA consists of two steps: first, we make an orthogonal linear transformation of the input data so that its individual components become uncorrelated (that is linearly independent), and second, we choose several coordinates in the transformed space and keep data only along those coordinates to reduce dimensionality. If $$y := f(x) = (y_1, \ldots ,y_n)$$, where $$f$$ is a PCA transformation now, we sort yis in the order of decreasing variances $$(Var(y1) \geq Var(y2) \geq \ldots \geq Var(yn))$$, so first component "explains more data" than the second and so on. In that case if we keep only first m components, we may reduce dimensionality by not keeping the "unimportant components".

In practice, if we have a matrix of observations $$X$$, each row of which is an individual training example, then PCA corresponds to a linear transformation which diagonalizes the covariance matrix $$X^{T}X$$. This can be done directly by finding an orthogonal matrix $$W$$, such that $$X^{T}X = W \Sigma W^T$$, where $$\Sigma$$ is a diagonal matrix. Alternatively, we can apply SVD to a matrix $$X$$: $$X = U \Sigma W^T$$, where $$U$$ and $$W$$ are orthogonal matrices. Then, $$X^T X = (W \Sigma U^T)(U \Sigma W^T) = W \Sigma^2 W^T$$.

**Convolutional networks**

The specific neural network architecture that I used to try weights preinitialization is called convolutional neural network.

![Convolutional net]({{ site.url }}{{ site.baseurl }}/images/cnn_architecture.jpg)

Each neuron in the following layer receives input from a subset of units in the previous layer weights of connections are the same for any location of the neuron. So we scan the input image with this filter and get activations of the next layer. We may want to have different filters to extract different features from an image therefore forming several transformed images which are called feature maps. If input is not an image (like the input to the first layer) but a set of features maps, then each neuron receives input from all the feature maps in the previous layer. That is, if the filter size is $$k \times k$$ and there are n feature maps, then each neuron in each of the feature maps in the following layer receives input from $$n \times k \times k$$ neurons. Each image in the training set produces activations of $$n$$ feature maps in the previous layer and those activations can be cut into multiple patches of size $$k \times k$$ to make samples which are input to the following convolutional layer. And using those samples we can compute the PCA transformations and preinitialize the weights.
