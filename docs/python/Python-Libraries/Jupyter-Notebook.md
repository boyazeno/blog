---
layout: default
title: Jupyter-Notebook
parent: Python Libraries
grand_parent: Python
nav_order: 2
has_children: false
---

# Jupyter-Notebook
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---
## Add your venv to jupyter notebook:
```
# 1. Activate your venv
source /path_to_your_venv/bin/activate
# 2. Install ipykernel
pip install ipykernel
# 3. Install your venv
python3 -m ipykernel install --user --name=venv
```

##