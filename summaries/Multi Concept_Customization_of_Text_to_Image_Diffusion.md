# Multi-Concept Customization of Text-to-Image Diffusion
Nupur Kumari,Bingliang Zhang,Richard Zhang,Eli Shechtman & Jun-Yan Zhu, **CVPR 2023**

## Summary 

While text-to-image diffusion models generally perform well, they face challenges with specific or nuanced concepts due to limited training data. The paper suggests a novel approach, introducing a method called custom diffusion to fine-tune pre-trained models. This enables the integration of new concepts with minimal resources and data.

![teaser](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/54447d1c-fd57-4625-88f3-9b109ca9cc34)


## Contributions 

- Demonstrates a quick and computationally efficient fine-tuning process that enables models to generate images of new concepts in detail.(Takes ~6 mins with 2 A100 GPU's)
- Introduces a method for the model to learn several new concepts at once and blend them together in generated images, making the model more useful for handling intricate scenes.
- Shows better performance to other methods in empirical evaluations
-  Introduces a new dataset of 101 concepts for evaluating model customization methods along with text prompts for single-concept and multi-concept compositions

## Method 

Given a set of target images, the method first retrieves (generates) regularization images with similar captions as target images. The final training dataset is union of target and regularization images. During fine-tuning the method update the key and value projection matrices of the cross-attention blocks in the diffusion model with the standard diffusion training loss. 
![methodology](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/4145d0a5-a5af-4ce5-9b04-2c617188488b)

Cross-attention block modifies the latent features of the network according to the condition features, i.e., text features in the case of text-to-image diffusion models.Given text features $c \in \mathbb{R}^{s \times d}$ and latent image features $f \in \mathbb{R}^{ (h\times w) \times l}$, a single-head cross-attention operation consists of $Q = W^q \textbf{f}, K = W^{k} \textbf{c}, V = W^{v} \textbf{c}$, and a weighted sum over value features as:

$$\begin{equation}
    \begin{aligned}
    \text{Attention}(Q, K, V) = \text{Softmax}\Big(\frac{QK^T}{\sqrt{d'}} \Big)V, \\
    \end{aligned}
\end{equation}$$

 where $W^q$, $W^k$, and $W^v$ map the inputs to a query, key, and value feature, respectively, and $d'$ is the output dimension of key and query features. The latent feature is then updated with the attention block output. The task of fine-tuning aims at updating the mapping from given text to image distribution, and the text features are only input to $W^k$ and $W^v$ projection matrix in the cross-attention block.Therefore, the authors propose to only update $W^{k}$ and $W^{v}$ parameters of the diffusion model during the fine-tuning process.

## Results 

### Single-Concept

![Tortoise](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/33a8f690-ab26-48c7-9e7b-ae6962a50cae)

Where, $V^∗$ is initialized with different rarely-occurring tokens and optimized along with cross-attention key and value
matrices for each layer during fine tuning.

### Multi-Concept

![Chair_Table](https://github.com/shivank21/vlg-recruitment-1y/assets/128126577/8a93e8c6-d05f-431c-af8f-2a54ff258f27)

## Two-Cents

The research takes a big step forward in tailoring models for turning text into images, but it also points to various possibilities ahead. One might consider expanding this approach to cover a broader range of creative tasks, like generating videos or even creating audio based on text descriptions.However, there are a couple of limitations. Tricky combinations, like having both a pet dog and a pet cat, still pose a challenge. Additionally, putting together three or more concepts becomes a  difficult task with this method.

## Resources 
- Webpage : https://www.cs.cmu.edu/~custom-diffusion/
- Paper https://arxiv.org/pdf/2212.04488.pdf


