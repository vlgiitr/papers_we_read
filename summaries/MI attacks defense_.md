# Improving Robustness to Model Inversion Attacks via Mutual Information Regularization

Tianhao Wang, Yuheng Zhang, Ruoxi Jia

## Summary

This paper focuses on MI attacks, a type of privacy attacks aimed at reconstructing the input associated with any given model output.
In this paper, they presented a defense mechanism against MI attacks, called MID, which can achieve the state-of-the-art performance for a variety of target models, datasets, and attack alogrithms in both blackbox and whitebox settings.
The goal is to find a defense that can achieve best tradeoff between privacy and model utility.


## Method

- The goal of defense is to design an algorithm to train the target model on the data distribution (X, Y ) such that the access to the resulting model does not allow an adversary to infer the information about P(X|Yˆ = y). 
- The idea is to quantify the dependence between X and Yˆ using their mutual information I(X; Yˆ ) and incorporate it into the training objective as a regularizer.
- ![loss function_](https://github.com/NikharW/papers_we_read/blob/master/images/loss%20function_.png)
- Above image describes our loss function.
- L(y,f(x)) is the loss function for main prediction task, and lambda is the weight coefficient that controls the tradeoff between privacy and utility on the main prediction task.

## Results

- ![regularizer comaparision](https://github.com/NikharW/papers_we_read/blob/master/images/regularizer%20comparision.png)
- Including the regularizer term has made distribution of predicted values concentrated across lot of labels.
- Comparision of MID and DP (Differential Privacy) :
- ![mid and dp comparision](https://github.com/NikharW/papers_we_read/blob/master/images/mid%20and%20dp%20comparision.png)
- MID defense achieves better privacy-utility tradeoff compared to other defenses. Another point to notice is that increasing model utility increases the vulnerability to attacks.

## Two-Cents

A limitation of defense as mentioned in paper is that it does not prevent the attack and hence future work can be done in this direction.

## Resources

Link of paper : https://arxiv.org/pdf/2009.05241.pdf 