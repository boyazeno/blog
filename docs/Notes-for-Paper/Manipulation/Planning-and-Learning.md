---
layout: default
title: Planning and Learning
parent: Manipulation
grand_parent: Notes for Paper
nav_order: 1
has_children: false
---
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>
# Planning and Learning
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

# Self-supervised Reinforcement Learning with Independently Controllable Subgoals **CoRL2021**
<div markdown="1">
NN
{: .label .label-blue }  
CODE-AVAILABLE
{: .label .label-blue }
</div>

[Link to paper](https://arxiv.org/pdf/2109.04150.pdf)

[Link to github]()

## Background
* Object-centric representation of the environments, each component corresponds to an object state.
* Hierarchical Rein-forcement Learning (HRL) ignore the potential interference between sub-tasks. (Non-compatible)
* Reformulating the agent’s subtasks and the corresponding reward signals. Motivate the robot to control components independently from other components.

* Introduce `selectivity reward signal` and `interaction graph` for choosing independ components.
* Estimate `interaction graph` using data drived RGNN.
* Learn `goal-directed selectivity reward function` to identify independ components for task.
* Provides SRICS alg to learn independently controllable subtask and decompose complex task.

## Representations
object-centric representations for state space S is the product of all object subspaces:

$$S=S^{1}×...×S^{K}$$

And each state contains position and identifier: $$s^{j, where}, s^{j, what}$$.

The Choose of goal space is crucial due to the independent subgoal requirements (define goal in a independ way is easier for learning independ control).




## Thinking
1. Sounds like a way to find the meta-task (eigen-task??). Maybe not literally independent, but could be clearly divided.

2. 

---

# Planning from Pixels in Environments with Combinatorially Hard Search Spaces **2021**
<div markdown="1">
NN
{: .label .label-blue }  
CODE-AVAILABLE
{: .label .label-blue }
PATH-PLANNING
{: .label .label-blue }  
TASK-PLANNING
{: .label .label-blue } 
</div>

[Link to paper](https://arxiv.org/pdf/2110.06149.pdf)

[Link to github](https://github.com/martius-lab/PPGS)

## What's new
* Use a model learn graph-based representation of real-world input (instead of using continuous & raw representation like in most RL).
* Use the learned graph do planning, take acceptable time.

## How
![cppf_voting](/blog/assets/PPGS_model.png){: width="600" }
* Define world as MDP.
* Use `conv` extract features -> hidden state $$z_t$$ -> latent space.
* Use **forward model** to get next hidden state from current state and action: $$z_{t+1}=f(a_t, z_t)$$ 
* Use **inverse model** to prevent model collapse: get possible action based on $$z_1$$ and $$z_2$$.
* Check if state is the same by compare `distance` of 2 latent spaces. Introduce margin loss $$L$$ aims to make $$z_t$$ and $$z_{t+1}$$ as different as possible. Help differ states. (doubt)
* Directly use bfs search on latent space.

## Thinking
* Continue -> discret representation seems to be easy learned and used.
* Graph representation could be connected with semantic and high level features
* What could the popular semantic describtion in rgb image do with this? Some real time graph building? Like SLAM based scan?
* How to make sure the latent state is distributed in a " uniformly" in latent space? The different between each states could be small. 
* Use traditional searching method on a "seems like continuous" space. 
* Circle like self checking makes things happened.


---

# Risk-Averse Zero-Order Trajectory Optimization **CoRL2021**
<div markdown="1">
TRAJECTORY-OPTIMIZATION
{: .label .label-blue }  
ALEATORIC-EPISTEMIC
{: .label .label-blue } 
</div>

[Link to paper](https://openreview.net/pdf?id=WqUl7sNkDre)

## Why?
* In sequential discision-making problem, data-driven method hard to train. And it's kind of anti-human intention
* Traditional MPC use higher order info of trajectory = high time consuming -> not easy for real time 
* Model based method is predictable/controlable in sense of uncertainty.

## What's new?
* Considering difference between uncertainty caused by **system noise** and **lack of knowledge**
* Compare with PETS method

## How?

## Thinking


---

# Deep Reinforcement Learning in a Handful of Trials using Probabilistic Dynamics Models (PE-TS)**2018**
<div markdown="1">
TRAJECTORY-OPTIMIZATION
{: .label .label-blue }  
ALEATORIC-EPISTEMIC
{: .label .label-blue }  
MDP
{: .label .label-blue }  
</div>

[Link to paper](https://arxiv.org/abs/1805.12114)

## Why?
* Model based RL could easily outperform Data-based RL with fewer samples.
* Data based RL trend to convergent to better level compare to model based RL
* 2 types of uncertainty is imporant for RL: Aleatoric(noise), Epistemic(recognition)
* GP process don't fit well with large data
* Learning one time, should be able to plan multiple times.

## What's new?
* Combine both method
* Propose probabilistic ensembles with trajectory sampling(PETS)
* Get uncertainty representation from ensembled dynamic model
* Learning transfer function of MDP. Use MPC to find the best action (w.r.t. accumlated reward) given current state and transfer function within T steps. 
* During MPC calculating best action, use sampling method to get the best predicted action sequence. Also use [CEM](https://www.researchgate.net/publication/279242256_Chapter_3_The_Cross-Entropy_Method_for_Optimization) to constrain action sample range.  
* Result: Prob Ensamble>Prob>Determi Ensamble>Determi

## How?
### Learning Aleatoric & Epistemic
Aleatoric uncertainty is caused dy the system noise, etc, which exist inside sampled data. It can be learned by NN model directly: **NN learning parameter of distribution, but NN could still be deterministic**.

Epistemic uncertainty is exist due to ***the lack of sufficient data to uniquely determine the underlying system exactly***. It can be learned by "ensemble" multiple networks. 

Author defined 3 different NN (since the model selection for learning world model is critic):

**Probabilistic neural networks (P):**

This is a very normal deterministic NN, but named as "probabilistic NN", because it learns the parameters to describe a distribution. 
If we model the transfer function in MDP as an gaussian distribution (means for each state-action pair $$s_{t},a_{t}$$, the next state $$s_{t+1}$$ is determined by a specific gauss distribution): 

$$L(\theta)=-\sum_{n=1}^{N}log\tilde{f_\theta} (s_{n+1}|s_{n},a_{n} )$$

, where the distribution could be gauss: 

$$f=Pr(s_{n+1}|s_{n},a_{n})=N(\mu_\theta (s_{n},a_{n}), \Sigma_\theta (s_{n},a_{n}))$$

***Problem: For out-of-distribution input, the output value could be arbitrary***


**Deterministic neural networks (D):**

This is the same as above, just replace the output from learning the distribution to learning the exact next state. This actually could be understand as a special case of (P) using delta distribution, but not using gauss distribution.


**Ensembles (DE and PE):**

Use several deterministic/probabilistic NN with same base model, but just trained with different subset of samples. The final distribution are denoted by average of each model $$\tilde{f_\theta} = \frac{1}{B}\sum_{b=1}^{B}\tilde{f_\theta}_b$$, where $$B$$ is the number of ensembled models. The result could look like this:

![cppf_voting](/blog/assets/planning-and-learning-ensumble.png){: width="300" }

### Trajectory Sampling:
They defined B (=5) ensambled models (trained with different part of the data) and P (=20) samples.
At begin of each trial, P number of particals will be initialized. The next state of each of them will be sampled by one of the $$\tilde{f}_{\theta_{b(p,t)}}(s_t^P,a_t^P)$$ sampled from ensembled-NNs. All samples will be sampled with the same action $$a_t$$, but with world_predication function from different ensambled models.

Author introduced 2 types of sampling method:
> TS1 refers to particles uniformly re-sampling a bootstrap per time step.

In TS1, at each new time step, each partical will rechoose a ensemble for sampling next state (regardless which ensemble-NN was used before).

> TS∞ refers to particle bootstraps never changing during a trial.

In TS∞, since the first time sampling, the ensemble-NN for each partical is fixed. That means, every state transfer for each partical is only using the same f.

> An advantage of using TS∞ is that aleatoric and epistemic uncertainties are separable

Because each partical are separated.

### Baseline:
For comparison with TS method, three other method are method: 
> Expectation(E): This simply use the average value of $$s_{t+1}$$ from all samples as the predicted next state.
> Moment matching(MM): Each sample will still use different model b and same action $$a_t$$ to make thier own prediction of next state. An approximated normal distribution will be matched using predictions from all samples. The final next state for each sample p will be resampled from this distribution.
> Distribution sampling(DS): Since MM approximate distribution of next state with only one peak (too strict), DS approximate distribution of next state using sample predictions $$s_{t+1}^{p}$$ for each ensamble b. 

### CEM:
CEM here is used as action sampler.

Core idea of CEM for optimization is: **find the best matching proxi-distribution w.r.t. target-distribution through some stoastic process.** The comparison uses cross-entropy. But the target-distribution is unknown. So sampling method are used to get a blick of the target-distribution.

Main step consists: (link)[The Cross-Entropy Method A Unified Approach to Combinatorial Optimization, Monte-Carlo Simulation, and Machine Learning ]
1. Use certain strategy generate samples.(e.g. guass distribution)
2. Calculate and rank the target value (through trial)
3. Get the top k values and its corresponded samples, update parameters of strategy. (Recalculate  $$\mu$$ and $$\sigma$$).
4. Redo 1-3 until $$\sigma$$ is small enough.(for optimzation)

## Thinking