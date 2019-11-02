
# CyCADA: Cycle-Consistent Adversarial Domain Adaptation

Judy Hoffman, Eric Tzeng, Taesung Park, Jun-Yan Zhu, Phillip Isola, Kate Saenko, Alexei A. Efros, Trevor Darrell, ICML-2018

## Summary

This paper proposes a novel discriminatively-trained Cycle-Consistent Adversarial Domain Adaptation model. Leveraging the cycle-consistency, the model does not require aligned pairs and the author claims state-of-the-art results across multiple domain adaptation tasks.

## Main Contributions

- The main contribution of the paper is its novel techique for domain adaptation using cycle-consistency losses, taking inspiration from the *CycleGAN* paper. But while CycleGAN produced task-agnostic domain transfer, this model has been trained for various particular tasks.

-  Provided is source data X<sub>s</sub>, source labels Y<sub>s</sub>, target data X<sub>t</sub>, but no target labels. The goal is to learn a model *f* to correctly predict label for target data X<sub>t</sub>.

- What are the cases to be considered in the cases of basict things that we are 