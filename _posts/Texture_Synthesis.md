# Paper review - Texture Synthesis Using Convolutional Neural Networks

## High level overview
The authors introduce a way to synthesize natural texture from an image using deep convolutional networks. They propose a model that takes an image (which we will refer to as target image), computes the texture in the image, and generates an image (which we will refer to as synthesized image) with the texture. In a more technical sense, the network takes the target image along with a random image (random image meaning an image full of Gaussian white noise), and iteratively changes the random image so that it will resemble the texture in the target image.

![image](https://user-images.githubusercontent.com/33714067/121808106-54b06700-cc57-11eb-8a48-87a959ef9551.png)



## In depth understanding
### Gram matrix
To understand how texture is formulated and computed, we must first look into what the authors refer to as a 'Gram matrix'. A Gram matrix is a covariance matrix of features. Neural networks with multiple layers have intermediate results, and the results can be understood as a multidimensional array of features. For example, in a VGG19 network, we would have features from the first convolutional layer, all the way up to the fifth pooling layer. 

####Image of VGG19

With a feature array, one can compute the Gram matrix by taking its covariance. 

####Equation


### Gram loss


### Reconstruction process
