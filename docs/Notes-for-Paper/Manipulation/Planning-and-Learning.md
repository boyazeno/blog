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