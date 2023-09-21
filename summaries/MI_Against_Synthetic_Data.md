# Membership Inference Attacks against Synthetic Data through Overfitting Detection
Boris van Breugel,Hao Sun Zhaozhi Qian,Mihaela van der Schaar, **AISTATS**,**2022**

## SUMMARY:

The paper focuses on Membership Inference Attacks (MIAs), where attackers try to determine if a real sample was used in training a model. The authors introduce DOMIAS, a novel density-based MIA model that targets local overfitting of generative models. DOMIAS assumes attackers have knowledge of the data distribution, making it more realistic than previous methods. It outperforms existing techniques, especially for attacking uncommon samples, and shows that MIAs can be more successful when attackers have independent data from the same distribution.

## Contributions:
- Introduction of DOMIAS, a density-based MIA model, with a more realistic assumption about attacker knowledge.
- Significant improvement in MIA performance, particularly for attacking underrepresented samples.
- Demonstration that MIAs can be successful in realistic settings.
- Discussion on the choice of density estimator for approximating data distributions.

## Method:
DOMIAS is a density-based MIA model designed to detect membership by targeting local overfitting in generative models. It assumes attackers have knowledge of the data distribution. The paper discusses density estimation methods for approximating data distributions and highlights the importance of choosing the right estimator based on dataset size and prior knowledge. DOMIAS's performance is evaluated against previous methods in both the white-box and black-box settings.


<img src='../MI_against_synthetic_data_through_overfitting.png'>

<img src='../MI_against_synthetic_data_through_overfitting2.png'>

## Results:
- DOMIAS outperforms existing MIA methods, especially for attacking underrepresented samples.
- MIAs can be more successful than previously recognized when attackers have access to independent data from the same distribution.
- The choice of density estimator impacts the effectiveness of DOMIAS and similar MIA models.

<img src='../MI_against_synthetic_data_through_overfitting3.png'>


## Two-Cents:
This paper presents an important advancement in understanding and defending against Membership Inference Attacks on synthetic data. DOMIAS's realistic assumptions and improved performance highlight the potential privacy risks associated with synthetic data. The emphasis on data representation and the significance of adequate representation for underrepresented groups in datasets are noteworthy. However, further research should explore the scalability and robustness of DOMIAS in real-world applications. Overall, this paper provides valuable insights into MIA attacks and privacy in synthetic data, paving the way for improved privacy protection in data sharing and utilization.
