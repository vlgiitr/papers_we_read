
# Unpaired Image-to-Image Translation using Cycle Consistent Adversarial Networks

Jun-Yan Zhu, Taesung Park, Phillip Isola, Alexei A. Efros, ICCV-2017

## Summary

This paper proposes an image-to-image translation mechanism without paired images from both the domains. Unkike its predecessor image translation techniques such as pix2pix it is able to do the task of image translation without paired labelled images by using cycle consistency loss.

## Main contributions

- The main contribution of this paper is the **cycle consistency loss** which the authors have used successfully in various domain transfer problems.

- The basic idea behind cycle consistency is enfore an image to translate properly to a new domain even with the absence of paired images. To accomplish the task, the author rather than using a single generator and discriminator network has rather used two generators and two discriminators.

- The first generator(G<sub>12</sub>) traslates the image from 1st domain to another while its corresponding discriminator(D<sub>1</sub>) acts a critic to differentiate from real and fake. The generated output from this is then passed through the second generator(G<sub>21</sub>) which then translates the image back to its original domain with its corresponding discriminator(D<sub>2</sub>). The same process is repeated but with passing the images from 2nd domain to G<sub>21</sub> while forming a complete cycle again

<img src='https://github.com/vlgiitr/papers_we_read/blob/master/images/cyclegan.png'>

- However even with the above process the network can easily just translate an image to another domain without caring about the information that makes it unique since there are no paired images. For example, in a horse to zebra transformation, with the above process we can easily translate a horse to a zebra but we may just not retain the information such as how the zebra is standing among many others if we follow this process.

- To rectify this, the authors introcuduced the *cycle consisteny loss* which is basically just comparing the initial image to the image obtained after passing it through the two networks(G<sub>12</sub> and G<sub>21</sub>) and adding that also as a loss in the final loss equation. So we get two cycle consistency losses in our final equation.

## Implementation Details

- They used the architecture from [Johnson et al.](https://github.com/jcjohnson/fast-neural-style)
- They used [LSGAN](https://arxiv.org/abs/1611.04076)(Least squares GAN) instead of normal ones as it seemed to stabilize their training process.
- To reduce model oscillation , they used [Shrivastava et al.’s strategy](https://arxiv.org/abs/1612.07828)

## Our two cents

- The major contribution of the paper was undoubtedly its introduction of cycle consistency loss for unpaired image-to-image translation. The cycle loss has been used extensively in literature for various different tasks including segmentation and various other domain adaptation problems. Some of the experiments done by the community by using principles of this paper have even been underlined in the experiments section of the paper.

- Although the technique has been able to achieve transformations of complex nature, it still has many limitations on the kind of transformations it can achieve, one of them being complex geometric transformation (eg. dog -> cat).

- Also some failure cases are caused when applying transformation on the corner cases of examples obtained from training data, which indicates a lack of power by the network to generalize.

## Implementation

- [https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix)
