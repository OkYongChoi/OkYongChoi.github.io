---
title: "GoogLeNet"
toc: true
categories:
  - Computer Vision
tags:
  - image classification
  - imgae localization
  - 
---

# GoogLeNet (Inception v1)
GoogLeNet consists of 22 layers. If you count the number of floors in the blue block, you can see that there are 22 floors. In the figure below, blue units indicate convolutional layers, red units indicate max-pooling units, yellow units indicate softmax layers, and green units indicate other functions.
![](https://production-media.paperswithcode.com/methods/Screen_Shot_2020-06-22_at_3.28.59_PM.png)

[GoogLeNet](https://ethereon.github.io/netscope/#/preset/googlenet)

## 1x1 Convolution
What does 1 x 1 convolution mean? In GoogLeNet, 1 x 1 convolution is used to reduce the number of feature maps. As the number of characteristic maps decreases, the amount of computation decreases.

For example, suppose that there are 14 x 14 x 480 feature maps. If this is convolved with 48 5 x 5 x 480 filter kernels, 14 x 14 x 48 feature maps are generated. In this example,(14 x 14 x 48) x (5 x 5 x 480) = about 112.9M numbers of operations are required.

This time, let's reduce the number of feature maps by convolving 14 x 14 x 480 feature maps with 16 1 x 1 x 480 filter kernels first. As a result, 14 x 14 x 16 feature maps  are generated. Note that the 480 feature maps have been reduced to 16 feature maps. If this 14 x 14 x 16 feature map is convolved with 48 5 x 5 x 16 filter kernels, 14 x 14 x 48 feature maps are generated. Check that the size and depth of the resulting feature map are the same as when there is no 1 x 1 convolution above. So, how many calculations are needed at this time? (14 x 14 x 16) x (1 x 1 x 480) + (14 x 14 x 48) x (5 x 5 x 16) = about 5.3M, which is much smaller than 112.9M above!!
## Inception Module
GoogLeNet referenced the Network in network (NIN) paper structure. Just as NIN designed a basic module called Mlpconv and used a structure that nested it, GoogLeNet uses a structure that nested a basic module called **Inception**. GoogLeNet includes a total of 9 Inception modules.
![GoogLeNet | PyTorch](https://pytorch.org/assets/images/googlenet1.png)
GoogLeNet tried to utilize several convolutions in parallel to extract more diverse features from the local receptive field. Originally, as shown in Figure (a) above, a structure was devised to arrange 1x1, 3x3, 5x5 conv and 3x3 max pooling side by side, and this is a suitable structure for extracting features of various scales. However, 3x3 and 5x5 conv are expensive units than 1x1 conv in terms of the amount of computation. 

So, as shown in Figure (b), if 1x1 conv is placed in a cascade structure in front of 3x3 conv and 5x5 conv, and the dimension of feature-map is reduced through 1x1 conv, various scales for feature extraction are secured while balancing the amount of computation. be able to match The reason GoogLeNet can deepen up to 22 layers may be because the amount of computation can be controlled through 1x1 conv.

![GoogLeNet (InceptionV1) with TensorFlow | by mrgrhn | Artificial  Intelligence in Plain English](https://miro.medium.com/max/1400/0*q5eMDjUHKqEyo7qY.png)
## Auxiliary Classifier
The deeper the network, the more difficult it is to avoid the vanishing gradient problem. That is, back propagation is mainly used in the process of training the weights, and the gradient used to update the weights in the process of back propagation gradually decreases and becomes 0. Therefore, the weights in the network are not properly trained. To overcome this problem, in GoogLeNet, two auxiliary classifiers are added in the middle of the network. These auxiliary classifiers are used only during training and are removed when used.
![Auxiliary Classifier Explained | Papers With Code](https://production-media.paperswithcode.com/methods/GoogleNet-structure-and-auxiliary-classifier-units_CM5xsxk.png)
 The configuaration of the auxiliary classifier: 5 x 5 average pooling with stride 3 -> convolution with 128 1x1 filter kernels 1024 FC layers -> 1000 FC layers -> softmax.

## Global Average Pooling
In order to use the softmax layer for image classification, the feature maps need to be made into a one-dimensional vector. While FC uses many parameters in this process,, global average pooling does not require additional weights in this process.
![Global average pooling layer replacing the fully connected layers. The... |  Download Scientific Diagram](https://www.researchgate.net/profile/Pantelis-Kaplanoglou/publication/318277197/figure/fig10/AS:513490781966337@1499437150580/Global-average-pooling-layer-replacing-the-fully-connected-layers-The-output-layer.png)

In AlexNet and VGGNet, fully connected (FC) layers are connected to the latter part of the network. However, GoogLeNet uses a method called global average pooling instead of the FC method. 
