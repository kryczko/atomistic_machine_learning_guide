# Introduction to Pytorch

In the previous chapter, we saw that `numpy` allows us to perform mathematical operations easily and avoid slow loops. Pytorch builds on this to allow these mathematical computations to happen on different devices, such as CPUs and GPUs. The interface looks very similar to `numpy`; however, instead of using array (`np.array`) objects, we call them tensor (`torch.tensor`) objects. A tensor is simply a high-dimensional array. These high-dimensional arrays can be constructed from either python lists or `numpy` arrays:

