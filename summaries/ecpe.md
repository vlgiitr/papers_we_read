# Emotion-Cause Pair Extraction: A New Task to Emotion Analysis in Texts
 
Rui Xia, Zixiang Ding
 
## Summary
 
This paper introduces a  new task called **emotion-cause pair extraction**  (ECPE), which aims to find emotion-cause pairs, this is different from emotion-cause pair extraction(ECE) as it does not depend on emotion annotations, they also propose a novel architecture to solve this task. **This paper also got the Outstanding Paper Award at ACL-2019**
 
## The ECPE task
 
- They work on improving the emotion clause - cause clause detection task, in which we identify clauses which express an emotion (emotion) and find the clauses that cause this emotion (cause clause).
 
- They suggest that because the ECE task requires emotion annotations beforehand it is not practically applicable and also as emotion and cause clause are co-related .....annotating the emotion clause separately beforehand doesn't take any advantage of this co-relation .....they try to fix these two shortcomings through the ECPE task.
 
- So the ECPE task basically has two tasks ..1.)  We first do both emotions and cause clause extraction using multi-task learning model(**Individual Emotion and Cause Extraction**) ...which they propose two variants of ....2.) We pair all the emotion and clauses extracted in step 1 together ...and then try to identify which of them actually form an emotion-cause pair. (**Emotion-Cause Pairing and Filtering**)
 
 
<img src="../images/ecpe.png" width="100%" title="hover text">
 
 
## Model
 
- So first comes Individual Emotion and Cause Extraction, they propose two variants for this task.
 
- First is **Independent Multi-task Learning**, so what they do is basically they separate the document into many clauses ...and each clause has some words. So for each clause, they pass its words through a Bi-LSTM, then apply attention to the hidden states obtained to get a clause representaion `s`.
 
- These clause representations `s` then go on to the upper layer which has two components, one for emotion extraction and other for clause extraction, both of them are Bi-LSTM, which predict whether each clause is an emotion (cause for the second component) clause given the `s` vectors as inputs.
 
- We then use a weighted sum of the cross-entropy loss of the two tasks as our loss function.
 
- But wait ...didn't we wanted to use the correlation between emotions and cause, so  the second method is  **Interactive Multi-task Learning**, this also has two variants 1.) Inter-EC in which we use emotion extraction to improve cause extraction, 2.) Inter-CE in which we use cause extraction to improve emotion extraction. Both of them are almost similar so the paper only introduces Inter-EC.
 
- In Inter-EC the lower component which extracts `s` is same as before, the upper layer contains two components, The first one, a Bi-LSTM takes the `s` as input and predicts if its an emotion clause. We embed this label as a vector and pass this vector to the second component which uses this vector along with `s` for cause clause extraction using a Bi-LSTM.
 
 
- Now that we the emotions and cause clauses, we do cartesian product to obtain all possible emotion and cause pairs. We then use the `s` of both of these clauses, along with a vector `v` representing the distance between the two, and pass it to a logistic regression model to detect if the pair is valid
 
## Main Results  and Contributions
 
- Both Inter-EC and Inter-CE get great improvement compared to the Independent model.
 
- They find that the improvements in Inter-EC are mainly in the recall rate on the cause extraction task, which shows that the predictions of emotion extraction are helpful in cause extraction.
 
- Similarly, for Inter-CE the improvements are mainly in the precision score on the emotion extraction task, which shows that the predictions of cause extraction are helpful in emotion extraction.
 
- They find that the improvements of Inter-EC on the cause extraction task are much more than the improvement of InterCE on the emotion extraction task, which can be due to the fact that cause extraction is a more challenging task.
 
- They observe that their model also performs comparably to the other models which depend on emotion annotation on the ECE task, which means they introduced practicality to the task while maintaining performance.
 
## Our Two Cents
 
- The paper's explanation is extremely lucid and clear ...which makes the paper an easy and intuitive read.
- The paper openly admits that there is more scope of improvement in their models for the task of cause extraction and they also want to make the model a single step .....so the paper admits its own faults and seems to want to work on it in the future.
 
 
## Implementation and References
 
- https://github.com/NUSTM/ECPE

