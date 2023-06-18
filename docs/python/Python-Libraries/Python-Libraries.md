---
layout: default
title: Python Libraries
parent: Python
nav_order: 1
has_children: true
---

## Introduction
Usage of some python library will be documented here.

## Zetane Viewer
[github](https://github.com/zetane/viewer)

A free useful tool for visualization of NN feature layers. Just cool and interesting. 

Could debug simple network easily.


## Panel
[Website](https://panel.holoviz.org/getting_started/index.html)
```
pip install panel
```

This is a powerful visualization and interaction lib.

Could be used to build interactive app with/-out mathmatic diagram with parameter changable.

Could even be used to create a conversation interaction interface.


## Redlines
[github](https://github.com/houfu/Redlines)
```
pip install redlines
```

This lib provde function to compare two string and output a string with Markdown format showing the difference between the 2 inputs.

Could be used to create good visualization for comparsion.

```python
from redlines import Redlines

test = Redlines(
    "The quick brown fox jumps over the lazy dog.",
    "The quick brown fox walks past the lazy dog.",
)
assert (
        test.output_markdown
        == "The quick brown fox <del>jumps over </del><ins>walks past </ins>the lazy dog."
)

# Or

test = Redlines("The quick brown fox jumps over the lazy dog.")
assert (
        test.compare("The quick brown fox walks past the lazy dog.")
        == "The quick brown fox <del>jumps over </del><ins>walks past </ins>the lazy dog."
)
```