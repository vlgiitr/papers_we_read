# Densely Connected Convolutional Networks (DenseNet)
Gao Huang, Zhuang Liu, Laurens van der Maaten, Kilian Q. Weinberger

## Summary
This paper proposes a Densely Connected Convolutional Networks (DenseNet) which is an extension of the ResNet idea, in which now every layer is connected to every other layer, and in which the combination is done via concatenation instead of simple summation. 
It has significantly fewer parameters than ResNet and also achieved SOTA performance in many object recognition tasks.  
**This paper also got the Best Paper Award in CVPR 2017**

### Overview

- The main idea behind the paper is to connect every layer with every other layer in the network. This is done is a feed-forward fashion meaning that all the preceding layers are connected to the current layer, and its output is connected to the input of all the subsequent layers.
<img src='https://github.com/ayushtues/papers_we_read/blob/master/images/densenet.png' style="max-width:100%">

- As the concatenation operation is valid for feature maps of same sizes only, the network consists of dense blocks which contain such connected layers followed by **transition layers** , which is basically a 1x1 Conv followed by 2x2 average pooling to reduce the size of feature maps.
- Each layer adds k new feature maps to the network, this is called **growth rate**.
- **Bottlenecks** which are 1x1 Conv that occur before 3x3 convs are present in denseblocks to improve computation
- The number of feature maps from a denseblock is scaled m times (called **compression factor** ) via the transition layer.
- Densenet with both bottlenecks and compression are called **Densenet-BC**

### Implementation Details

- They used three Denseblocks that had an equal number of layers. 3x3 convs with zero padding of 1 was used to preserve feature map size. 1x1 convs followed with 2x2 average pooling for transition layers .
- The feature-map sizes in the three dense blocks are 32× 32, 16×16, and 8×8, respectively.
- Trained with SGD using Nesterov Momentum without dampening along with dropout. 

### Strengths
- As every layer is connected there is a lot of feature reuse and hence a small number of feature maps per layer (small k) also leads to good performance which is a clear improvement over ResNets. So it has fewer parameters and comparable accuracy.
- As a layer is closely connected with the output and input layers, so they kind of receive direct supervision and hence it improves gradient and information flow.
- They are also shown to have a regularizing nature and are less prone to over-fitting.

### Weakness
- The paper could have had better figures describing its architecture.
- The regularisation nature could have been explored better.
- They could have combined the concatenation operation of DenseNet with the summation operation of Resnets to maybe explore  

### Implementaion
https://github.com/liuzhuang13/DenseNet

