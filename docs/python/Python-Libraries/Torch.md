---
layout: default
title: Torch
parent: Python Libraries
grand_parent: Python
nav_order: 5
has_children: false
---

# Torch
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---
# Tipps:

## Reparameterization/Sampling:

There are 2 methods for sampling (e.g. next action) out of a given distribution:
### Construct a distribution with parameter directly and sampling from it:
* Way of doing:
```python
probs = policy_network(state)
# Note that this is equivalent to what used to be called multinomial
m = Categorical(probs)
action = m.sample()
```
* This works when the policy is differentiable, so that it could back propagate.
### Use a fixed distribution term plus some modification (schifting/scaling) using parameters and then sample from it:





