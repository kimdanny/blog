---
title: "[Perceptron] Single and Multi-Layer Perceptron"
categories:
  - deep-learning
tags:
  - perceptron
  - activation function
  - deep-learning
use_math: true
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
## 1. **Neuron**
where the concept of perceptron came from.
neuron image

## 2. **Single Layer Perceptron**
explanation with image
explanation about activation function
```python
def activation_function(y):
    if y <= 0:
        return 0
    else:
        return 1
```

### 2-1. Implementing Logic Gates with perceptrons
By using the activation_function above, we are going to implement famous logic gates with perceptron.

1. AND Gate
```python
def AND_gate(x1, x2):
    x = np.array([x1, x2])

    # these are the weights that are to be multiplied by x1 and x2
    w = np.array([0.5, 0.5])

    # bias
    b = -1 * w[0]

    # perceptron
    y = np.sum(w * x) + b

    return activation_function(y)
```

2. OR Gate
```python
def OR_gate(x1, x2):
    x = np.array([x1, x2])
    w = np.array([0.5, 0.5])
    b = -0.2
    y = np.sum(w * x) + b

    return activation_function(y)
```

3. NAND Gate
```python
def NAND_gate(x1, x2):
    x = np.array([x1, x2])
    w = np.array([-0.5, -0.5])
    b = 0.7
    y = np.sum(w * x) + b

    return activation_function(y)
```

## 3. **Multiple Layer Perceptron**
explanation with image

### 3-1. Implementing XOR Gate with multiple perceptrons
4. XOR Gate
```python
def XOR_gate(x1, x2):
    NANDed = NAND_gate(x1, x2)
    ORed = OR_gate(x1, x2)
    return AND_gate(NANDed, ORed)
```

## 4. **Activation Functions**

### 4-1. Type of activation functions
1. Softmax activation
```python
def softmax(x):
    nominator = np.exp(x)
    denominator = sum(np.exp(x))
    return nominator / denominator
```

2. Sigmoid activation
```python
def sigmoid(x):
    denominator = 1 + np.exp(-x)
    return 1 / denominator
```

3. tanh activation
```python
def tanh(x):
    nominator = np.exp(x) - np.exp(-x)
    denominator = np.exp(x) + np.exp(-x)
    return nominator / denominator
```

4. ReLU activation
```python
def ReLU(x):
    return np.maximum(0, x)
```

### 4-2. Comparison by Visualisation
```python
import matplotlib.pyplot as plt

x = np.arange(-5.0, 5.0, 0.1)
plt.title('Activation Functions')

plt.plot(x, softmax(x))
plt.plot(x, sigmoid(x))
plt.plot(x, tanh(x))
plt.plot(x, ReLU(x))

plt.ylim(-1.5, 1.5)
plt.legend(['y = softmax(x)', 'y = sigmoid(x)', 'y = tanh(x)', 'y = ReLU(x)'])
plt.grid()
plt.show()
```
![Visualisation](/images/perceptron/activation_functions.png)

### 4-3. Outputting final outcome (eg. Softmax)
Softmax function is worth putting an emphasis on,
because for most of the time, Softmax function is used as the activation function
as the last step in fully connected layers.
```python
# data that are outputted from perceptrons
class_scores = np.array([6.53633456, 4.97468644, 8.767656678, 16.6543235, 9.23489844,
              14.8763765, 15.3625635, 16.8317633, 5.98765434, 9.4398844])

# result array that has gone through softmax
result = softmax(class_scores)

max_index = np.argmax(result)
max_value = result[max_index]

print('Result from Softmax: ', np.round(result, 5))
print('maximum value is {}, which has index {}'.format(np.round(max_value, 5), max_index))

# <Terminal output>
# Result from Softmax: [2.0000e-05 0.0000e+00 1.4000e-04 3.7883e-01 2.3000e-04 6.4020e-02
#                       1.0410e-01 4.5238e-01 1.0000e-05 2.8000e-04]
# maximum value is 0.45238, which has index 7
```


## References
