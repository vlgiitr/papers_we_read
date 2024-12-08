# UNDERSTANDING THE EFFECTS OF RLHF ON LLM GENERALISATION AND DIVERSITY

Robert Kirk, Ishita Mediratta, Christoforos Nalmpantis, Jelena Luketina, Eric Hambro, Edward Grefenstette and Roberta Raileanu, **ICLR 2024**

## Summary 

The paper explores the impact of Reinforcement Learning from Human Feedback on Large Language Models in terms of two crucial aspects: their ability to generalize to new, unseen data (OOD generalization) and the diversity of their generated outputs compared to Supervised Fine-Tuning(SFT) and Best-of-N sampling(BoN).

## Resuls and Analysis

The paper focuses mainly on the trade-offs between model generalisation—how well an LLM adapts to new, unseen data distributions—and output diversity, defining the range of different outputs the model can generate.

### Generalisation:

- RLHF is shown to enhance both in-distribution(ID) and out-of-distribution (OOD) performance compared to SFT. This is notably observed in instruction following tasks with more considerable distribution shifts .

- When evaluating summarisation models, RLHF maintains superior performance in comparison to SFT across diverse test datasets. BoN notably outperforms RLHF in summarisation, although BoN incurs significantly higher inference costs.

### Diversity:

- RLHF significantly reduces per-input diversity, which could pose challenges in scenarios where diverse responses are crucial. However, across-input diversity is only slightly affected, indicating that while RLHF limits variation for individual prompts, it preserves some level of diversity across a range of inputs. This phenomenon is anaological to the problem of “mode collapse” often observed in reinforcement learning applications, where the model favors certain patterns at the expense of variety.

### Effect of KL-Penalty: 

- While RLHF enhances both in-distribution (ID) and out-of-distribution (OOD) performance, it significantly reduces output diversity compared to SFT. Given that the KL penalty coefficient is designed to keep the RLHF policy closer to the SFT policy, the authors explored whether adjusting this coefficient could balance generalization and diversity. However, the findings indicate that this approach is ineffective—increasing the KL penalty coefficient results in both reduced performance and lower per-input diversity, contrary to what is expected. This highlights the need for further research into more advanced methods to achieve a better balance between generalization and diversity.

**RLHF Equation:**
$$ R(x, y) = RM_{\theta_{RM}}(x, y) - \beta_{KL} D_{KL}(\pi_{\theta_{RL}}(y|x) \| \pi_{\theta_{SFT}}(y|x)) $$
Where : 
- RM: Reward Model
- $ \theta_{RL}, \theta_{RM}, \theta_{SFT} $: parameters of the RLHF, RM, and SFT models, respectively
- x: input
- y: output
- $\beta_{KL}$: weight of the KL penalty

**Per-Input Diversity:**
$$ \text{PerInputDiversity}_D(\pi) = \frac{1}{N} \sum_{i=1}^{N} D(O_i^\pi) $$
- D: Diversity Metric(eg N-grams, sentence-BERT cosine similarity ,NLI diversity)
- $ \pi $: Policy
- N: Number of sample inputs 
- $O_i^\pi$ : Set of K outputs from policy $\pi$ to input $i$

**Across-Input Diversity:**
$$ \text{AcrossInputDiversity}_D(\pi) = D\left(\bigcup_{i=1}^{N} O_i^\pi\right) $$

### Our Two Cents

The paper provides insights into RLHF, SFT, and BoN sampling, shedding light on the trade-offs between generalization and diversity in LLM fine-tuning. The trade-offs identified call for strategies that effectively balance these factors without significant compromises. Future research could explore hybrid methods or enhance RLHF with diversity-oriented adjustments.

### Resoruces

- [Paper Link](https://arxiv.org/pdf/2310.06452)

- [Code](https://github.com/facebookresearch/rlfh-gen-div)







