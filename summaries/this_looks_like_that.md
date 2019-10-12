# This Looks Like That: Deep Learning for Interpretable Image Recognition
Chaofan Chen, Oscar Li, Chaofan Tao, Alina Jade Barnett, Jonathan Su, Cynthia Rudin

## Summary
This paper proposes a novel idea for interpretable deep learning , it basically figures out some protopyical parts of images by itself , and then uses these prototypes to make classification , hence making the classification process interpretable.
**Among the top 3% accepted papers of NIPS 2019**

### Architecture Details
- Intially we have the first 13 layers of the  VGG-16 network ,followed by 2 , 1x1 convs
- The network learns m **prototypes** (pj) , with a predefined number of prototypes for each class .
- Each prototpye is applied to all the patches of the conv output , and using the L2 distance , a **similarity score is produced for a single prototype for all the patches** , this can also be used to make a heatmap of similarity .
- Then global pooling is applied to convert this into a single score **gpj**.Which represents the strongest similar finding of that prototype in the image.
- Then the m global similarity scores are feeded in the FC layer to perform classification.
<img src='https://github.com/ayushtues/papers_we_read/blob/master/images/this_looks_like_that.png' style="max-width:100%">

### Overview
- The network basically learns from the training set, a limited number of **prototypical parts** that are useful in classifying a new image.
- The model is able to identify several parts of the image where it thinks that this identified part of the image looks like that prototypical part of some training image, and makes its prediction based on a weighted combination of the **similarity scores** between parts of the image and the learned prototypes.



- There are 3 training stages  
    1. SGD of the layers before the FC layer h 
    2. Projection of the prototypes onto the closest latent representations of training image patches from the same class as       that of the prototype. 
    3. Convex optimization of the last layer h. (Using Adam)
