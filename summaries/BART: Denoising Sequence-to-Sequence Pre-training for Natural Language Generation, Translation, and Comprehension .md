# BART: Denoising Sequence-to-Sequence Pretraining for Natural Language Understanding and Generation

**Authors:** Mike Lewis, Yinhan Liu, Naman Goyal, Marjan Ghazvininejad, Abdelrahman Mohamed, Omer Levy, Veselin Stoyanov, Luke Zettlemoyer

**Conference:** ACL (Association for Computational Linguistics) 2020

## Summary
BART, short for Bidirectional and Auto-Regressive Transformers, represents a groundbreaking development in the field of natural language processing. This paper introduces a versatile denoising autoencoder designed for pretraining sequence-to-sequence models. The primary objective of BART is to address the inherent challenges in NLP tasks, including text generation and comprehension. The model achieves this through a unique combination of bidirectional encoding and left-to-right autoregressive decoding within a Transformer-based architecture.

## Contributions
- BART introduces an innovative denoising autoencoder framework that significantly advances the field of NLP.
- The model's performance in text generation and comprehension tasks sets new benchmarks, with broad applicability across NLP applications.
- BART's design enables fine-tuning on various NLP tasks, making it an indispensable tool for researchers and practitioners.

## Method
BART's architecture is built upon the well-established Transformer neural network. This architecture encompasses both bidirectional encoding and left-to-right autoregressive decoding. During the pretraining phase, BART follows a two-stage process: text corruption and model learning for text reconstruction. Diverse text corruption techniques, including token masking, token deletion, text infilling, sentence permutation, and document rotation, are employed to improve the model's understanding of contextual information and text generation capabilities.

Mathematical equations are an integral part of optimizing the negative log likelihood of the original document during pretraining. The fine-tuning phase encompasses a range of NLP tasks, such as sequence classification, token classification, sequence generation, and machine translation, where BART adapts its learned representations to specific task requirements.

## Results
BART outperforms existing models on a variety of NLP tasks, particularly excelling in text generation tasks, including abstractive summarization, dialogue response generation, and question answering. Notably, it achieves remarkable gains, with up to a 6-point improvement in ROUGE scores for summarization tasks and a 1.1 BLEU increase over back-translation systems for machine translation. Importantly, BART consistently delivers strong performance across the spectrum of NLP tasks.

## Comparisons with Baselines
In direct comparisons with various pretraining objectives, including conventional language models, permuted language models, and masked language models, BART consistently demonstrates its strength and adaptability across different tasks. It showcases that its unique design significantly enhances its performance.

## Two-Cents
BART's introduction is a landmark moment in NLP, as it successfully marries the advantages of bidirectional encoding with autoregressive decoding. Its adaptability for fine-tuning on a wide array of NLP tasks is a testament to its versatility. BART's remarkable performance in text generation tasks, including summarization and dialogue response, is a testament to its efficacy. While BART has raised the bar in NLP research, future endeavors might explore novel text corruption techniques and further optimization for specific applications.

## Resources
- [BART Paper (PDF)](https://paperswithcode.com/paper/bart-denoising-sequence-to-sequence-pre)

