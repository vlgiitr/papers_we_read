# SparseGPT: Massive Language Models Can be Accurately Pruned in One-Shot
## AUTHOR:
Elias Frantar ,Dan Alistarh 
## Summary:

SparseGPT is a post-training pruning method for compressing large language models such as GPT3 efficiently and accurately. The method can be used to prune large language models in one-shot with minimal accuracy loss. For example, you can use SparseGPT to prune OPT-175B to 50% sparsity with a 0.13 decrease in perplexity.Thus 100 billion weights from the model can be ignored at inference time, increasing the model's throughput while reducing latency. SparseGPT can be applied to a GPT model with 175B parameters on a single GPU in a couple of hours with negligible accuracy loss.

## Contribution:
 becuase Large language models (LLMs) solve natural language processing problems with astounding accuracy. However, these models are enormous and require a lot of space, cost, and computation power to deploy. For example, the GPT-175B model has 175 billion parameters requiring 320GB of storage and at least 5 A100 GPUs with 80GB of memory each for inference. This computation power is expensive, making this solution only viable for some organizations. Hence, deployment of such models falls outside the purview of small organizations and individuals. 

## Remark:
Post-training compression is usually done by splitting the full-model compression problem into layer-wise subproblems, whose solution quality is measured in terms of the ` 2-error between the output, for given inputs X` , of the uncompressed layer with weights W` and that of the compressed one.
methodology:
The SparseGPT algorithm works as follows, given a fixed pruning mask: 
:Prune weights in each column of the weight matrix incrementally using a sequence of hessian inverse

:Update the remainder of the weights in the rows located to the right of the column being processed

SparseGPT is local because it performs weight updates after each pruning step, maintaining the input-output relationship between each layer. The high parametrization of GPT models makes it possible to make the updates without any global gradient information. The cost of the reconstruction process consists of the computation of the initial Hessian, iterating through the inverse Hessian sequence, and pruning.

The pseudocode is interpreted as:
1.Create a pruning mask M with zeros and ones.
2.Construct a matrix E to store the block quantization errors.
3.Calculate the inverse Hessian sequence information 
4.Loop the blocks while updating the pruning mask and the error.
5.Select the largest weights for each block and set their corresponding values in the pruning mask to 1.
6.Prune the remaining weights and set their corresponding values in the pruning mask to 0.
7.Compute the pruning error for each weight in the block and set the error in the E matrix.
8.Freeze the weights that were not pruned.
9.Update the weights in the block using the Cholesky decomposition of the inverse Hessian matrix.
10.Update weights not updated in the previous loop after processing all the blocks.
11.Set pruned weights to 0 by element-wise multiplying the weight matrix with the pruning mask.

## Two cents:
SparseGPT solves the row-hessian challenge by reusing Hessians between rows and distinct pruning masks, leading to an accurate and efficient algorithm.

large-scale generative pretrained Transformerfamily models can be compressed to high sparsity via weight pruning in one shot, without any retraining, at low loss of accuracy, when measured both in terms of perplexity and zero-shot performance through this method.

## RESOURCE
https://paperswithcode.com/paper/massive-language-models-can-be-accurately

