---
layout: default
title: PointCloud Feature Extraction
parent: Feature Extraction
grand_parent: Notes for Paper
nav_order: 1
has_children: false
---
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>
# Feature Extraction
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

# General
## Introduction
In this blog, only paper related to 3d feature extraction will be listed. (On going)


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
# PointNet++: Deep Hierarchical Feature Learning on Point Sets in a Metric Space **NIPS2017**
<div markdown="1">
NN
{: .label .label-blue }  
HIERARCHICAL-FEATURE
{: .label .label-blue }
POINTCLOUD
{: .label .label-blue }
CODE-AVAILABLE
{: .label .label-blue }
</div>
[Link to paper](https://arxiv.org/pdf/1612.00593.pdf) PointNet

[Link to paper](https://arxiv.org/pdf/1706.02413.pdf) PointNet++

[Link to github](https://github.com/charlesq34/pointnet2)


## Features
* \[PointNet\] The most common used 3d feature extraction network with pointcloud as input, different level features as output.
* \[PointNet\] Keep transformation invariant by learning mini-transformation through each T-net.
* \[PointNet\] Solve input points order issue by using "symmtric function" style, which means use NN learn pointwise features (keep points order unchanged).
* \[PointNet\] Get global feature directly from local features using max pool
* \[PointNet\] Combine local and global by point-wise appendence


# PRIN/SPRIN: On Extracting Point-wise Rotation Invariant Features **2021**
<div markdown="1">
NN
{: .label .label-blue }  
ROTATION-INVARIANT
{: .label .label-blue }
POINTCLOUD
{: .label .label-blue }
CODE-AVAILABLE
{: .label .label-blue }
</div>

[Link to paper](https://arxiv.org/pdf/2102.12093.pdf)

[Link to github](https://github.com/qq456cvb/CPPF)

## Features
* Rotation Invariance