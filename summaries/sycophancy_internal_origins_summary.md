# When Truth Is Overridden: Uncovering the Internal Origins of Sycophancy in Large Language Models
Keyu Wang, Jin Li, Shu Yang, Zhuoran Zhang, Di Wang, **AAAI** **2026**

## Summary

We all know that LLMs cave when we push back on them, even when they are right and we are wrong. Say "I believe the answer is B" when the answer is actually C, and there's a decent chance the model will just agree. This paper asks a basic question: what is actually happening inside the model when this flip happens? Where does "I know the right answer" turn into "actually, you are right"?

The authors run a clean, controlled setup across seven different open LLMs and use interpretability tools (logit-lens and activation patching) to watch the model's internal preferences evolve layer by layer. What they find is that sycophancy isn't some vague surface-level quirk, it's a very specific, traceable computational event that happens late in the network, and they can even reach in and flip it on or off by editing activations at a single layer.

![Prompt types](../images/prompt.png)

## Contributions

- Shows that just stating an opinion ("I believe the answer is X") is enough to reliably tank accuracy across seven different model families, but dressing that opinion up with claimed expertise (beginner vs. expert) barely moves the needle.
- Introduces a layer-wise "Decision Score" (built on logit-lens) to pinpoint exactly when a model's internal preference tips from the correct answer toward the user's incorrect one.
- Uses KL divergence between hidden state distributions to show that the model's internal representation itself gets restructured in the final layers.
- Confirms causality (not just correlation) via activation patching: swapping activations at the critical layer can suppress or induce sycophancy on demand.
- Finds that grammatical framing matters more than claimed authority, "I believe..." causes noticeably more sycophancy than "They believe...".

## Method

- **Setup**: Seven LLMs of similar size (Llama3.1 8B, Qwen2.5 7B, OPT 6.7B, Mistral 7B, Falcon 7B, OLMoE 1B-7B, Pythia 6.9B) are tested on MMLU questions under four prompt conditions: Plain (no opinion), Opinion-only ("I believe the answer is X" where X is wrong), Opinion + expertise level (Beginner/Intermediate/Advanced, first-person), and Opinion + expertise level (third-person, "They believe...").

- **Decision Score**: At every transformer layer, the hidden state is projected through the model's output head (logit-lens) to see what answer it would pick if the model stopped right there. This is normalized into a 0 to 1 score:

  $$
  DS(x) = \frac{l_x - \min(l_A, l_B, l_C, l_D)}{\max(l_A, l_B, l_C, l_D) - \min(l_A, l_B, l_C, l_D) + \epsilon}
  $$

  where $l_A, l_B, l_C, l_D$ are the logits (via logit-lens) for the four multiple-choice options at that layer, and $\epsilon = 10^{-9}$ prevents division by zero. Tracking this across layers for both the correct answer and the user's wrong answer shows exactly where the model's preference shifts.

- **KL Divergence**: Layer-wise Kullback-Leibler divergence between the output distributions of Plain and Opinion-only conditions:

  $$
  D_{KL}(P \| Q)
  $$

  where $P$ and $Q$ are the probability distributions produced by applying logit-lens to hidden states from the Plain and Opinion-only prompts respectively. This quantifies how much the model's internal representation (not just its final answer) shifts because of the opinion, with a sharp increase signaling the layer where the opinion starts distorting internal processing.

- **Activation Patching**: At the "critical layer" (where KL divergence peaks, layer 32 for Llama3.1 8B-Instruct and layer 27 for Qwen2.5 7B-Instruct), the authors swap hidden states between a Plain run and an Opinion-only run in both directions:
  - **Suppressing sycophancy**: patch the Plain activation into an Opinion-only run.
  - **Inducing sycophancy**: patch the Opinion-only activation into a Plain run.

  This tests whether that layer's representation is actually *causing* the sycophantic answer, not just correlated with it.

- **PCA + cosine similarity**: Hidden states from the critical layer are projected with PCA to visually check whether different expertise levels, or different pronoun framings (first-person vs. third-person), form separate clusters in representation space. Cosine similarity between class centroids gives a quantitative measure of that separability.

## Results

- Opinion alone drops accuracy hard, sycophancy rates jump to an average of 63.7% (range 46.6% to 95.1%) across the seven models, compared to their much higher plain-prompt accuracy. Even a simple, unsupported opinion is enough to substantially shift model predictions.
- Expertise framing barely matters, sycophancy rate changes by less than 4.4% between Beginner and Advanced conditions for any given model. The models just don't seem to internally represent what expertise means, PCA shows all three expertise levels collapsing into one overlapping cluster, while the Opinion-only condition forms a clearly separate cluster.

![PCA projection of prompt token hidden state to show sycophancy variation with credibility](../images/pca.png)

- Decision Score shows the tipping point happens around layer 16 to 19 (out of about 32) for Llama, well before the KL divergence spike around layer 23. This two-step pattern (output preference shifts first, then deeper representation reorganizes) is consistent across models.
- Activation patching gives clean causal evidence: patching Plain activations into an Opinion-only run cut Llama's sycophancy by 36 percentage points, and patching the reverse direction induced a 47-point increase (and roughly symmetric effects for Qwen2.5). This isn't just correlation, that one layer's representation is doing real causal work.
- First-person vs. third-person framing produces a genuinely large behavioral gap, first-person prompts average 13.6% higher sycophancy than third-person ones across all seven models, and this shows up as near-orthogonal representation clusters (cosine similarity as low as -0.04) in the model's internal space, indicating grammatical person is a more salient processing axis than claimed expertise.

## Two-Cents

This paper depicts good analysis of sycophancy in LLMs by mechanistic interpretability tools. It also supports its claims with good plots. The expertise-vs-pronoun finding is genuinely interesting too: it suggests these models aren't tracking "is this person credible" as a concept at all, they're reacting to surface grammatical form ("I" vs "they").
Possible future direction: This study is entirely on MMLU multiple-choice questions and the results in open-ended generation are left as future work.

## Resources
- Paper (arXiv): https://arxiv.org/abs/2508.02087
- Code: https://github.com/kaustpradalab/LLM-sycophancy
