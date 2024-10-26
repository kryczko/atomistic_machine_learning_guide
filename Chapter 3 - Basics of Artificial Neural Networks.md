# Basics of Neural Networks

There are many resources out there that provide details of artificial neural networks (ANNs), so I will keep things brief. ANNs are inspired by neurons in the brain. A Neuron receives some input and based on that input will turn on or off. Neurons have connections with many others, and therefore the action of turning on a single neuron may trigger another, creating a cascading, collective effect that inevitably leads to a final result. Instead of considering a binary case of neurons turning on and off, ANNs work on floating point values. If the problem we're trying to solve is classification, we must convert the final output to binary values (i.e. 1s and 0s). However, most cases in atomistic machine learning are regression problems. ANNs consist of `layers` and in each layer, the inputs get multiplied against the `weights` for that layer, a `bias` is added, and a non-linear `activation` function is applied. The activation function is the necessary ingredient to learn non-linear mappings.

Let's see how we can program a single neuron in `pytorch`. Let's start with a high dimensional vector `x`, which we can define in as
```python
import torch
x = torch.rand(1, 10) # a single vector with dimension 10
```
This input, `x,` will be the input into the neuron. The output of a single neuron is one floating-point value, which means we will need 10 weights to multiply against. In addition, a single bias value is defined for each layer (which is just a single neuron at this point). The computation then becomes
```python
weights = torch.rand(10, 1, requires_grad=True) # add a requires_grad here so we can modify the weights!
bias = torch.rand(1, requires_grad=True)
output = x @ w + bias
```
To add in the non-linearity, we need an activation function. There are many variants, but let's use [SiLU](https://pytorch.org/docs/stable/generated/torch.nn.SiLU.html). This can be done by writing
```python
from torch.nn.functional inport silu
activated = silu(output)
```
And just like that we've created everything we need for an ANN! Pytorch uses an interface, so we don't have to perform the matrix multiplications, and they initialize the weights for us given the input size and the number of neurons. This can be defined as follows:
