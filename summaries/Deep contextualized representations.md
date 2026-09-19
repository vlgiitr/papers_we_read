## Title: Deep Contextualized Word Representations

**Authors:** Matthew E. Peters, Mark Neumann, Mohit Iyyer, Matt Gardner, Christopher Clark, Kenton Lee, Luke Zettlemoyer (2018)

**Abstract:**  
This paper talks about deep contextualized word representations, a novel approach to capturing the meaning of words in context. Traditional static word embeddings like Word2Vec and GloVe provide a single vector for each word, regardless of its usage in different contexts. This paper’s model produces representations that dynamically change based on the context (not only local context but also takes care of global context) in which the word appears.

![Multi layer bidirectionals LSTM model to get ELMo embeddings](https://www.mihaileric.com/static/baseline_biLM-f9173e8e65a8597d3d8a909f3aee39f1-36fb0.png)

This is achieved through a bidirectional language model (biLM) - has bidirectional multi-layer LSTM’s as a memory element - that is pre-trained on a large corpus and fine-tuned for specific tasks. The resulting embeddings, known as ELMo (Embeddings from Language Models), significantly improve performance on a wide range of natural language processing tasks including:
1. Question answering
2. Textual entailment
3. Semantic role labeling
4. Named Entity Extraction
5. Sentiment Analysis
6. Coreference resolution

## ELMo Architecture
![ELMo Architecture](https://www.researchgate.net/profile/Gregory-Senay/publication/344603301/figure/fig1/AS:945655694508034@1602473295832/Masked-ELMo-architecture-composed-by-2-bidirectional-LSTM-layers.png)

**Contributions:**
- Introduction of ELMo (Embeddings from Language Models): The paper introduces ELMo, generates embeddings based on the entire context in which a word appears.
- Bidirectional Language Model (biLM): The paper utilizes a deep bidirectional LSTM (Long Short-Term Memory) network for training the language model. This model reads text in both forward and backward directions, enabling it to gather comprehensive contextual information.
- Integration of Character-level Information: To handle out-of-vocabulary words and morphological variations, ELMo incorporates character-level information, which enhances the robustness and flexibility of the embeddings according to context.
- Ablation Studies: The paper includes thorough ablation studies that illustrate the importance of various components of the model, such as bidirectionality and character-level convolutions, in achieving the observed performance gains.

**Result:**  
ELMo embeddings used in models led to substantial improvements over existing static embeddings across various tasks. For example, in the Stanford Question Answering Dataset (SQuAD), our model achieved state-of-the-art performance.


**Two Cents**
By introducing ELMo embeddings for  generating word representations that dynamically adapt to context addresses a fundamental limitation of traditional static embeddings such as GLOVe,Word2Vec .This also gives state of the art performance on various NLP tasks.

**Resources:**
- Research Paper: [1802.05365 (arxiv.org)](https://arxiv.org/abs/1802.05365)
- Blogs:
  - [Masked ELMo architecture](https://www.researchgate.net/figure/Masked-ELMo-architecture-composed-by-2-bidirectional-LSTM-layers_fig1_344603301)
  - [Deep Contextualized Word Representations (ELMo)](https://www.mihaileric.com/posts/deep-contextualized-word-representations-elmo/)
