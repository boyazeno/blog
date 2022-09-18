---
layout: default
title: Continuous-Learning
parent: NN
grand_parent: Notes for Paper
nav_order: 1
has_children: false
---
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>
# Continuous Learning
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

# Brain-inspired replay for continual learning with artificial neural networks **Nature2020**
<div markdown="1">
NN
{: .label .label-blue }  
BRAIN-INSPIRED
{: .label .label-blue }
CONTINUOUS-LEARNING
{: .label .label-blue }
CODE-AVAILABLE
{: .label .label-blue }
</div>

[Link to paper](https://www.nature.com/articles/s41467-020-17866-2)
[Link to code](https://github.com/GMvandeVen/brain-inspired-replay)

## Why
ANN normally trained with data from multiple stored examples. But it's could forget old example if trained only on new data again without using the old data at the same time. This makes the continuous learning hard. And always stores data could cause privacy issue, etc.

Experiments with human brain show that "the reactivation of neuronal activity patterns" from previous experiences is important for stabilizing new memories. This "replay" could be used as tool for continual learning in ANN. 

Class-incremental learning (Class-IL) fails even in simple tasks. It cannot distinguish between classes learned from different learning episodes. Generative replay (GR) could solve the problem. But upscaling the problem is hard for GR. (only suitable for simple input and less tasks)

## What's new
* Use a learned generative neural network model for replaying past observations.
* Proposed a new variant of GR using internal or hidden representations for replaying without storing data.
* Experiments shows that pure GR suffering from low quality generation or huge cost on training, which affects continuous learning result.


## How
* Use a standard variational autoencoder (VAE) as generator in the first place. Trying to figure out how it works and what is the possible limitation of it.
* For compariation, the author also tried regularization methods: elastic weight consolidation (EWC) and synaptic intelligence (SI). Suppose to estimate the influence of each tasks for each parameters. And punishment will be added to more influenced parameters when training on other tasks. 

  ![training and inference](/blog/assets/nn-continuous-learning-brain-inspired-nature.png){: width="600" }
  
  During retraining, only a fixed number of samples are taken from generative model. Meaning there is no need to keep the training dataset the same size.
  It is possible to train generative model with low quality, due to the fact that "learning new things is harder than not forget", which is proved by experiments.
  Experiment result shows that only RG method prevent from forgetting issue in simple scenario. But with complexer input, GR get very bad result due to generate  model output worse images, which inspired the hidden state representation.
* Modification based on standard VAE:
  ![training and inference](/blog/assets/nn-continuous-learning-brain-inspired-nature-bir.png){: width="800" }
  1. Modification 1: Replay-through-feedback
  Instead of use 2 network for model and generative model, now use one symmtric VAE for both. This is inspired by human brain. The first few layers are fixed, the rest are shared between generative model and inference model.
  2. Modification 2: Conditional-replay
  Instead of use the same normal distribution, now we learn a Gaussian mixture model with a normal distribution for each class as encoder output.
  3. Modification 3: Gating based on internal context
  Instead of using all parameters for each class/task, now for each class during generative phase, a random selected group of parameters will be deactivated (activation set to 0).
  4. Modification 4: Internal replay
  Instead of generate image, now in generative model, the generation will happens at fully connection layer (won't reach the last layer), with the purpose of getting a simpler features.

According to the experiment result:
1. Store memory in functions is better than as representations.
2. Internal replay appeared to be the most influential modification.
3. Different modifications were complementary to each other. 

## Thinking
* 2 types of continuous learning problem: 1. more labelled data from the same training dataset are generated; 2. more types of classes need to be classifed. How to handle if the output of the network need to be changed?

* Memory using GR in RL might looks like a differentialable physical simulation/rendering engine?