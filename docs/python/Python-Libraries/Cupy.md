---
layout: default
title: Cupy
parent: Python Libraries
grand_parent: Python
nav_order: 1
has_children: false
---

# Cupy
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---
## Introduction

In short words, cupy is a gpu version of numpy which provide parallel calculation for matirx.

The cupy api are the same as numpy, just some rare used methods are not implemented.

For large matrix calculation, cupy could only take several orders of magnitude less time than numpy. But only when the matrix are large enough.

[Official basic tutorial](https://docs.cupy.dev/en/stable/user_guide/basic.html)

[Official customize kernal tutorial](https://docs.cupy.dev/en/stable/user_guide/kernel.html)

## Installation

```
# For CUDA 10.2
pip install cupy-cuda102

# For CUDA 11.0
pip install cupy-cuda110

# For CUDA 11.1
pip install cupy-cuda111

# For CUDA 11.2 ~ 11.x
pip install cupy-cuda11x

# For AMD ROCm 4.3
pip install cupy-rocm-4-3

# For AMD ROCm 5.0
pip install cupy-rocm-5-0
```
---

## Tipps
* Define array context
```python
with cp.cuda.Device(0):
    x_gpu_0 = cp.ndarray([1, 2, 3]) 
```
* Could be used for CPU/GPU agnostic programming
```python
# Get the module of array x: numpy or cupy
xp = cp.get_array_module(x)
xp.maximum(0, x)
```




