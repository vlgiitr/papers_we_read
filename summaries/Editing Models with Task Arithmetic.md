## Title: Editing Models with Task Arithmetic


**Authors:** Gabriel Ilharco, Marco Tulio Ribeiro, Mitchell Wortsman, Suchin Gururangan, Ludwig Schmidt, Hannaneh Hajishirzi, Ali Farhadi, March 2023
Paper- https://arxiv.org/pdf/2212.04089

**Abstract:**  
In "Editing Models with Task Arithmetic," the paper introduces a novel method for modifying the behavior of pre-trained neural networks using task vectors. Task vectors are directions in the weight space of a pre-trained model, created by subtracting the model's original weights from its weights after fine-tuning on a specific task. This approach allows for performance improvements on target tasks by moving in the direction of the task vector without expensive pre-training, reducing costs significantly.

The paper outlines three operations on task vectors from various fine-tuned LLMs:

- **Negation:** Reducing performance on a task by negating the task vector, with minimal impact on other tasks.
- **Addition:** Enhancing performance across multiple tasks by adding task vectors together.
- **Analogy Relationships:** Improving performance on a fourth task by combining task vectors from three related tasks, even without training data for the fourth task.

Experiments across various models, modalities, and tasks demonstrate that task arithmetic is an efficient and effective technique for fine-tuning model behavior.

# Task Vectors and Task Arithmetic

<img src='../images/Editing using Task Arithmetic.png'>

**Contributions:**
- **Unlearning/Forgetting:** Using negative task vectors to correspond to a given task.
- **Building Models from Multiple Models:** Knowledge transfer and model merging.
- **Subpopulations with Little Data:** Task Analogies address the issue of data scarcity by leveraging existing task vectors to handle cases where quality datasets are unavailable.
- **Domain Generalization Using Task Analogies:** Task vectors can generalize and use analogies to enable models to perform well on various tasks based on the context provided by the task vectors.

**Results:**  
Editing LLMs using task vectors has achieved state-of-the-art performance levels.

1. **Forgetting via Negation:**
   <img src='../images/Editing using Task Arithmetic-Forgetting via negation.png'>  

3. **Addition of Task Vectors:**
   <img src='../images/Editing using Task Arithmetic-addition.png'>

5. **Usage of Task Analogy:**
   <img src='../images/Editing using Task Arithmetic-Task Analogy.png'>


**Two Cents:**  
This paper introduces a new method for editing models based on arithmetic operations over task vectors. For various vision and NLP models, adding multiple specialized task vectors results in a single model that performs well on all target tasks or even improves performance on a single task. Negating task vectors allows users to remove undesirable behaviors, such as toxic generations, or even forget specific tasks altogether while retaining performance elsewhere. Task analogies leverage existing data to improve performance on domains or subpopulations where data is scarce. These operations are efficient to compute, especially compared to alternatives involving additional fine-tuning or pre-training. They recycle and transfer knowledge from large collections of publicly available fine-tuned models. Since these operations result in a single model of the same size, they incur no extra inference cost or computational resources.

**Resources:**
- Research Paper: https://arxiv.org/pdf/2212.04089 (Main paper)("EDITING MODELS WITH TASK ARITHMETIC")
- 
- Additional Paper: [Task Arithmetic in the Tangent Space: Improved Editing of Pre-Trained Models](https://arxiv.org/pdf/2305.12827)
- 
- Code: [Task Vectors GitHub Repository](https://github.com/mlfoundations/task_vectors)
