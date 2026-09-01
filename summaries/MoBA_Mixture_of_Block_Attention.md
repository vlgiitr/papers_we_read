# MoBA: Mixture of Block Attention for Long-Context LLMs

Enzhe Lu, Zhejun Jiang, Jingyuan Liu, Yulun Du, Tao Jiang, Chao Hong, Shaowei Liu, Weiran He, Enming Yuan, Yuzhi Wang, Zhiqi Huang, Huan Yuan, Suting Xu, Xinran Xu, Guokun Lai, Yanru Chen, Huabin Zheng, Junjie Yan, Jianlin Su, Yuxin Wu, Neo Y. Zhang, Zhilin Yang, Xinyu Zhou, Mingxing Zhang, Jiezhong Qiu **NeurIPS** **2025**

## Summary

LLMs are widely used to process large amounts of text, documents and code, sometimes all at once. For example, AI coding agents read the entire codebase, documents from the internet and text files before making any changes to your code. Thus, long-context modelling is an active area of research which focuses on studying the performance of LLMs in long-context reasoning tasks and optimizing architectures to scale LLMs to context-lengths of millions of tokens. The attention mechanism in transformers scales with a quadratic time complexity as the context-length increases, because it requires every token to attend to every other previous token. Past work has identified several ways to optimize the attention mechanism for long context-lengths by sparsifying attention and optimizing attention calculation algorithms by utilizing the parallel computing power of GPUs. 

Attention has inherent sparsity, i.e. every token does not necessarily need to attend to every other token. Many sparse attention methods rely on this property and use heuristics (for example recent neighbours, attention sinks etc.) to make tokens attend to only some specific tokens, which improves the time complexity (sub-quadratic). However, these methods impose strong biases and are highly task specific. This paper introduces Mixture of Block Attention (MoBA), which aims to reduce this dependence on structured heuristics by routing tokens to important ones autonomously (routing is learned during training). They borrow the routing idea from Mixture-of-Experts (MoE) and apply it directly to attention. The context is chopped into fixed-size blocks, and for each query token a gating mechanism scores every historical block and dynamically selects only the top-k most relevant ones to attend to, with the current block always included (and causally masked) to preserve autoregressive validity. Moreover, it acts as a drop-in replacement for full attention, and can even be used together with full attention in a hybrid fashion. This architecture is being used in the lastest Kimi models and has shown to perform well on long-context modelling tasks in an efficient way.

