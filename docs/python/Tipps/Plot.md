---
layout: default
title: Plot
parent: Tipps
grand_parent: Python
nav_order: 1
has_children: false
---

# Plot Tipps
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## How to plot margin (band area) around a curve?
![cppf_voting](/blog/assets/python-tipps-plot-margin.png){: width="400" }

```python
import matplotlib.pyplot as plt
import numpy as np

f,ax = plt.subplots(1,1)
x=np.arange(0,10,0.1)
y = np.sin(x)
ax.fill_between(x,y*0.9,y*1.1,alpha=0.1)
ax.plot(x,y)
plt.show()

```
