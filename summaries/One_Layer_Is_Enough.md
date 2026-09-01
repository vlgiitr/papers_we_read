# One Layer Is Enough: Adapting Pretrained Visual Encoders for Image Generation

Yuan Gao, Chen Chen, Tianrong Chen, Jiatao Gu, CVPR 2026

## **Summary**
Pretrained visual backbones like DINOv2 and SigLIP offer rich semantic features, but their high-dimensional outputs make latent diffusion models unstable and slow to train. The Feature Autoencoder (FAE) resolves this issue by using a single self-attention layer to compress pretrained embeddings into compact latents. It uses an architecture such that the original feature space is restored before decoding it into RGB pixels.

## **Contributions**

* Shows that a single self-attention layer followed by a linear projection effectively maps frozen encoder outputs into a low-dimensional generative latent space without overfitting.
* Proposes a decoupled decoding pipeline separating semantic feature reconstruction from pixel synthesis, so the generator never has to learn raw pixel rendering directly.
* Retains the deep semantic representation of frozen backbones while keeping the downstream diffusion network lightweight and low-dimensional.
* Achieves competitive ImageNet and MS-COCO FID scores in significantly fewer training epochs compared to direct-modeling and feature-alignment baselines.

## **Method**

<img src="../images/fae_stages.png" alt="Diagram showing FAE training stages" width="500">
Self-supervised vision encoders produce high-dimensional patch embeddings (e.g., 1,536 dimensions for DINOv2-g) to preserve high-level semantics. Generative diffusion models require low-dimensional latents (typically 4–64 dimensions) because tracking high-dimensional noise across iterative denoising steps is unstable. FAE resolves this trade-off using a two-phase architecture:

* **Single-Attention Encoder:** A shallow adapter with one self-attention layer (enables inter-patch communication to remove redundant global information) and a linear projection mapping patch embedding $x \to z$. Keeping capacity minimal prevents the adapter from overfitting to pixel-level reconstruction at the expense of general semantics.
* **Feature Decoder:** A 6-layer Transformer that reconstructs features $\hat{x}$ from $z$, trained via a VAE objective:

$$\mathcal{L}_{VAE} = \Vert{} \hat{x} - x \Vert{}_2^2 + \beta\, \text{KL}\big(q(z \mid x) \,\Vert{}\, p(z)\big)$$


* **Pixel Decoder:** A network that maps reconstructed features $\hat{x}$ to RGB pixels using adversarial, perceptual, and reconstruction losses.
* **Generative Engine:** A standard diffusion or flow model (e.g., SiT, STARFlow) trained directly on clean latents $z$. At generation time: sample noise $\to$ denoise to clean $z \to$ feature decoder $\to \hat{x} \to$ pixel decoder $\to$ final image.

## **Results**
<img src="../images/fae_compare.png" alt="Diagram comparing SD-VAE, VA-VAE, RAE, and FAE" width="500">

* **Generation Performance:** Reaches competitive FID scores on ImageNet and MS-COCO with a fraction of the training compute required by traditional VAEs.
* **Versus Feature Alignment (REPA, VA-VAE):** Avoids complex multi-stage training overhead and alignment loss tuning while retaining backbone representations.
* **Versus Direct Modeling (RAE):** Avoids forcing the generative backbone to expand channel width or add extra heads to process massive uncompressed vectors.
* **Limitations:** Reconstruction FID slightly lags behind pixel-first methods like VA-VAE, trading a small degree of fine-grained pixel detail for structural simplicity and lower compute cost.

## **Two-Cents**
Using just one attention layer works surprisingly well because it keeps the original features intact without ruining them. Separating feature recovery from pixel painting saves a lot of compute time for only a tiny loss in fine detail.
* Paper: [https://arxiv.org/abs/2512.07829](https://arxiv.org/abs/2512.07829)
* Apple ML Research: [https://machinelearning.apple.com/research/adapting-pretrained-visual-encoders](https://machinelearning.apple.com/research/adapting-pretrained-visual-encoders)
