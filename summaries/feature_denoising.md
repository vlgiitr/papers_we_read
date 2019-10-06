
# Feature Denoising for Improving Adversarial Robustness

Cihang Xie, Yuxin Wu, Laurens van der Maaten, Alan Yuille, kaiming He, CVPR-2019

## Summary

This paper proposes a new technique to achieve adversarial robustness for a Deep ConvNet by leveraging the fact that adversarial perturbation lead to noise in the features extracted by these networks. So their network contains noise blocks for denoising feature maps and hence making the network classify the input correctly.

## Main contributions

- The main contribution of the paper is their feature denoising module that has been used to denoise the feature map obtained when passing a perturbed/normal image through the network.

- The concept that the authors leverage for adversarial robustness is that the activation maps of adversarial examples are very different from the ones obtained by the normal inputs in the sense that they are noisy or more precisely assign weights to parts of images not required for classification or our main purpose.

- So to fix this, a denoising block is added to the network after one of the layers to get activations close to normal activations and hence make the decision process by the network accurate.

<img src='https://github.com/vlgiitr/papers_we_read/blob/master/images/feature_denoising.png'>

- As can be seen from the figure, the major component of this block is the denoising operation which denoises the image while 1\*1 conv and the residual connection are mainly for feature combination. The parameters for the denoising block can be learned in an end-to-end fashion.

- The various Denoising operations they have used in the paper are as follows:
	- **Non-Local Means**: Computes denoised feature map of an input feature map by taking a weighted mean of features in all spatial locations. The various ways to select the weighting function:
		- Gaussian Softmax: Kind of softmax-based self-attention computation of the features
		- Dot Product: Simple dot product between features to get the weighring function. Provides denoising operation with no extra parameters.
	- **Bilateral filter**: Computes denoised feature map of an input feature map by taking a weighted mean of features in a predefined neighborhood. The various ways to calculate the weighting function:
		- Gaussian Softmax and Dot product are same as before.
		- Mean filter: Computation of simple mean of the features in the neighborhood window defined.
		- Median filter: Computation of simple median of the features in the neighborhood window defined.

## Implementation Details

- Usage of PGD attacker to get adversarial examples to train the denoising block.

- Choosing their baselines as ResNet-101/152, they have added 4 denoising blocks to a ResNet, each added after last residual block of res<sub>2</sub>, res<sub>3</sub>, res<sub>4</sub> and res<sub>5</sub> respectively.

## Our two cents

- The notion of introducing the denoising block in the network has leveraged one of the basic concepts of adversarial examples and has given pretty competitve results.

- Also the fact that the denoising block can be learned in an end-to-end fashion means that the modified network can also be used for simple image training, with little to no change in accuracy, as has been stated by the authors as well.

- However, the work can still be extended further with the usage of even more complex denoising blocks, with operations not just directly on the feature maps but some kind of higher level representations of those, trainable in an end-to-end fashion in the same way. 

## Implementation

- [https://github.com/facebookresearch/ImageNet-Adversarial-Training](https://github.com/facebookresearch/ImageNet-Adversarial-Training)