
# Graph U-Nets

Hongyang Gao, Shuiwang Ji, ICML-2019

## Summary

This paper proposes a U-Net like architecture for graphical data and tries pretty good performance on node classification and graph classification tasks. Also for this task, they develop a novel pooling and unpooling techniques for graphical data, which is essential to get wider perspective during classification process, just like in the case of simple image data.

## Main contributions

- The main contribution of this paper is its novel pooling and unpooling technique which has been incorporated in their U-Net architecture(which is similar to as for the case of images) to obtain better results on graph classification problems.

- The simple pooling operation for image data cannot possibly work in the case of graphical data as the operation is reliant on spatial information while there is no sense of spatial information in the case of graphs.

<img src='https://github.com/vlgiitr/papers_we_read/blob/master/images/graph_unet.png'>

- So for **gPool operation**, we take a projection of **X**(The information matrix for nodes) to make it a 1D vector and then take *top-k* nodes, where k is according to our need. Once the index of the *top-k* nodes are noted, the rows from **X** corresponding to them are kept while others are removed. So we get a reduced information matrix out of this process.

- However to include the projection vector **p** in the training process so that it is also learned, we multiply the *sigmoid* of *top-k* nodes to the reduced information matrix found above to get the final reduced information matrix. Also to get a new Adjacency matrix we just pick the entries corresponding to the remaining nodes and hence obtain a reduced adjacency matrix for our reduced information matrix.

- For **gUnpool operation**, we just reverse the above process by adding zeroes in the information matrix in the exact places which were removed in the *top-k* selection process. And also to get the corresponding adjacency matrix, we just simply inflate the reduced adjaceny matrix with the same values(i.e. connection information) that was removed in the gPool operation.

- Once these operations are decided, the author moves forward with the U-Net architecture, which is composed of encoder and decoder parts. The encoder consists of Convolutions and gPool operations to reduce the graph size and get useful information, while decoder consists of Convolutions and gUnpool operations to get the graph to its original size and also classify its nodes from the information extracted by Encoder. Skip connections are used between Encoder and Decoder.

## Implementation Details

- For whole of their experiments, they have modified the convolution operation by using **A+2I** rather than **A+I** (a pretty common practise that is followed before convolution), where A is adjacency matrix and I is the identity matrix. The authors argue that more emphasis on node being operated upon lead to better results.

- **Graph Connectivity Augmentation using Graph Power** is a technique used by authors to deal with the possibility of isolated nodes because of pooling operation. To deal with this issue, the authors use *k<sup>th</sup>* power of the graph to increase graph connectivity. In their work they employ *k=2*.

## Our two cents

- The novel technique for pooling operation introduced while leveraging the already deduced power of U-Net architecture undoubtedly is able to produce some pretty good results for graph classification problem.

- To be able to properly generalize to image data, the usage of **Graph Connectivity Augmentation using Graph Power** is of paramount importance for this pooling operation as it leads to forming of a connection between the remaining nodes and not just the connections after the simple gPool operation.

## Implementation

- [https://github.com/HongyangGao/gunet](https://github.com/HongyangGao/gunet)