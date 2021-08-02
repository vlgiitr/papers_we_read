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

[![image](https://user-images.githubusercontent.com/76916164/127901570-e6dc6414-675a-4b39-8ebf-975ac620a2f5.png)(https://github.com/Sandstorm831/papers_we_read/blob/master/images/DoodlerGAN%20architecture.png)


- The input sketch is represented by stacking channels for each part the sketch while a separate channel for the entire sketch. 
- It follows the Discriminator architecture from the styleGAN2 architecture. Discriminator is given access to the input sketch and the corresponding part channels so that it can distinguish between real and fake parts corresponding to both the shape and position of the parts relative to other parts, whereas the input to the discriminator is an image with (no. Of parts + 1) channels. 
- For part selector, similar architecture as the encoder, just a linear layer added at the end. 

[![image](https://user-images.githubusercontent.com/76916164/127901684-109844f1-3f08-460e-8169-e21faa89bd00.png)](https://github.com/Sandstorm831/papers_we_read/blob/master/images/Output%20images.png)

### Conclusion
Although, sketches by DoodlerGAN are quite creative and diversified, it can surpass human test in case of creative birds but not in case of creative creatures. 

[![image](https://user-images.githubusercontent.com/76916164/127901759-187eeafb-0626-47e1-adda-42dfe79cb0c2.png)](https://github.com/Sandstorm831/papers_we_read/blob/master/images/Conclusion.png)

Thus, leaving room for the improvement and scope in generating more complex creative sketches. 

