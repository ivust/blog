---
layout: post
title:  "Weights preinitialization in a ConvNet"
date:   2014-12-10 18:02:00
categories: preinitialization convnet pca
---
One way to look at the transformations performed by deep neural networks is that they somehow extract useful information from the data and at the same time disregard redundant information and noise. So, if the input data are images then the output of the network, that is the activations of the last layer, hopefully consists of some meaningful features of those images which could be used for further processing.

Getting rid of redundant information in the data is in some sense constructing a transformation of the input space in such a way that components of the input data vectors are statistically independent in a transformed space. If we assume that our data set $$D \in \mathbb{R}^n$$ is made of independent samples from the random vector $$x=(x_1, \ldots ,x_n)$$ then that would be a function $$f: \mathbb{R}^n \to \mathbb{R}^n$$ and $$y:=f(x)=(y_1,\ldots,y_m)$$ with $$y_i$$ and $$y_j$$ being independent for any $$i \neq j$$. Under the assumption that at each layer of the network a similar transformation is applied to its input, we may wonder what could be a good way to preinitialize weights in each layer of the network, so that this preinitialization corresponds to a somewhat similar function. A simple way to do that is to make each layer perform a principal component analysis of its input.

*PCA*

PCA consists of two steps: first, we make an orthogonal linear transformation of the input data so that its individual components become uncorrelated (that is linearly independent), and second, we choose several coordinates in the transformed space and keep data only along those coordinates to reduce dimensionality. If $y := f(x) = (y_1, \ldots ,y_n)$, where $f$ is a PCA transformation now, we sort yis in the order of decreasing variances $(Var(y1) \geq Var(y2) \geq \ldots \geq Var(yn))$, so first component "explains more data" than the second and so on. In that case if we keep only first m components, we may reduce dimensionality by not keeping the "unimportant components".

In practice, if we have a matrix of observations X, each row of which is an individual training example, then PCA corresponds to a linear transformation which diagonalizes the covariance matrix XTX. This can be done directly by finding an orthogonal matrix W, such that XTX=WΣWT, where Σ is a diagonal matrix. Alternatively, we can apply SVD to a matrix X: X=UΣWT, where U and W are orthogonal matrices. Then, XTX=(WΣUT)(UΣWT)=WΣ2WT.
