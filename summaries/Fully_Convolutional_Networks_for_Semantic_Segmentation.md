# Fully Convolutional Networks for Semantic Segmentation
Jonathan Long, Evan Shelhamer, Trevor Darrell, CVPR 2015

## Summary
This paper shows an end to end, pixel to pixel trained comletely convolutional network capable of performing semantic segmentation tasks from classic 
networks used in classification task like AlexNet, VGGnet and GoogleNet by replacing their fully connected layers with convolutional ones and adding skip
connections.

## Overview

-The main idea behind this paper is to replace the fully connected layers found in the last of most classification networks to form a fully convolutional
network that can output the semantically segmented form of a given image.
-This networks also makes use of transfer learning to transfer the learned representations of the orignal classification networks and then using stochastic 
gradient descent with momentum to fine tune the parameters for our goal.
-There is use of skip connections between the middle and final layers of the network to allow for dense predictions.
-Shift and stitch techniques are also used to obtain dense predictions.

## Implementation Details

-The structure of the networks is as following:
<img src='images/Fully_Convolutional_Networks_for_Semantic_Segmentation.png' style="max-width:100%">
-Use of transfer learning is made to transfer the learned representations of the orignal classification networks and then using stochastic gradient
descent with momentum to fine tune the parameters for semantic segmentation tasks.

## Strengths

-Capable of handling input image of any dimensions and outputing the dense predictions for that.
-Makes good use of known classification networks by the method of transfer learning.
-The use of conv layers instead of FC layers would reduce the number of parameters and the skip connections would also help in reducing the training effort
by improving gradient flows.

## Weaknesses

-Some of the techniques used like, patchwise training and shift and stich would increase the computational effort to be put in.


## Implementation
[https://github.com/shelhamer/fcn.berkeleyvision.org](https://github.com/shelhamer/fcn.berkeleyvision.org)
