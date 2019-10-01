# Densely Connected Convolutional Networks (DenseNet)
Gao Huang, Zhuang Liu, Laurens van der Maaten, Kilian Q. Weinberger

## Summary
This paper proposes a Densely Connected Convolutional Networks (DenseNet) which is an extenstion of the ResNet idea , in which now every layer is connected to every other layer , and in which the combination is done via concatenation instead of simple summation . 
It has significantly less parameters than ResNet and also achieived SOTA performance in many object recognition tasks.  
**This paper also got the Best Paper Award in CVPR 2017**

### Overview

- The main idea behind the paper is to connect every layer with every other layer in the network . This is done is a feed-forward fashion meaning that all the preceeding layers are conntected to the current layer , and its output is connected to the input of all the subsequent layers.
<img src='https://github.com/ayushtues/papers_we_read/blob/master/images/densenet.png' style="max-width:100%">
