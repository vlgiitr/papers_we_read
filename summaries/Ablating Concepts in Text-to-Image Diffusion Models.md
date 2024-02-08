# Ablating Concepts in Text-to-Image Diffusion Models
Nupur Kumari et. al , **ICCV** **2023**

## Summary
The paper introduces a method for modifying pre trained text-to-image diffusion models to remove certain undesired concepts while preserving related ones to ablate (remove) undesired concepts such as copyrighted material, memorized images, or specific art styles.

## Contributions 

1. Addressing the challenge of efficiently preventing a diffusion model from generating specific concepts without retraining from scratch: The paper presents two strategies for aligning the target concept distribution with an anchor concept's distribution. The method is demonstrated to be highly efficient, taking only about five minutes per concept for model updates on NVIDIA A100 GPU.

2. Ethical Control in Generative AI: A notable contribution is the introduction of a new avenue for controlling generative AI output ethically. By allowing for the modification of specific concepts while preserving related ones, the research enhances ethical governance over diffusion model outputs.

## Method 

The primary challenge addressed by the paper is efficiently preventing a diffusion model from generating specific concepts without retraining from scratch or losing related concepts. This is tackled by aligning the target concept image distribution - which is to be ablated - with the distribution of an anchor concept.

They assume that the user provides the desired anchor concept, e.g., Cat for Grumpy Cat. The anchor concept overwrites the target concept and should be a superset or similar to the target concept. Thus, given a set of text prompts ${c^ {*} }$ describing the target concept, they aim to match the following two distributions via Kullback–Leibler (KL) divergence:

$$ \begin{aligned}
\arg \min_{\hat{\Phi}} D_{\text{KL}}(p(x(0...T)|c) || p_{\hat{\Phi}}(x(0...T)|c^*)),
\end{aligned} $$

where $p(x_{(0...T)}|c)$ is some target distribution on the $x_t, t \in [0, T]$, defined by the anchor concept c, and $p_{\hat{\Phi}}(x_{(0...T)}|c^{ * })$ is the model’s distribution for the target concept. 

## Results 
Extensive experiments validate the effectiveness of the proposed method across 16 ablation tasks, showing strong numerical evidence of the ability to obviate target concepts – which included specific objects, styles, and memorized images. 
![image](https://github.com/shivank21/1y-recruitment-vlg/assets/128126577/523f4b03-ade3-4ecd-b09e-341f7130c86b)
![image](https://github.com/shivank21/1y-recruitment-vlg/assets/128126577/8e553897-8fdd-4f62-b72b-21eef9bd1fa9)

## Two Cents 
The paper's contribution signifies a notable advancement in the field of generative AI, enabling enhanced control and ethical oversight over the outputs of diffusion models. It is noteworthy that certain instances demonstrate a deterioration in the outputs of analogous concepts following ablation, suggesting opportunities for further research in this area.
