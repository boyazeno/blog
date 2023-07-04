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
* 
* 

![cppf_voting](/blog/assets/PPGS_model.png){: width="600" }
## Representations


## Thinking
1. 
2. 






