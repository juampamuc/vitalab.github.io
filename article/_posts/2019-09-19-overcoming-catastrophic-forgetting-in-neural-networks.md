---
layout: review
title: "Overcoming catastrophic forgetting in neural networks"
tags: deep-learning continual-learning catastrophic-forgetting
author: "Bach Kim"
cite:
    authors: "James Kirkpatrick et al."
    title:   "Overcoming catastrophic forgetting in neural networks"
    venue:   "Proceedings of the National Academy of Sciences of the United States of America"
pdf: "https://arxiv.org/pdf/1612.00796.pdf"
---

#  Introduction
The ability to learn tasks in a sequential fashion is crucial to the development of artificial intelligence. Neural networks are not, in general, capable of this and it has been widely thought that catastrophic forgetting is an inevitable feature of connectionist models. 
In this study, the research group proposed a continual learning method namely Elastic Weight Consolidation (EWC) that selectively slows down the update on the important weights associated with old tasks based on Fisher information during training on new task.
# Method
- Using Bayes’ rule, the log value of the posterior probability is:
$$ log P(\theta|D) = log P(D|\theta) + log P(\theta) - log P(D)$$
- Assume that the data $$D$$ consists of two independent parts: $$D_A$$ for task $$A$$ and $$D_B$$ for task $$B$$, the above equation can be re-written as:
$$ log P(\theta|D) = log P(D_B|\theta) + log P(\theta|D_A) - log P(D_B)$$
- The left hand side of the equation is still he posterior distribution given the entire data-set, while the right side only depends on the loss function for task $$B$$, i.e., $$log P(D_B|\theta)$$. All the information related to task $$A$$ is embedded in the term $$ log P(\theta|D_A) $$. EWC wants to extract information about weights' importance from $$log P(\theta|D_A) $$. Unfortunately, $$ log P(\theta|D_A) $$ is intractable. Thus, EWC approximates it as a Gaussian distribution with mean given by the parameters $$\theta^*_A$$ and a diagonal precision by the diagonal of the Fisher information matrix $$F$$. Thus, the new loss function in EWC is:
$$L(\theta)=L_B(\theta) + \sum_i (\lambda/2) F_i (\theta_i-\theta^*_{A,i})^2 (*)$$
where $$L_B(\theta)$$ is the loss for task $$B$$ only; $$\lambda$$ controls the importance of the old task compared to the new task; $$i$$ denotes the index in the weight vector.
- If $$\theta$$ has $$n$$ dimensions: $$[\theta_1,...,\theta_n]$$, the Fisher information matrix $$F$$ is a $$n \times n$$ matrix with each entry being:
$$I(\theta)_{ij}=E_X[\frac{\partial log P(D|\theta)}{\partial \theta_i} \frac{\partial log P(D|\theta)}{\partial \theta_j}|\theta]$$
- The diagonal entry is then:
$$F_i = I(\theta)_{ii}=E_X[(\frac{\partial log P(D|\theta)}{\partial \theta_i})^2|\theta]$$
- When a task $$B$$ comes, EWC updates Equation (*) with the penalty term enforcing the parameters $$\theta$$ to be close to $$\theta^*_A$$, where $$\theta^*_A$$ is the parameters learned for tasks $$A$$.

## Illustration of EWC
![](/article/images/overcoming-catastrophic-forgetting-in-neural-networks/ewc.png)

# Experiments
- Dis-joint MNIST.
- Atari games.

## Results
![](/article/images/overcoming-catastrophic-forgetting-in-neural-networks/ewc_disjoint_mnist.png)
![](/article/images/overcoming-catastrophic-forgetting-in-neural-networks/ewc_atari_games.png)

# Conclusion
- The EWC algorithm can be grounded in Bayesian approaches to learning. Formally, when there is a new task to be learnt, the network parameters are tempered by a prior which is the posterior distribution on the parameters given data from previous task(s). This enables fast learning rates on parameters that are poorly constrained by the previous tasks, and slow learning rates for those which are crucial.
