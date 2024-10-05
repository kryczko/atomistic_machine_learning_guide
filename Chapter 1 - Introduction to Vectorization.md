# Introduction to Vectorization

Python is slow. However, people figured out that libraries can be wrriten in C, and then used by calling Python code. To motivate why people wrote these C codes, let's consider the following Python Python list:
```python
a = [list(range(10)) for _ in range(10)]
# a = [[0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]]
```
which can be thought of as a 2-dimensional array. Let's say we wanted to compute the sum of all elements. To do this we write
```python
sum = 0
for row in a:
    for elem in row:
        sum += elem
# sum = 450
```
which means we have two loops. We must write an additional for every dimension we add to the array. Due to the nature of how Python functions, this becomes very slow. This also can become slow in C/C++, but not as slow as Python. We therefore want to have as many for loops written in C as possible. To do this, we leverage a library like `numpy`. These for loops are written under the hood of `numpy`, and we never have to deal with them. Consider the previous example:
```python
import numpy as np
a = [list(range(10)) for _ in range(10)]
a = np.array(a)
sum = a.sum()
# sum = 450
```
With one command we turned the 2 for loop code above to one simple line. Beautiful! Let's consider the case where we want to perform not a total sum over all elements, but the local sums over the individual lists in `a`. To do this without `numpy` looks something like:
```python
a = [list(range(10)) for _ in range(10)]
local_sums = []
for row in a:
    local_sum = 0
    for elem in row:
        local_sum += elem
    local_sums.append(local_sum)
# local_sums = [45, 45, 45, 45, 45, 45, 45, 45, 45, 45]
```
However, with `numpy` this becomes
```python
a = [list(range(10)) for _ in range(10)]
a = np.array(a)
local_sums = a.sum(axis=1)
# local_sims = array([45, 45, 45, 45, 45, 45, 45, 45, 45, 45])
```
Again, one line! Another advantage about using vectorization in `numpy` is multiplying matrices. Let's take the previus list of lists `a`, and multiply it by itself, or compute $\text{a}^2$. Proceeding without `numpy`:
```python
a = [list(range(10)) for _ in range(10)]
a_2 = []
for row in a:
    row_2 = []
    for elem in row:
        row_2.append(elem**2)
    a_2.append(row_2)
# a_2 = [[0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]]
```
With `numpy`, it yields another one-liner:
```python
a = [list(range(10)) for _ in range(10)]
a = np.array(a)
a_2 = a * a # or a**2
""" a_2 = [array([[ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81],
       [ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81],
       [ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81],
       [ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81],
       [ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81],
       [ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81],
       [ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81],
       [ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81],
       [ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81],
       [ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81]])
"""
```
This doesn't even scratch the surface of what capabilities are built into `numpy`, but gives you an idea of the power behind it. Happy hiking!
