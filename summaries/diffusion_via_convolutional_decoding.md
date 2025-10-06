# Fast and Fluent Diffusion Language Models via Convolutional Decoding and Rejective Fine-tuning


Yeongbin Seo, Dongha Lee, Jaehyung Kim, Jinyoung Yeo, ICML 2025 (arXiv preprint: 2509.15188v1) 
## Summary

The paper tackles a critical challenge in Diffusion-based Language Models (LMs): the Long Decoding-Window (LDW) problem. Since diffusion LMs decode multiple tokens in parallel across a fixed window, tokens far from the input context often become irrelevant or repetitive, hurting fluency and coherence. While prior solutions like semi-autoregressive (semi-AR) decoding address the LDW problem by dividing the window, they sacrifice decoding speed and the inherent bidirectionality of diffusion models.

To overcome these limitations, the authors introduce two novel methods:

**Convolutional decoding (Conv):** A normalization-based technique that smoothly narrows the decoding window without hard segmentation, preserving speed and flexibility.

**Rejecting Rule-based Fine-Tuning (R2FT):** A post-hoc training scheme that directly mitigates the model's preference for repetitive and high-prior tokens.
The combination of Conv and R2FT achieves state-of-the-art performance among diffusion LM baselines on open-ended generation tasks, even with a significantly smaller step size, demonstrating major improvements in both speed and quality.
## Contributions

Defines the Long Decoding-Window (LDW) problem as a core bottleneck in fluent text generation for diffusion LMs.
Identifies the time-interval expansion problem in the prior semi-AR solution, showing that it severely limits speedup due to degradation in text quality at small step sizes.
Proposes Convolutional decoding (Conv), a normalization-based method to narrow the decoding window that bypasses the limitations of semi-AR while retaining speed and bidirectionality.
Introduces Rejecting Rule-based Fine-Tuning (R2FT), a post-hoc training objective that effectively suppresses the model's preference for repetitive and high-prior tokens without harming language capability.
Achieves state-of-the-art performance among diffusion LM baselines on open-ended answer generation tasks (e.g., AlpacaEval) with up to one-third the step size of previous work, confirming significant improvements in fluency, coherence, and speed.

## Method
The Long Decoding-Window Problem
In Masked Diffusion Language Models (MDLMs), a fixed-size decoding window (L=1024) is treated as candidates for unmasking at every step. Tokens predicted at positions far from the input context tend to be irrelevant and random, manifesting as repetition of context or high-prior functional words (e.g., "the", "is"), which dominate the meaningful tokens in top-ranked candidates.

1. Convolutional Decoding (Conv)

**Mechanism:** Conv narrows the effective decoding window using a normalization mechanism rather than rigidly dividing it into blocks like semi-AR. The transformation is applied to the probability of a token 

<img width="650" height="99" alt="Screenshot 2025-10-06 at 6 43 32 PM" src="https://github.com/user-attachments/assets/a612cc55-c5af-4e61-b31b-fc88c38daf7c" />

**Advantage:** By applying normalization instead of fixed blocks, Conv avoids the time-interval expansion problem identified in semi-AR, allowing the model to maintain generation quality even at small kernel sizes, leading to a much more stable and robust speedup.

**Objective**: R2FT is an additional, short training stage (after standard SFT) that leverages a Direct Preference Optimization (DPO)-like loss to reject unwanted generation patterns. It trains the model to prefer the good samples from the standard dataset over their rule-based corrupted versions which are synthetically created to contain repetition patterns

<img width="736" height="119" alt="Screenshot 2025-10-06 at 6 41 08 PM" src="https://github.com/user-attachments/assets/389ef074-d753-4416-86dd-82325f1aaedc" />

**Effect:** This targeted training effectively reduces the model's preference for both repetition and high-prior tokens, causing the context-aligned "meaning" tokens to shift to higher ranks, which in turn enables highly deterministic decoding strategies like Top-k sampling to produce coherent text

## Results
The methods are evaluated primarily on open-ended generation benchmarks like AlpacaEval using the G-Eval metric, which aligns closely with human judgment. The standard setting for all MDLM baselines is L=1024 and a highly compressed step size S=128 to demonstrate real-world speed advantages.

**Superior Quality and Speed:** The combination of R2FT and Conv achieves the highest performance across all scales and benchmarks (AlpacaEval, MT-Bench, Wiki). For the small model, the combination (46.92% win rate) significantly outperforms the categorical baseline (32.16%).

**Efficiency:** The proposed methods with S=128 achieve comparable or better performance than the semi-AR baseline with S=1024, demonstrating a significant speed advantage. The use of EOS-fill further accelerates decoding, achieving approximately 3× faster decoding speed (tokens per step) compared to AutoRegressive models.

**Semi-AR Limitation Confirmed:** Ablation studies show that Conv is significantly more robust than semi-AR, which experiences a sharp degradation in performance as its block size (stride) decreases (a manifestation of the time-interval expansion problem).

## Two-Cents
This paper provides a thorough and convincing analysis of the Long Decoding-Window problem, which is arguably the most significant barrier to the fluency and coherence of parallel-decoding diffusion LMs. By formally defining the LDW problem and meticulously exposing the inherent limitations of the prior semi-AR approach, the authors establish a clear need for a better solution. Convolutional Decoding offers an elegant, stable, and theoretically justified alternative to block-based segmentation, while R2FT provides a smart, low-cost method to clean up the model's output distribution. The resulting performance gains in speed and open-ended quality demonstrate that diffusion LMs are a highly competitive, fast alternative to traditional autoregressive models. Future work should focus on leveraging Conv's preserved 
bidirectionality for tasks like goal-oriented dialogue, where this capability provides a distinct advantage.

## Resources
(https://arxiv.org/html/2509.15188v1)
