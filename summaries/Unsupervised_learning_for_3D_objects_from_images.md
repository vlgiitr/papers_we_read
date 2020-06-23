## Unsupervised Learning of Probably Symmetric Deformable 3D Objects from Images in the Wild

Shangzhe Wu, Christian Rupprecht, Andrea Vedaldi, **CVPR 2020**

## Summary

The paper proposes a method to learn 3D deformable object categories from raw single-view images, **without external supervision**. The method is based on an autoencoder that factors each input image into **depth, albedo, viewpoint and illumination**. In order to disentangle these components without supervision, authors use fact that many object categories have, at least in principle, a symmetric structure. They show that reasoning about illumination allows us to exploit the underlying object symmetry even if the appearance is not symmetric due to shading. Furthermore, they model objects that are probably, but not certainly, symmetric by predicting a symmetry probability map, learned end-to-end with the other components of the model.

<img src='../images/unsupervised_deformable_3D_summary.png'>

This paper received the **Best Paper Award** at CVPR 2020. 

## Assumptions

- For decomposing images into depth, albedo, viewpoint and illumination, we note that many object categories are symmetric. Assuming an object is perfectly symmetric, one can obtain a virtual second view of it by simply mirroring the image.

- However, specific object instances are in practice never fully symmetric, neither in shape nor appearance. To counter this issue: 
    - First, authors explicitly model illumination to exploit the underlying symmetry and show that, by doing so, the model can exploit illumination as an additional cue for recovering the shape. 
    - Second, they augment the model to reason about potential lack of symmetry in the objects. To do this, the model predicts, along with the other factors, a dense map containing the probability that a given pixel has a symmetric counterpart in the image.

## Methodology    

- Given an unconstrained collection of images of an object category, such as human faces, our goal is to learn a model Φ that receives as input an image of an object instance and produces as output a decomposition of it into 3D shape, albedo, illumination and viewpoint. 

- In order to learn such a decomposition without supervision for any of the components, we use the fact that many object categories are bilaterally symmetric. 

- However, as stated before, the appearance of object instances is never perfectly symmetric. We take two measures to account for these asymmetries: explicitly modelling asymmetric illumination and estimating a confidence score that explains the probability of the pixel having a symmetric counterpart in the image, for each pixel in the input image. (see conf σ, σ' in Fig. 2).

### Photo-Geometric Autoencoding

An image *I* is a function *Ω → R<sup>3</sup>* defined on a grid *Ω = {0, . . . , W − 1} × {0, . . . , H − 1}*, or, equivalently, a tensor in *R<sup>3×W×H</sup>*. We assume that the image is roughly centered on an instance of the object of interest. The goal is to learn a function *Φ*, implemented as a neural network, that maps the image I to four factors *(d, a, w, l)* comprising a depth map *d : Ω → R<sub>+</sub>* , an albedo image *a : Ω → R<sup>3</sup>*, a global light direction *l ∈ S<sup>2</sup>*, and a viewpoint *w ∈ R<sup>6</sup>* so that the image can be reconstructed from them.

The image I is reconstructed from the four factors in two
steps, lighting Λ and reprojection Π, as follows:
        
        Î = Π (Λ(a, d, l), d, w) -(1)

The lighting function Λ generates a version of the object based on the depth map *d*, the light direction *l* and the albedo *a* as seen from a canonical viewpoint *w = 0*. The viewpoint *w* represents the transformation between the canonical view and the viewpoint of the actual input image *I*. Then, the reprojection function *Π8 simulates the effect of a viewpoint change and generates the image *Î* given the canonical depth *d* and the shaded canonical image *Λ(a, d, l)*. Learning uses a reconstruction loss which encourages *I ≈ Î*.

<img src='../images/unsupervised_deformable_3D.png'>

### Probably Symmetric Objects

Leveraging symmetry for 3D reconstruction requires identifying symmetric object points in an image. Here we do so implicitly, assuming that depth and albedo, which are reconstructed in a canonical frame, are symmetric about a fixed vertical plane. An important beneficial side effect of this choice is that it helps the model discover a ‘canonical view’ for the object, which is important for reconstruction. To do this, we consider the operator that flips a map *a ∈ R<sup>C×W×H</sup>* along the horizontal axis: *[flip a]<sub>c,u,v</sub>* = *a<sub>c,W −1−u,v</sub>*. We then require *d ≈ flip d'* and *a ≈ flip a'*. While these constraints could be enforced by adding corresponding loss terms to the learning objective, they would be difficult to balance. Instead, we achieve the same effect indirectly, by obtaining a second reconstruction *I'* by replacing *a* and *d* in `Eq. (1)` with *flip a* and *flip d* respectively.

<img src='../images/unsupervised_deformable_3D_loss.png'>

Modelling uncertainty is generally useful, but in our case is particularly important when we consider the “symmetric” reconstruction *Î*, for which we use the same loss *L( Î', I, σ')*. Crucially, we use the network to estimate, also from the same input image *I*, a second confidence map *σ'* . This confidence map allows the model to learn which portions of the input image might not be symmetric.

Overall, the learning objective is given by the combination of the two reconstruction errors:
        
        E(Φ; I) = L( Î, I, σ) + λ*L( Î', I, σ') ; where λ = 0.5. - (4)

### Image Formation Model

The image is formed by a camera looking at a 3D object. If we denote with *P* = *(P<sub>x</sub> ,P<sub>y</sub> ,P<sub>z</sub>) ∈ R<sup>3</sup>* a 3D point expressed in the reference frame of the camera, this is mapped to pixel *p* = *(u, v, 1)* (for mathematical details about how this transformation happens, look at the paper).

### Perceptual Loss

The *L<sub>1</sub>* loss function Eq. (3) is sensitive to small geometric imperfections and tends to result in blurry reconstructions. We add a perceptual loss term to mitigate this problem:

<img src='../images/unsupervised_deformable_3D_perceptual_loss.png'>

With this, the loss function *L* in `Eq. (4)` is replaced by *L + λ<sub>p</sub>L<sub>p</sub>* with *λ<sub>p</sub>* = 1.

## Main Contributions

- The paper presented a method that can learn a 3D model of a deformable object category from an unconstrained collection of single-view images of the object category.

- The model is able to obtain high-fidelity monocular 3D reconstructions of individual object instances.

- Introduces **Photo-Geometric Autoencoding**: an autoencoding pipeline where the factors have, due to the way they are recomposed, an explicit photo-geometric meaning.

## Our two cents

Currently, the model works mostly for images which are symmetric to a high extent. It would be interesting to see how the future work tackles the asymmetries in extreme images with the help of additional constraints or innovations.

## Resources

- [Visual Geometry Group blog (You can try out the model on different images here!)](http://www.robots.ox.ac.uk/~vgg/blog/unsupervised-learning-of-probably-symmetric-deformable-3d-objects-from-images-in-the-wild.html?image=021_paint&type=human)

- [Project website](https://elliottwu.com/projects/unsup3d/)

- [Implementation](https://github.com/elliottwu/unsup3d)