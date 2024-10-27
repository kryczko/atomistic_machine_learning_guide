# Basics of Neural Networks

There are many resources out there that provide details of artificial neural networks (ANNs), so I will keep things brief. ANNs are inspired by neurons in the brain. A Neuron receives some input and based on that input will turn on or off. Neurons have connections with many others, and therefore the action of turning on a single neuron may trigger another, creating a cascading, collective effect that inevitably leads to a final result. Instead of considering a binary case of neurons turning on and off, ANNs work on floating point values. If the problem we're trying to solve is classification, we must convert the final output to binary values (i.e. 1s and 0s). However, most cases in atomistic machine learning are regression problems. ANNs consist of `layers` and in each layer, the inputs get multiplied against the `weights` for that layer, a `bias` is added, and a non-linear `activation` function is applied. The activation function is the necessary ingredient to learn non-linear mappings.

#### Programming a Single Layer
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
from torch.nn.functional import silu
activated = silu(output)
```
And just like that we've created everything we need for an ANN! Pytorch uses an interface, so we don't have to perform the matrix multiplications, and they initialize the weights for us given the input size and the number of neurons. This can be defined as follows:
```python
from torch.nn import Linear
layer = Linear(10, 1) # 10 input features, one output feature
output = silu(layer(x))
```
Within each `torch.nn.Module` class, there is a `forward` function that performs the math we wrote above.

#### Programming an Artificial Neural Network
Now that we know how a single layer works within an ANN, we can string them together to form our first neural network. To do this we can introduce another class:
```python
from torch.nn import Sequential, Linear, SiLU
layers = []
for _ in range(3):
    layers += [Linear(10, 10), SiLU()]
net = Sequential(*layers) # * operation expands the list
net.append(Linear(10, 1))
x = torch.rand(1, 10) # a single vector with dimension 10

output = net(x)
 
```
The  `Sequential` class enables us to combine various layers, creating our first neural network! However, if we want to do something fancier (let's say add a residual connection), then we have to define our own layer. To do this, consider the following code block:
```python
class ResidualForward(nn.Module):
    def __init__(self, input_dim, output_dim, activation=nn.SiLU()):
        super().__init__() # allows us to use methods from the parent class nn.Module
        self.input_dim = input_dim
        self.output_dim = output_dim
        self.act = activation

    def forward(self, x):
        
```
