# PAPER_TITLE: Feature Fusion Attention Network for Single Image Dehazing
 Authors:Qin, Xu and Wang, Zhilin and Bai, Yuanchao and Xie, Xiaodong and Jia, Huizhu
YEAR_OF_PUBLICATION=2020
Publication=AAAI_2020


## Summary:
This paper presents the Feature Fusion Attention Network (FFA-Net) for dehazing a single image, a crucial task in computer vision applications. FFA-Net consists of three essential elements: a Feature Attention (FA) module that combines Channel Attention and Pixel Attention, a basic block structure that incorporates Local Residual Learning and Feature Attention, and multi-layer feature fusion that adaptively learns feature weights to improve information preservation. Quantitatively and qualitatively, the accuracy demonstrated by the experimental results surpasses that of previous methods. FFA-Net demonstrates a novel approach to information integration, with applications that extend beyond dehazing.


## Contributions :

- Introduction of FFA-Net, a new single image dehazing technique.
- Channel Attention and Pixel Attention integration within the Feature Attention module.
- Utilization of a fundamental block structure to improve the concentration of information.
- Adaptive learning of feature weights for multi-layer feature fusion.
- Significant accuracy enhancements relative to existing methodologies.

## The method:
The FFA-Net method consists of three fundamental components:
- Feature Attention (FA) Module: Combines Channel Attention and Pixel Attention mechanisms to effectively manage diverse forms of data.
- Include Local Residual Learning and Feature Attention to emphasize relevant information.
- Multi-layer Feature Fusion: Learns adaptive feature weights, preserving shallow layer information and transferring it to deeper layers for enhanced fusion efficiency.

## Results:

- Utilizing Peak-Signal-to-Noise-Ratio (PSNR) and Structural Similarity Index (SSIM) to evaluate performance.
- Quantitatively and qualitatively, FFA-Net outperforms previous techniques.
> On the SOTS indoor test dataset, the PSNR metric improved from 30.23 to 36.39 dB.

- Enhanced precision and quality of vision in dehazed images.
- FFA-Net outperforms contemporary single image dehazing techniques.
- PSNR and SSIM metric enhancements relative to extant baselines.
- The demonstration of qualitative superiority through visual comparisons.

## Two cents:
> This paper provides a compelling solution to the difficult problem of single image dehazing. The innovative combination of Channel Attention, Pixel Attention, and feature fusion techniques in FFA-Net results in significant accuracy enhancements. In addition, the adaptability of FFA-Net's feature fusion mechanism allows for applications beyond dehazing, including denoising, deraining, and super-resolution. Future research could investigate the scalability and generalizability of FFA-Net in order to apply it to a broader spectrum of low-level vision tasks.







