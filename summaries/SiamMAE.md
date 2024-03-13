# Siamese Masked Autoencoders
 
**Authors:** Agrim Gupta , Jiajun Wu , Jia Deng , Li Fei-Fei
**NeurIPS 2023**

 ## Summary 

This paper introducs a neural network architecture called SiamMAE Network, which is a simple extention of Masked Autoencoders (MAE) for learning visual correspondence from videos. Here, asymmetric masking approach is explored, which encourages the network to model object motion, or in other words, to understand what *went where* 

## Contributions

- Asymmetric masking approach is introduced, it creates a challenging self-supervised learning task while encouraging the network to learn temporal correlations (in f1 (0%) and mask a very high ratio (95%) of patches in f2).
- This approach helps encourage the network to model object motion and focus on object boundaries.
- It outperforms many netowrks when it comes to the task of Video Object Segmentation, Video Part Segmentation and Pose Tracking without the need for data augmentation, handcrafted tracking-based pretext tasks, or other techniques to prevent representational collapse. 

## Methodology

- They utilize Self-supervised visual representation learning as a way to learn generalizable visual representations. Contrastive methods in the image domain encourage models to learn representations by modeling image similarity and dissimilarity or only similarity.
- Here, we use **Masked Autencoders**, which are a type of denoising autoencoder that learn representations by reconstructing the original input from corrupted (i.e., masked) inputs. They have had a transformative impact in masked language modeling and even learning image represntaions.
- And **Siamese Networks** which are weight sharing entites use to compare entities, these have extensively used in modern contrastive learning approaches.

#### Key Components 
- The siamese encoders are used to process the two frames independently and the asymmetric masking serves as an information bottleneck. Siamese networks have been used for correspondence learning and often require some information bottleneck to prevent the network from learning trivial solutions
- The output from the encoder is projected using a linear layer and MASK tokens with position embeddings are added to generate the full set of tokens corresponding to the input frame.
- To further increase the difficulty of the task, the two frames are sampled with a larger temporal gap and small number of patches as input for the second frame results in a challenging yet tractable self-supervised learning task. 

#### Working
<img src='../images/SiamMae-Archi.png'> 

## Results

<img src='../images/SiamMae_comparision.png'>

<img src='../images/SiamMae-Results.png'>

## Our Two Cents

- The paper introduces how asymmetric masking strategy is an effective strategy for learning correspondence, and considering it achieves these competitive results without the need for data augmentation, handcrafted tracking-based pretext tasks, or other techniques to prevent representational collapse. 
- Here, the network relies on learning correspondences by operating on pairs of video frames, so adapting it for multiple frames can be a challenge and looked into for its scalabilty. 

## Resources

Project Page: https://siam-mae-video.github.io/
