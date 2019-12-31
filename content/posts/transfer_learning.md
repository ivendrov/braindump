+++
title = "Transfer Learning"
author = ["Jethro Kuan"]
lastmod = 2019-12-26T21:37:28+08:00
draft = false
math = true
+++

Prior understanding of problem structure can help us solve complex
tasks quickly. Perhaps solving prior tasks would help acquire useful
knowledge for solving a new task.

Transfer learning is the _use of experience from one set of tasks for
faster learning and better performance on a new task_.


## Few-Shot Learning {#few-shot-learning}

"shot" refers to the number of attempts in the target domain. For
example, in 0-shot learning, a policy trained in the source domain
works in the target domain.

This typically requires assumptions of similarity between source and
target domain.


## Taxonomy of Transfer Learning {#taxonomy-of-transfer-learning}

"forward" transfer
: train on one task, transfer to a new task
    -   Randomizing source domain
    -   Fine-tuning

multi-task transfer
: train on many tasks, transfer to a new task
    -   generate highly randomized source domains
    -   model-based RL
    -   model distillation
    -   contextual policies
    -   modular policy networks

multi-task meta-learning
: learn to learn from many tasks
    -   RNN-based/Gradient-based meta-learning


## Forward Transfer {#forward-transfer}


### Fine-tuning {#fine-tuning}

_Key Idea_: Train on the source task, then train some more on the target
task, for example, by retraining the weights on the last layer. Lower
layers are likely to learn representations from the source task that
are useful in the target task. This works well if the source task is
broad and diverse.

Fine tuning is popular in the supervised learning setting.

-    Why fine-tuning doesn't work well for RL

    1.  RL tasks tend to be narrow (not broad and diverse), and features
        are less general
    2.  RL methods tend to learn deterministic policies, the policies that
        are optimal in the fully-observed MDP.
        1.  Low-entropy policies adapt very slowly to new settings
        2.  Little exploration at convergence

    To increase diversity and entropy, we can do maximum-entropy learning
    which acts as randomly as possible while collecting high rewards <a id="1b37e467d7dc76e365875dfb5c03fa1e" href="#pmlr-v70-haarnoja17a" title="Tuomas Haarnoja, Haoran Tang, Pieter Abbeel \&amp; Sergey Levine, Reinforcement Learning with Deep Energy-Based Policies, 1352--1361, in in: {Proceedings of the 34th International Conference on Machine Learning}, edited by Doina Precup \&amp; Yee Whye Teh, PMLR (2017)">(Tuomas Haarnoja et al., 2017)</a>:

    \begin{equation}
      \pi(a|s) = \mathrm{exp} (Q\_\phi(s,a)-V(s))
    \end{equation}

    This optimizes

    \begin{equation}
      \sum\_t E\_{\pi(s\_t, a\_t)}[r(s\_t, a\_t)] + E\_{\pi(s\_t)}[\mathcal{H}(\pi(a\_t|s\_t))]
    \end{equation}


### Manipulating the Source Domain {#manipulating-the-source-domain}

This is used where we can design the source domain (e.g. training in a
simulator, which can be tweaked). Injecting randomness/diversity in
the source tends to be helpful.

Resources:

-   [Domain Randomization for Sim2Real Transfer](https://lilianweng.github.io/lil-log/2019/05/05/domain-randomization.html)


## Multi-task Transfer {#multi-task-transfer}

Some things remain constant across tasks, such as the laws of physics.
Model-based RL may learn these laws, and transfer this knowledge
across multiple tasks.

In model distillation, multiple policies are combined into one, for
concurrent multi-task learning. Policy distillation
<a id="6c0f3e1b8610e021d4a59b9c6b7598dd" href="#rusu15_polic_distil" title="Rusu, Colmenarejo, , Gulcehre, Desjardins, , Kirkpatrick, Pascanu, Mnih, Volodymyr, Kavukcuoglu \&amp; Hadsell, Policy Distillation, {CoRR}, v(), (2015).">(Rusu et al., 2015)</a> is able to:

1.  Compress policies learnt on single games into smaller models
2.  Build agents capable of playing multiple games
3.  Improve the stability of the [DQN learning algorithm]({{< relref "q_learning" >}}) by distilling
    online the policy of the best performing agent

# Bibliography
<a id="pmlr-v70-haarnoja17a"></a>Haarnoja, T., Tang, H., Abbeel, P., & Levine, S., *Reinforcement learning with deep energy-based policies*, In D. Precup, & Y. W. Teh, Proceedings of the 34th International Conference on Machine Learning (pp. 1352–1361) (2017). International Convention Centre, Sydney, Australia: PMLR. [↩](#1b37e467d7dc76e365875dfb5c03fa1e)

<a id="rusu15_polic_distil"></a>Rusu, A. A., Colmenarejo, S. G., Gulcehre, C., Desjardins, G., Kirkpatrick, J., Pascanu, R., Mnih, V., …, *Policy Distillation*, CoRR, *()*,  (2015).  [↩](#6c0f3e1b8610e021d4a59b9c6b7598dd)