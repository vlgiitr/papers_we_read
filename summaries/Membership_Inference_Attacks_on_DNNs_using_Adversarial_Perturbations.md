# MI_Attacks_On_DNNs_Using_Adversial_Perturbations 
Hassan Ali1,Adnan Qayyum,Ala Al-Fuqaha, Junaid Qadir


## Summary

The research paper focuses on membership inference attacks on Deep Neural Networks (DNNs) in the context of adversarial perturbations. It introduces an innovative framework comprising three stages: preparation, indication, and decision. The paper presents two novel attacks, Adversarial Membership Inference Attack (AMIA) and Enhanced AMIA (E-AMIA). AMIA efficiently utilizes membership and non-membership information while minimizing a novel loss function in an adversarial context. E-AMIA integrates EMIA and AMIA, achieving impressive True Positive Rates (TPRs) on datasets. The paper also introduces augmented indicators, a new evaluation metric (RTA), and examines transferability of MI attacks. It suggests counterstrategies against such attacks, including differentially private SGD training and L2 regularization.


## Contributions:

 - Introduction of the AMIA and E-AMIA attacks for membership inference.
 - Development of augmented indicators and the RTA evaluation metric.
 - Insights into the transferability of MI attacks.
 - Exploration of counterstrategies such as differentially private SGD training and L2 regularization.


## Method:

The paper's methodology consists of three stages: preparation, indication, and decision. It introduces AMIA and E-AMIA attacks. AMIA computes small magnitude perturbations to input data, efficiently exploiting the loss landscape of shadow DNNs for membership inference. E-AMIA combines EMIA and AMIA, achieving impressive results on various datasets. The paper also proposes augmented indicators that enhance loss information within a Gaussian neighborhood of a subject and introduces the RTA evaluation metric.

<img src='../images/mi_attack_using_adversial_perterbation_1.png'>


## Results:

 - AMIA achieves a 6% True Positive Rate (TPR) on Fashion-MNIST and MNIST datasets.
 - E-AMIA achieves 8% TPR on Fashion-MNIST and 4% TPR on MNIST datasets at a 1% False Positive Rate (FPR).
 - Augmented indicators improve TPR by an average of 2.5% on Fashion-MNIST and 0.25% on MNIST datasets at 1% FPR.
 - AMIA is the most transferable MI attack, followed by E-AMIA and f-LiRA, with EMIA being the least transferable.
 - Insights into counterstrategies against MI attacks, including differentially private SGD training and L2 regularization.


## Two Cents:

This paper presents a significant advancement in the field of membership inference attacks on DNNs. The AMIA and E-AMIA attacks, along with augmented indicators and the RTA metric, provide valuable tools for assessing the vulnerability of DNNs to privacy breaches. The insights into transferability and counterstrategies offer practical implications for securing sensitive data. However, further research should explore the robustness of these attacks against more advanced defenses and consider real-world applications beyond datasets. Overall, this paper contributes significantly to the understanding and mitigation of membership inference attacks in the age of adversarial perturbations.

## Resources

- Orignal Paper[https://arxiv.org/abs/2307.05193/pdf]
