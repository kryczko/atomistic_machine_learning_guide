# Introduction to Pytorch

In the previous chapter, we saw that `numpy` allows us to perform mathematical operations easily and avoid slow loops. Pytorch builds on this to allow these mathematical computations to happen on different devices, such as CPUs and GPUs. The interface looks very similar to `numpy`; however, instead of using array (`np.array`) objects, we call them tensor (`torch.tensor`) objects. A tensor is simply a high-dimensional array. These high-dimensional arrays can be constructed from either python lists or `numpy` arrays:
```python
import numpy as np
import torch
a = [list(range(10)) for _ in range(10)]
ta = torch.tensor(a)
print(ta.sum())
# sum = 450

# or we can write
ta = torch.from_numpy(np.array(a))
print(ta.sum())
# sum = 450
```
