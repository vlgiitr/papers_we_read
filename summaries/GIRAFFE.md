# GIRAFFE: Representing Scenes as Compositional Generative Neural Feature Fields
Michael Niemeyer, Andreas Geiger

## Summary
![alt text](https://github.com/kumar-devesh/papers_we_read/blob/master/images/giraffe_overview.PNG)

The paper presents a **generative neural feature fields** based approch for representing scenes as a composition 
of multiple objects and a background which allows better disentanglement of scene properties like object shapes, 
appearences and camera pose without any posed images or any other supervision. The model is able to disentangle 
individual objects and allows rotation and translation of the individual objects as well as change in camera pose.

## Contributions

- A novel method for generating scenes in a controllable and photorealistic manner while training from unposed images.
- Incorporating a compositional 3D scene representation directly into the generative model to allow more controllable 
image synthesis and using neural rendering for contiuous scene representation for more realistic images.

## Model

![alt text](https://github.com/kumar-devesh/papers_we_read/blob/master/images/giraffe_architecture.PNG)

The model builds on the GRAF model where a scene was modeled as a neural feature field conditioned on
two latent vectors the **appearance code zₐ** and the **shape code zₛ** sampled randomly from a distribution. 

In the GIRAFFE model, for each object seperate latent codes are sampled and a neural feature field is conditioned on
these latent codes along with an affine transfomation using matrices from *the object space to the scene space*. The 
composite scene is obtained from the densities and feature vectors sampled for all the objects as a weighted average 
of the features according to their densites and the volumetic density is the sum of the densities for all objects
for a given point and the viewing direction. 
![alt text](https://github.com/kumar-devesh/papers_we_read/blob/master/images/giraffe_composition_scene_feature_field.PNG) 

Volume rendering for a sampled camera pose **ε** for the feature field gives the feature image which is used to obtain the final image using a CNN renderer.

## Results
![alt text](https://github.com/kumar-devesh/papers_we_read/blob/master/images/giraffe_controllable_scene_generation.PNG) 

## Our Two Cents
- The paper presents a novel idea for controllable image synthesis by disentangling individual objects from the 
background as well as their shape, appearance and pose without explicit supervision.
- The model scales well on adding multiple objects in a scene at test time inspite of being trained on data with a single object in an image.

## Resources
- Paper Link 
https://openaccess.thecvf.com/content/CVPR2021/html/Niemeyer_GIRAFFE_Representing_Scenes_As_Compositional_Generative_Neural_Feature_Fields_CVPR_2021_paper.html
