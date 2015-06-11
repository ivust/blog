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

**Experiments**

I used a convolutional network with 2 convolutional layers of 20 and 50 feature maps respectively and the filter size of $$5 \times 5$$. Each convolutional layer is followed by the max-pooling layer over a $$2 \times 2$$ region. The code I used is based on [this](https://github.com/Newmu/Theano-Tutorials/blob/master/5_convolutional_net.py) implementation of convolutional net. The preinitialization of the first layer is implemented as described above:

{% highlight python %}
for image in range(len(trX)): # Iterate over all images in the training set
  for patch_index in range(10): # Ten samples per each images
    (center_x, center_y) = np.random.randint(2, 26, size=2) # Choose center of the sample randomly
    patch = trX[image][0][center_x-2:center_x+3, center_y-2:center_y+3] # Make a 5x5 patch
    patch = patch.reshape(1, 5*5) # Reshape into a vector
    samples_5x5[image*patch_index] = patch # Add to a matrix of samples
cov_samples = np.cov(samples_5x5.transpose()) # Covariance matrix
(U, d, W) = np.linalg.svd(cov_samples) # SVD of a covariance to compute transformation to a eigenbasis
new_w = np.zeros((20, 1, 5, 5)) # Initialize new weights
for next_feature_map in range(20):
  # Set weights to each of a feature maps in the next layer to a corresponding principal component
  new_w[next_feature_map][0] = W[:,next_feature_map].reshape(5, 5) 
w.set_value(new_w) # Update weights
{% endhighlight %}

The second layer is preinitialized in the same way with the only difference that input is not a single image but a set of feature maps:

{% highlight python %}
# Function to compute output of the first layer (20 feature maps)
first_layer_output_function = theano.function(inputs=[X], outputs=l1, allow_input_downcast=True)
first_layer_output = first_layer_output_function(trX)

# Initialize matrix of samples from the first layer output. For each image in the training set
# 5 patches of size 5x5 are made in each of the 20 feature maps 
samples_20x5x5 = np.zeros((len(trX) * 5, 20*5*5))

for image in range(len(trX)): # Loop over first layer output produced by each of the images in the training set
  for patch_index in range(5): # 5 patches for each image in each feature map
    sample = np.array([[]]) # Initialize vector for a current sample (will be of length 20x5x5)
    for feature_map in range(20):
      (center_x, center_y) = np.random.randint(2, 10, size=2) # Choose random center of a patch
      patch = first_layer_output[image][feature_map][center_x-2:center_x+3, center_y-2:center_y+3]
      patch = patch.reshape(1, 5*5)

      # Add samples from each of the features maps together to make a 20x5x5 input space to the next layer
      sample = np.concatenate((sample, patch), axis=1) 
    samples_20x5x5[image*patch_index] = sample
cov_samples = np.cov(samples_20x5x5.transpose()) # Covariance matrix of the input to the next layer
(U, d, W) = np.linalg.svd(cov_samples) # SVD to compute the eigenbasis
new_w = np.zeros((50, 20, 5, 5))
for next_feature_map in range(50):
  # Set weights to each of a feature maps in the next layer to a corresponding principal component
  new_w[next_feature_map] = W[:, next_feature_map].reshape(20, 5, 5)
w2.set_value(new_w) # Update the weights
{% endhighlight %}

By default, all the weights are preinitialized with random numbers drawn from a normal distribution with zero mean and variance of 0.01.The preinitialization by computing the PCA tranformations improves the performance of the network. Below is the plot showing the test error against the nuber of training epochs with two, one or none of the convolutional layers preinitialized with PCA.

![]({{ site.url }}{{ site.baseurl }}/images/plot_1.png)


