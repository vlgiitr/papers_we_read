# TIES-MERGING: Resolving Interference When Merging Models

Prateek Yadav, Derek Tam, Leshem Choshen, Colin Raffel, Mohit Bansal, NeurIPS, 2023

## Summary

This paper addresses the challenge of combining multiple fine-tuned models into a single multitask model without additional training. Traditional model merging techniques often suffer from performance degradation due to parameter interference. The authors identify two primary sources of this interference: redundant parameter updates and sign conflicts across models. To mitigate these issues, they propose TIES-MERGING, a method that systematically resolves such interferences, leading to more effective model merging.

## Contributions

- Identification of Interference Sources: The paper highlights two main interference types in model merging:
    - Redundant Parameter Updates: Parameters that have negligible changes during fine-tuning.
    - Sign Conflicts: Parameters updated in opposite directions across different models.

- Introduction of TIES-MERGING: A method of merging that prevents interference.

## Method

- The TIES-MERGING approach involves the following steps:

    1. Trim: For each task, keep the top k% of task vector values and set the rest to 0.
    2. Elect Sign: Find the sign with the highest total magnitude across all relevant models. This majority sign is considered the "elected" sign for that parameter.
    3. Disjoint Merge: Merge only those parameter updates that align with the elected sign. Compute the average of these aligned updates to obtain the final parameter value.

- Scale the merged task vectors and add them to the original parameter values.

## Results

- Performance: TIES-MERGING outperforms averaging, Fisher Merging, RegMean, and Task Arithmetic. The improvement in performance increases as the number of merged models increases.

- Robustness: The method shows consistent improvements across diverse settings, including different tasks, domains, and model architectures. It also scales well as the number of tasks increases.

- Ablation Studies: 
    - Trim Step: Removing this step leads to increased noise from parameters that did not meaningfully change during fine-tuning.
    - Elect Sign Step: Omitting this causes performance to drop sharply, especially when sign conflicts are prevalent — validating this as a core strength of the method.
    - Disjoint Merge: Ensures cleaner parameter integration — performing better than simple averaging or majority vote across entire parameter vectors.

## Two-Cents

TIES-MERGING presents a thoughtful approach to a nuanced problem in model merging. By dissecting the sources of interference and addressing them systematically, the authors provide a practical solution that enhances multitask learning without additional training overhead. Future research could explore extending the method to other model types beyond transformers.

## Resources

Paper: [TIES-MERGING: Resolving Interference When Merging Models](https://proceedings.neurips.cc/paper_files/paper/2023/file/1644c9af28ab7916874f6fd6228a9bcf-Paper-Conference.pdf)
Code Repository: [GitHub - prateeky2806/ties-merging](https://github.com/prateeky2806/ties-merging)