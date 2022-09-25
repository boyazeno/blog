---
layout: default
title: cnn
parent: NN
grand_parent: Notes for Paper
nav_order: 2
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

# Scaling Up Your Kernels to 31x31: Revisiting Large Kernel Design in CNNs **CVPR2022**
<div markdown="1">
CNN
{: .label .label-blue }  
INDUCTIVE-BIAS
{: .label .label-blue }
VIT
{: .label .label-blue }
LARGE-KERNEL-SIZE
{: .label .label-blue }
CODE-AVAILABLE
{: .label .label-blue }
</div>

[Link to paper](https://arxiv.org/pdf/2203.06717.pdf)
[Link to code](https://github.com/DingXiaoH/RepLKNet-pytorch)

## Why
* People believe the stack of small kernels could bring same receptive field as large kernel, but with smaller number of parameters.  

* ViT model not only improved score on Image-Net a lot, but also has a strong performance in downstream tasks (segmentation, detection). 

Previous works show that the key point is not because of self-attention (replaced self-attention module with other module, still get good performance).

* Although resnet could be used to build very deep model, but according to the experiment, the different between 150+ and 100+ are not big. Because most of the gradient go through shortcut, the main layer are not activated. (means the receptive field are not become larger.) 

## What's new
* Shows that using large kernel size and reparam technique is even more efficient than smaller.
* Proposed a structure with  Transformer with larger kernel size (31*31), which achieved equal/better result on ImageNet, and faster time in downstream tasks.  

## How
* 

## Discussion:
* Representation learning: Key point is learning the **invariance**. The chosen feature should have low mutual information with input data -> means the feature should be sparse enough. But it's hard to learn disentangled/global representation independend from downstream task. Important to learn the common point of the tasks (occolusion invariance, translation invariance, scaling invariance, etc.) 


## Thinking


