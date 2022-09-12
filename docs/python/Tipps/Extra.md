---
layout: default
title: Extra
parent: Tipps
grand_parent: Python
nav_order: 1
has_children: false
---

# A Small Example
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## How to use abstract function
```python
import abc
class A(abc.ABC):
  def __init__(self):
    pass
  @abc.abstractmethod
  def hallo(self):
    pass

```
