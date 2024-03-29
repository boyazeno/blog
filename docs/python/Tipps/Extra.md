---
layout: default
title: Extra
parent: Tipps
grand_parent: Python
nav_order: 2
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


## How to get matrix split
The index at each dim could be a matrix, e.g. `B_idx`,`C_idx`. 

They should be broadcast-able between index matrix for each dimension: `B_idx` and `C_idx` should be broadcast-able, but the size itself has no relation with `orig_mat`. Only the index value for `B_idx` and `C_idx` should less than dim size for queried matrix which is `orig_mat` here.

By each dim, using notation `:` means keep the whole dimension, which essntially means add the whole dim of orig mat to the result mat.

**Attention**
If mix basic indexing and advance indexing, there is a detail to be awared.
> The advanced indices are separated by a slice, Ellipsis or newaxis. For example x[arr1, :, arr2].
> The advanced indices are all next to each other. For example x[..., arr1, arr2, :] but not x[arr1, :, 1] since 1 is an advanced index in this regard. [source](https://numpy.org/devdocs/user/basics.indexing.html#combining-advanced-and-basic-indexing)

For the spearated case, dimension from `:, ..., newaxis` will be appended at the end with order.

For the combined case, dimension from `:, ..., newaxis` will stay at the place where it should be.dimem

(See below)

```python
import torch

B,C,F,N = 1,2,3,4

orig_mat = torch.arange(B*C*F*N).view(B,C,F,N)

# new_mat:
nB, nC, nS, nN = 5,6,7,8
B_idx = torch.randint(0, B, (nB, 1, 1, 1))
C_idx = torch.randint(0, C, (1, nC, nS, 1))
F_idx = torch.randint(0, F, (1, nC, 1, nN))
N_idx = torch.randint(0, N, (1, 1, 1, nN))

print(orig_mat[B_idx, C_idx,F_idx, N_idx].shape) # nB*nC*nS*nN, only advanced indexing
print(orig_mat[B_idx, C_idx, F_idx,:].shape) # nB*nC*nS*1*N, advanced indexing all together
print(orig_mat[:, :, F_idx, N_idx].shape) # B*C*1*nC*1*nN, advanced indexing all together
print(orig_mat[B_idx, C_idx, :, N_idx].shape) # nB*nC*nS*1*F, advanced indexing separated by basic indexing
print(orig_mat[B_idx, :, F_idx, N_idx].shape) # nB*1*1*nN*C, advanced indexing separated by basic indexing
```

## How to add method function to a class outside defination
Use `types.MethodType(func, instance)`, this could add functions with `self` argument.

```python
import types

def say_hello(self):
    print(f"Hello, {self.name}!")

class Greeter:
    def __init__(self, name):
        self.name = name

greeter = Greeter("Sir Robin")
greeter.greet = types.MethodType(say_hello, greeter)
greeter.greet() # Prints "Hello, Sir Robin!"
```
[reference](https://stackoverflow.com/questions/70538009/how-do-you-attach-a-function-to-an-instance-of-a-class)


## How to convert between dict and Namespace in argparse
```python
import argparse
# To namespace
my_dict = {"param":"value"}
ns = argparse.Namespace(**my_dict)

# To dict
my_dict = vars(ns)
```


## What is each type in custom named array?
The shortened string format codes may seem confusing, but they are built on simple principles. **The first (optional) character is < or >, which means "little endian" or "big endian," respectively**, and specifies the ordering convention for significant bits. The next character specifies the type of data: characters, bytes, ints, floating points, and so on (see the table below). The last character or characters represents the size of the object in bytes.

```
Character	Description	Example:
 'b'	Byte	np.dtype('b')
 'i'	Signed integer	np.dtype('i4') == np.int32
 'u'	Unsigned integer	np.dtype('u1') == np.uint8
 'f'	Floating point	np.dtype('f8') == np.int64
 'c'	Complex floating point	np.dtype('c16') == np.complex128
 'S', 'a'	String	np.dtype('S5')
 'U'	Unicode string	np.dtype('U') == np.str_
 'V'	Raw data (void)	np.dtype('V') == np.void
```
[link](https://jakevdp.github.io/PythonDataScienceHandbook/02.09-structured-data-numpy.html)