---
layout: default
title: "PARALLEL Q-LEARNING: SCALING OFF-POLICY REINFORCEMENT LEARNING"
parent: RL
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

# PARALLEL Q-LEARNING: SCALING OFF-POLICY REINFORCEMENT LEARNING **ICLR2023**
<div markdown="1">
RL
{: .label .label-blue }  
Q-Learning
{: .label .label-blue }
Data-Parallel
{: .label .label-blue }
Sample-Efficiency
{: .label .label-blue }
</div>

[Link to paper](https://openreview.net/pdf?id=vFvw8EzQNLy)

## Background
* On-policy e.g. PPO could be speed up due to parallelzation with multiple agents and running on GPU
* Off-policy has higher sample efficiency but not parallelized.
* Popular RL framework run env upate, value update, policy update in sequence

## What's new
* Proposed Parallel Q-Learning: higher sample efficiency
* parallelize data collection / pi learning / V function learning  
* Compared hyper parameter affect: batch size, num parallel env, exploration schemes, etc.

## Concept:
###  Asynchronous SGD:
* Split batch into mini-batch -> GPU[0-n]
* Each GPU update model with it's calculated gradient directly after calculation

Problem:
* Affect the precision of the network, since the updates will happens after some batch has been updated
   

## How
### Key components:
- policy function
- Q value function
- env

* Save a replay-buffer at each learner -> reduce comunication 
* Asynchronize the data/network transfer between actor<->v learner<->p learner (use Ray for parallelization)
* Throttle the relative updating frequency (add extra hyper parameters), try to exploit the time (but convergence performance better?)
* Add noise with step-wise amplitude to action command, save fine tune time for sigma.


## Thinking
1. Why N num of env matters? (matters if the update of Q is good enough or not? Overfitting of policy should encourage exploration or not?) If it try to exploit GPU usage, smaller N => higher update times for fixed amount of value. (the update per wall-clock time should not change too much, means performance should be the same?)
2. Compare mainly on wall-clock time but not steps.
3. Large batch size good for learning.
3. Good paper for engineering

## Summary:
* 8 times update for Q value per action update is good.
* Exploration with different noise level for each env ($$a_{real}=a_{calc}+(0.2,0.4,0.8,1.0)*\sigma$$) during data collection is good. 


