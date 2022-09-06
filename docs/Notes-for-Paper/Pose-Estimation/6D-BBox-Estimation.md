---
layout: default
title: 6D BBox Estimation
parent: Pose Estimation
grand_parent: Notes for Paper
nav_order: 1
has_children: false
---
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>
# A Small Example
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

# General
## Introduction
In this blog, only paper related to 6D bounding box estimation will be listed. (On going)


## Methods


---
# Dataset

## SUN RGB-D
[Link](https://rgbd.cs.princeton.edu/)
* 10335 RGBD images
* 2D/3D bbox
* semantic segmentation
* scene classification
* For scene-understanding tasks

## 

---
# CPPF: Towards Robust Category-Level 9D Pose Estimation in the Wild **CVPR2022**
<div markdown="1">
NN
{: .label .label-blue }  
PPF
{: .label .label-blue }
VOTE
{: .label .label-blue }
SIM-2-REAL
{: .label .label-blue }
CATEGORY-LEVEL
{: .label .label-blue }
CODE-AVAILABLE
{: .label .label-blue }
</div>

[Link to paper](https://openaccess.thecvf.com/content/CVPR2022/papers/You_CPPF_Towards_Robust_Category-Level_9D_Pose_Estimation_in_the_Wild_CVPR_2022_paper.pdf)
[Link to github](https://github.com/qq456cvb/CPPF)


Remove ambiguity in orientation voting by: 

coarse-to-fine algorithm to eliminate noisy point pairs
## Features
* Learning from ppf method, try to predict point-pair-feature with size = 5: $${u, b,\alpha, \beta, \gamma}$$, given point-pair and thier neighbours.
* Use voting result for center and orientation as origin ppf. 
* Eliminate noise by first voting center. Resample point-pairs which vote to the same center. Use the new samples for orientation voting.
* Use PRIN/SPRIN for feature extraction.
* Concatenate new features and origin ppf features as MLP input.


## Voting Design
### Voting for center
For each point-pair, user use length $$u$$ and $$v$$ as features as shown below. (C is center, P1-P2 is feature pair) As long as enough number of point-pairs are found, the center of the object could be voted.

This expression naturely suitable for symmtric. 
![cppf_voting](/blog/assets/cppf_voting.png){: width="400" }

### Voting for orientation
For orientation, the author use the angle $${\alpha, \beta}$$ bewteen point-pair connecting-line and upper/right direction $$e1$$,$$e2$$ as feature. The same voting will be used as center to vote for $$\alph$$ (since the ambiguous region is a cone). $$\beta$$ could be calculated also as 
![cppf_voting](/blog/assets/cppf_orientation_voting.png){: width="400" }

### Voting for scale
For scale, the author use prior information (class level average scale $$\bar{S}$$) and averaged scale $$\gamma$$ from all point-pairs to estimate scale:

$$\hat{S}=e^{\gamma}*\bar{S}$$

---
# NOCS: 
<div markdown="1">
NN
{: .label .label-blue }  
PPF
{: .label .label-blue }
</div>

[Link to paper](https://openaccess.thecvf.com/content/CVPR2022/papers/You_CPPF_Towards_Robust_Category-Level_9D_Pose_Estimation_in_the_Wild_CVPR_2022_paper.pdf)