## Contributions
- Introduces Mixture of Block Attention (MoBA), a novel attention architecture that applies Mixture-of-Experts (MoE) to the attention mechanism itself, letting the model dynamically learn which tokens to attend rather than relying on predefined structural biases like sink or sliding-window attention.
- Provides a theoretical unification of existing sparse attention schemes, showing that sliding window attention, attention sinks, [Quest](https://arxiv.org/abs/2406.10774) (Tang et al. 2025), and [LongHeads](https://arxiv.org/abs/2402.10685) (Y. Lu et al. 2024) can all be recovered as special cases of MoBA under specific gating-network constraints.
- Designs MoBA as a parameter-free, drop-in substitute for full attention, introducing no additional parameters. They demonstrate that MoBA can be hybridized with full attention, both across training stages and across layers.

## Method

**MoBA Attention.** In standard attention, a query token attends to all N key-value pairs: $Attn(q, K, V) = Softmax(qK^T)V$. MoBA instead restricts each query to a learned subset of keys and values:

$$\text{MoBA}(q, K, V) = \text{Softmax}(qK[I]^\top)V[I]$$

where $I \subseteq [N]$ is a dynamically selected index set.

**Block Partitioning.** The context of length $N$ is split into $n$ contiguous blocks of size $B = N/n$, with block $i$ covering the range $I_i = [(i-1)\times B + 1,\ i \times B]$.

**Gating Mechanism.** Each query computes an *affinity score* $s_i$ against every block, which is the inner product between the query and the **mean-pooled** key representation of that block:

$$s_i = \langle q, \text{meanpool}(K[I_i]) \rangle$$

Only the top-k scoring blocks are activated ($g_i = 1$ otherwise $0$); the query then attends solely to the union of tokens in those blocks: $I = \bigcup_{g_i > 0} I_i$.

**Causality.** Two mechanisms preserve autoregressive validity: (1) queries are never routed to future blocks ($s_i = -\infty$ if $\text{pos}(q) < i \times B$), and (2) the block containing the query itself (the "current block") is always selected and masked causally, preventing leakage from future tokens via mean pooling.

<div align="center">
<img width="50%" alt="architecture_diagram" src="../images/MoBA_Architecture_Diagram.png" />
<p>Fig. Illustration of mixture of block attention (MoBA).</p>
</div>


In this example, we have two query tokens and four KV blocks. The router (gating network) dynamically selects the top two blocks for each query to attend. As
shown in the figure, the first query is assigned to the first and second blocks, while the second query is assigned to the
third and fourth blocks.

<div align="center">
<img width="50%" alt="algorithm" src="../images/MoBA_Algorithm.png" />
<p>Algorithm: MoBA (Mixture of Block Attention) Implementation.</p>
</div>

## Results

### Downstream Benchmark Evaluation

Llama-3.1-8B continually pre-trained up to 1M context length (Llama-8B-1M-MoBA: block size 4096, top-k=12, sparsity up to 95.31%, last 3 layers full attention) vs. an equivalently trained full-attention baseline (Llama-8B-1M-Full). MoBA is used for **prefill** only; generation uses full attention.

**Table 3: Benchmark Comparison**

| Benchmark | Llama-8B-1M-MoBA | Llama-8B-1M-Full |
|---|---|---|
| AGIEval [0-shot] | 0.5144 | **0.5146** |
| BBH [3-shot] | 0.6573 | **0.6589** |
| CEval [5-shot] | **0.6273** | 0.6165 |
| GSM8K [5-shot] | **0.7278** | 0.7142 |
| HellaSWAG [0-shot] | 0.8262 | **0.8279** |
| Loogle [0-shot] | **0.4209** | 0.4016 |
| Competition Math [0-shot] | 0.4254 | **0.4324** |
| MBPP [3-shot] | **0.5380** | 0.5320 |
| MBPP Sanitized [0-shot] | **0.6926** | 0.6615 |
| MMLU [0-shot] | 0.4903 | **0.4904** |
| MMLU Pro [5-shot][CoT] | 0.4295 | **0.4328** |
| OpenAI HumanEval [0-shot][pass@1] | 0.6951 | **0.7012** |
| SimpleQA [0-shot] | 0.0465 | **0.0492** |
| TriviaQA [0-shot] | **0.5673** | 0.5667 |
| LongBench @32K [0-shot] | **0.4828** | 0.4821 |
| RULER NIAH multi 2 key @128K [0-shot] | 0.9700 | **1.000** |
| RULER NIAH multi 3 key @128K [0-shot] | **1.000** | **1.000** |
| RULER NIAH multi value @128K [0-shot] | **0.2975** | 0.2800 |

*(Bold = higher score.)*

- MoBA is highly competitive with full attention across nearly all short- and long-context benchmarks, with differences typically under 1–2 points.
- On **Needle-in-a-Haystack** (up to 1M context, evaluated across insertion depths 0–100%), Llama-8B-1M-MoBA scores consistently near-perfect (≈100) across the entire context range, demonstrating the long-context retrieval capability.

---

### Efficiency and Scalability

| Setting | Result |
|---|---|
| Prefill speedup at 1M tokens (attention layer only) | **up to 6.5×** vs. FlashAttention |
| Prefill speedup at 10M tokens (constant sparsity, expanded tensor parallelism) | **up to 16×** vs. FlashAttention |
| Sparsity maintained during 10M-token scaling | 95.31% (fixed 64 blocks, top-k = 3) |

- MoBA demonstrates **sub-quadratic scaling**, with the computational advantage over full attention becoming progressively larger as sequence length grows (comparable at 32K–512K, then diverging sharply beyond 1M tokens).

### Scaling Law Comparison (MoBA vs. Full Attention)

The authors train five language models (545M–2.1B params) following the Chinchilla scaling law, comparing MoBA (block size 512, top-k=3) against full attention at a sequence length of 8K.

**Fitted Scaling Law Curves**

<div>
<img width="50%" alt="scaling" src="../images/MoBA_Scaling_Law.png" />
</div>

| Metric | MoBA | Full Attention |
|---|---|---|
| LM loss (seqlen = 8K) | $2.625 \times C^{-0.063}$ | $2.622 \times C^{-0.063}$ |
| Trailing LM loss (seqlen = 32K, last 2K tokens) | $1.546 \times C^{-0.108}$ | $1.464 \times C^{-0.097}$ |

- At 8K sequence length (81.25% sparsity), validation loss curves for MoBA and full attention are nearly identical, with differences staying within **1e-3**.
- At 32K sequence length (95.31% sparsity), MoBA shows a marginally higher trailing-token loss than full attention, but the gap narrows as compute increases, indicating favorable long-context scalability.

---

### Fine-Grained Block Segmentation

Using a 1.5B model at 32K context, the authors vary block granularity while holding sparsity fixed at 75% (dividing into 8/16/32/64/128 blocks, selecting 2/4/8/16/32 blocks respectively).

- **Coarser blocks hurt performance**: the 2-of-8 configuration underperforms all finer-grained settings by roughly **1e-2 LM loss**.
- Performance stabilizes and nearly matches the full-attention baseline once granularity is sufficiently fine (8/32 and beyond), suggesting fine-grained segmentation benefits MoBA.

---

### Hybrid of MoBA and Full Attention

**MoBA/Full Hybrid Pre-training** (1.5B models, 30B tokens, 32K context, block size 2048, top-k=3):
- MoBA-only training shows higher position-wise loss on trailing tokens.
- The two-stage hybrid recipe (90% training tokens using MoBA → 10% training tokens using full attention) achieves position-wise loss nearly identical to full attention, with no observed loss spikes at the switch point.

**Layer-wise Hybrid (for SFT):**
- MoBA alone can underperform during supervised fine-tuning, likely due to sparse-gradient effects from SFT's loss masking.
- Switching only the last few Transformer layers to full attention (while keeping earlier layers as MoBA) closes this SFT loss gap.

---

## Two-Cents

The methodology is especially beneficial because it lets the routing mechanism learn which tokens to attend to during training itself, rather than relying on predefined heuristics for selection of tokens. It can work as a direct substitute to attention with no additional parameters, trains faster and closely follows the training convergence of full attention. However, they could have provided more analysis on why they chose the affinity score to be $$s_i = \langle q, \text{meanpool}(K[I_i]) \rangle$$. They could have discussed other scoring methods along with comparison versus their affinity score, under design choices.

## Resources
- [Paper](https://papers.nips.cc/paper_files/paper/2025/hash/19eae75beed66321d62272e794a9c2ac-Abstract-Conference.html)
- [Github](https://github.com/MoonshotAI/MoBA)
