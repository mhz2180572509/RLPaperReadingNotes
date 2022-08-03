# [HASSLE] Hierarchical Reinforcement Learning with Subpolicies Specializing for Learned Subgoals

> Bram Bakker and Jürgen Schmidhuber. Hierarchical Reinforcement Learning with Subpolicies Specializing for Learned Subgoals. 

![Hierarchical Reinforcement Learning with Subpolicies Specializing for Learned Subgoals](./6-HASSLE.md)

## Overview

The paper proposed the ***Hierarchical Assignment of Sub-goals to Sub-policies LEarning algorithm (HASSLE)*** to automatically discovery the sub-goals and synchronously learn to **specialize** the skills/policies to achieve sub-goals. The algorithm establish a two-level hierarchical architecture (N-level also works), where the high-level learns to select a desired sub-goal (a kind of abstract observation the agent want to reach in next steps) at a coarse level, while the low-level learns to specialize the sub-policies to reach corresponding sub-goals at a fine-grained level. Both two levels learn by standard RL algorithms. The algorithm can be implemented into both MDP and POMDP models, both of which are evaluated by experiments in a navigation environment.

## Main Innovations

The main components of the HASSLE algorithm is as follows (suppose the structure only has two levels):

1. For the high level, it learns a policy to select the next desired observation at **an abstract observation space**, which can be seen as a clustering of primitive, low-level observations.
2. For the low level, there is **a set of sub-policies**, each sub-policy maintains a table of ***C-value*** to learn the assignment for the sub-policy to complete which task (selected from high-level). The C-value is associated to $(o_s,o_g)$ pairs, where $o_s$  is the start high-level observation and $o_g$  is the goal high-level observation. If one policy completed one task perfectly, then the C-value for this pair will increase but for other pairs will decrease.
3. For the low level, each sub-policy is learned by any standard RL algorithm, where the reward signal is given from the high level.

The main innovations of this algorithm is to use C-values to learn the associations of the sub-policies to the sub-tasks. In this case, the sub-policies can realize **specialization**, as some sub-policy is good at to reach sub-goal A, while another sub-policy is good at to reach sub-goal B; the sub-policies can also realize **generalization**, as a single sub-policy may also learn to encompass multiple adjacent pairs.

The algorithm adopts standard RL algorithms for each level. The paper also proposed an extended version to use LSTM (Long Short Term Memory) based method to deploy in POMDP models.

## Main Drawbacks

* The most important issue of the HASSLE algorithm is **how to define/generate abstract observation spaces for the high level**. Unfortunately, the paper didn't give any particular method, but only gave the general idea. For the experiments conducted by the authors, they used Adaptive Resource *Allocation Vector Quantization (ARAVQ)* method [1]. The HASSLE algorithm is very sensitive to the high-level observation spaces and defining them is usually a hard work.
* The algorithm lacks of strict convergence guarantee.
* The model has large number of parameters to learn.

## Reference

[1] Fredrik Linåker and Henrik Jacobsson. 2001. Mobile robot learning of delayed response tasks through event extraction: a solution to the road sign problem and beyond. In Proceedings of the 17th international joint conference on Artificial intelligence - Volume 2 (IJCAI’01), Morgan Kaufmann Publishers Inc., San Francisco, CA, USA, 777–782.
