# LLM-Pruner: On the Structural Pruning of Large Language Models

Xinyin Ma, Gongfan Fang, Xinchao Wang **NeurIPS 2023**

## Summary

This paper introduces a method named "LLM-Pruner", this aims to ensure LLM's retain their multi-task solving and language generation abilities even after compression. This is done via removing coupled structures( based on connections between neurons) using gradient information, then the pruned LLM is trained on a limited dataset to help recover it's original abilities.  

## Contributions

The main contributions of this paper are:

* Introduces a dependency-aware pruning strategy that automatically groups and prunes coupled structures to preserve functionality

* Proposes a two-level importance estimation method (vector- and element-wise) using first- and second-order gradients

* Implements a three-stage pruning pipeline: Discovery (dependency detection), Estimation (group scoring), and Recovery (LoRA-based fine-tuning)

* Achieves fast and low-cost pruning: ~3 hours using only 50k public samples

* Does not require manual architecture-specific design or labeled data
 
## Method

1. **Discovery Stage**: This step focuses on identifying groups of interdependent structures within the LLM, this ensures coupled structures are pruned in unison, as partial pruning leads to increase in parameter size and misaligned representations

    <p align="center">
        <img src='../images/Discovery_stage.png' width="480" height="270">
    </p>

    Considering any neuron within the LLM as the initial trigger, it possesses the capability to activate neurons that depend on it. Subsequently, these newly triggered neurons can serve as the subsequent triggers to identify the dependency and activate their respective dependent neurons. This iterative process continues until no new neurons are detected.

2. **Estimation Stage**: In this stage, we estimate the importance of each group, to assign such a score to each group, the model is given access to a limited external dataset. Later, all groups are ranked according to their importance and pruned as per a pre-defined pruning ratio, group importance can be computed via two methods:
  * **Vector-wise Importance**:

    $L(W_i) = \Delta \mathcal{L}(\mathcal{D}) = \mathcal{L}(W_i)(\mathcal{D}) - \mathcal{L}(W_i = 0)(\mathcal{D}) = \left[ \frac{\partial \mathcal{L}}{\partial {\top}(\mathcal{D})}{\partial W_i} - \frac{1}{2} W_i^{{\top}} H W_i + \mathcal{O}(|W_i|^3) \right]$

    Note: A group is represented as $\mathcal{G} = \{W_i\}_{i=1}^{M}$, where $M$ is the number of coupled structures in one group and $W_i$ is the weight for each structure.

    where $H$ is the Hessian matrix. Here, $\mathcal{L}$ represents the next-token prediction loss. The first term is typically neglected as the model has converged on the training dataset, where $\frac{\partial \mathcal{L}^{\top}}{\partial W_i} \approx 0$. However, since $\mathcal{D}$(limited external dataset) here is not extracted from the original training data, which means that $\frac{\partial \mathcal{L}^{\top}}{\partial W_i} \not\approx 0$. This presents a desirable property for determining the importance since computation of second term is impractical due to complexity.

* **Element-wise Importance**: If we want an even finer and precise ranking we can opt for computing the importance of each element of $W_i$, instead of the appromixation we derived above. The mathematical formulation for this is-

    $L(W_i^k) = \mathcal{L}(W_i^k)(\mathcal{D}) - \mathcal{L}(W_i^k = 0)(\mathcal{D}) \approx \left[ \frac{\partial \mathcal{L}(\mathcal{D})}{\partial W_i^k} W_i^k - \frac{1}{2} \sum_{j=1}^{N} \left( \frac{\partial \mathcal{L}(\mathcal{D}_j)}{\partial W_i^k} W_i^k \right)^2 + \mathcal{O}(|W_i^k|^3) \right]$

    After the above computations, group importance can be calculated by any of the following four operations
    1. Summation
    2. Production (Product of all elements)
    3. Max
    4. Last Only (Assigning importance score of last executing structure in group as group importance)


* **Recover Stage**: Since we are working limited data while trying to recover original performance of model, it is necessary to minimize number of learnable parameters. To perform this LoRa is employed in the post-training process, the mathematical formulation of this method ensures no extra parameters are introduced in this process. 

## Results

<p align="center">
    <img src='../images/Pruner_1.png' height="300" width="400">
</p>

* Tested on: LLaMA-7B, Vicuna-7B, ChatGLM-6B

* Benchmarks: 7 zero-shot classification datasets + WikiText2/PTB for language modeling

* 20% pruning retains ~95% zero-shot performance after LoRA tuning

* Yields ~20% memory and ~15% latency reduction

* Outperforms baseline methods: random, L2 norm, and channel-only pruning

* Best results from second-order importance + block pruning (MLP + attention heads)

* Dependency grouping is crucial — without it, performance degrades significantly

* Pruning beyond 50% leads to notable performance loss

* Pruned models may show occasional repetitive or incoherent outputs

<p align="center">
<img src='../images/Pruner_2.png' height="150" width="400">
</p>

## Two-Cents

LLM-Pruner presents an efficient, task-agnostic pruning method for large language models that preserves performance with minimal data and compute, making it highly practical. However even with these advancements there is scope for further improvement like employing low-rank hessian approximations, adaptive pruning across layers as uniform pruning might lead to significant compression to critical layers.

## Resources

[Research Paper](https://arxiv.org/abs/2305.11627)

[Seminar](https://www.youtube.com/watch?v=S7og1mTFImw)

[Codebase](https://github.com/horseee/LLM-Pruner)