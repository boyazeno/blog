---
layout: default
title: Trimesh
parent: Python Libraries
grand_parent: Python
nav_order: 3
has_children: false
---

# Trimesh
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---
## Speed up raycasting by installing pyembree:
```
pip install pyembree
```

## Do ray-mesh intersection:
Could speed up with pyembree installed
```python
import trimesh
import numpy as np

mesh = trimesh.load_mesh('obj.ply')
ray_origins = np.random.random(10,3)
ray_directions = np.random.random(10,3)
locations, index_ray, index_tri = mesh.ray.intersects_location(ray_origins=ray_origins, ray_directions=ray_directions, multiple_hits=False)
```
