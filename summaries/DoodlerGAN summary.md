# CREATIVE SKETCH GENERATION
Songwei Ge, Vedanuj Goswami & C. Lawre1113ce Zitnick, Devi Parikh, ICLR 2021
### Summary
In this paper DoodlerGAN is proposed, which is a part-based Generative Adversarial Network (GAN). SketchRNN was the early approach to this direction with a a sequence-to-sequence Variational Autoencoder (VAE) later approaches incorporated a convolutional encoder to capture the spatial layout of sketches.  SketchGAN adopted a conditional GAN model as the backbone to generate the missing part. 
### Main points of the paper
- DoodlerGAN is a part-based GAN which draws any doodle part by part instead of drawing as whole. 
- DoodlerGAN instead of mimicking human drawings, it is used for sketches that are not seen in human drawings.
- DoodlerGAN basically consists of 2 parts
    - Part Selector module 
    - Part Generator module 
- Part Selector predicts which part to be drawn next whereas Part Generator draws the raster image of the part. 
- Part generator is a Conditional GAN based upon the styleGAN2 Architecture. It uses a 5-layer styleGAN2 generator with [512,256,128,64,32] output channels starting from 4x4x64 constant feature map. For encoding the input partial sketches, it uses a 5-layer CNN with [16, 32, 64, 128, 256] output channels. It downsample the intermediate feature map after each encoder layer and concatenate it channel-wise with the corresponding layers in the generator, giving it access to the hierarchical spatial structure of the input sketch. 
![image](https://user-images.githubusercontent.com/76916164/127711080-e1b9e359-610e-4a5b-ac8f-ccaf930f5598.png)
- The input sketch is represented by stacking channels for each part the sketch while a separate channel for the entire sketch. 
- It follows the Discriminator architecture from the styleGAN2 architecture. Discriminator is given access to the input sketch and the corresponding part channels so that it can distinguish between real and fake parts corresponding to both the shape and position of the parts relative to other parts, whereas the input to the discriminator is an image with (no. Of parts + 1) channels. 
- For part selector, similar architecture as the encoder, just a linear layer added at the end. 
![image](https://user-images.githubusercontent.com/76916164/127711194-9c161b6b-d64f-4783-bab0-5275cd917583.png)

### Conclusion
Although, sketches by DoodlerGAN are quite creative and diversified, it can surpass human test in case of creative birds but not in case of creative creatures. 
![image](https://user-images.githubusercontent.com/76916164/127711277-a0d4a2b6-2e37-4d88-a5c8-fbb7239d2e99.png)
Thus, leaving room for the improvement and scope in generating more complex creative sketches. 

