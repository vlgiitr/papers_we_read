# Rainbow Memory: Continual Learning with a Memory of Diverse Samples

Jihwan Bang, Heesu Kim, YoungJoon Yoo, Jung-Woo Ha, Jonghyun Choi, **CVPR** **2021**

## Summary

This paper introduces a new memory management strategy called Rainbow Memory to improve Continual Learning, particularly Class Incremental Learning(CIL) with tasks that share classes(Blurry-CIL). It involves two steps. First is ensuring that sampling from stored memory is diverse enough, where diversity is looked at in the context of classification uncertainty of the sample when distorted by various Data Augmentation methods. Second is ensuring sample diversity by Data Augmentation(DA), primarily Mixed-Label DA and Automated DA.



## Contributions

- Effective memory management strategy which allows storing old samples to alleviate catastrophic forgetting in CIL.
- Diversity-aware sampling method for effectively managing memory with limited capacity by leveraging classification uncertainty.
- Use of Data Augmentation techniques to enhance the diversity of samples also helps alleviate problems caused by continuously changing class distribution of each task given a task stream.

## Method
- Memory Update :
    - The authors measure uncertainty by measuring the variance of model output in all samples, slightly modified by data augmentation techniques like color jitter, shear and cutout.


    - The authors approximate uncertainty by Monte Carlo method of the distribution p(y=c|x) when given prior to perturbed sample $\tilde{x}$  as p($\tilde{x}$|x). Perturbation prior is a uniform mixture of all perturbations.
                      
        $$p(y=c|x) = \int_{\tilde{D}} p(\tilde{x}_t|x)p(y=c|\tilde{x}_t)d\tilde{x}_t$$

  
        $$\approx \frac{1}{A}\sum_{t=1}^A p(y=c|\tilde{x}_{t})$$      
        
    - x, $\tilde{x}$, y, A, $\tilde{D}$ denote a sample, perturbed sample, label of sample, number of perturbation methods and data distribution defined by perturbed samples respectively.
    - The perturbed sample $\tilde{x}$ is drawn from a random function $f_r(.)$, where $\theta_r$ is a hyperparameter which denotes the random factor of the r-th perturbation, as:         
        $$\tilde{x} =f_r(x|\theta_r), r=1,2,...,R $$
    - The prior $p(\tilde{x}|x)$ is defined as:
        $$\tilde{x} \sim \sum_{r=1}^R w_r *f_r(x|\theta_r) $$
        where the random variable $w_r$, r={1,...,R} is drawn from a categorical binary distribution.
    
    - If u(x) is the uncertainty of the sample with respect to perturbation:
        $$u(x)=1 - \frac{1}{T} \max\limits_c S_c $$

        $$S_c = \sum_{t=1}^T 1_c arg\max_{\tilde{c}} p(y=\tilde{c}|\tilde{x}_t) $$

        $S_c$ is the number of times class c is predicted as the most likely class, and $1_c$ denotes the binary class indexing vector.
    - So if there are k memory slots, and our stored samples from the previous task and fresh samples from the current task add up to m samples, they are first arranged according to u(x), then a sample at an interval of $\frac{m}{k}$ is picked thus forming a new set of stored samples for the next task.

- Data Augmentation(DA) :
    - In mixed-label DA, images of new tasks and old tasks and their labels are mixed in the same proportion; this helps alleviate side effects caused by the change of class distribution over tasks and improves performance.
    - Automated DA: composes multiple DAs on model performance under CIL(Class Incremental Learning).

## Results

- This method outperforms all other methods and baselines in blurry CIL and online(buffer unavailable) setups, which are more realistic and practical.
- Even in Disjoint and offline(buffer available) setups, it either outperforms or shows comparable performance to other methods.
- Performance gap decreases as memory size increases as the impact of effective sampling decreases.



- Enhancement provided by CutMix and AutoAug is the most effective among all DAs.



## Two-Cents

- This paper proposes a novel idea that helps deal with many realistic issues of Class Incremental Learning and helps make it more valuable in the real world.

## Resources

- [ Paper on Rainbow Memory](https://arxiv.org/pdf/2103.17230.pdf)
- [Code and Data Splits](https://github.com/clovaai/rainbow-memory)