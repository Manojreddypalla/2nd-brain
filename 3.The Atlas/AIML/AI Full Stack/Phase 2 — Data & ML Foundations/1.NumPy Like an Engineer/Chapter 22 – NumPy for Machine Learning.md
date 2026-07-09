# NumPy Chapter 22 – NumPy for Machine Learning

> **Goal:** Understand how NumPy fits into the ML ecosystem.

---

# The AI Stack

```
Python
   │
   ▼
NumPy
   │
   ▼
Pandas
   │
   ▼
Scikit-Learn
   │
   ▼
TensorFlow / PyTorch
```

NumPy is the **foundation**.

---

# Why ML Uses NumPy

Machine Learning works with:

- Numbers
- Matrices
- Vectors
- Tensors

NumPy is designed for exactly that.

---

# Dataset

Features (X)

```python
X = np.array([
    [170,70],
    [180,80],
    [160,60]
])
```

Think

```
Height

Weight
```

---

Labels (y)

```python
y = np.array([0,1,0])
```

```
0 = No

1 = Yes
```

---

# Normalize Data

```python
X = X / 255
```

Common before training.

---

# Matrix Multiplication

Neural Networks use

```python
X @ W
```

Where

```
X

↓

Input

W

↓

Weights
```

---

# Bias

```python
output = X @ W + b
```

Broadcasting automatically adds the bias.

---

# Activation

```python
np.maximum(0, output)
```

This is the **ReLU** activation.

---

# Prediction

```python
predictions = np.argmax(output, axis=1)
```

Pick the class with the highest score.

---

# Loss

```python
error = prediction - target
```

Training tries to reduce this error.

---

# Random Weights

```python
W = np.random.randn(3,4)
```

Most models start with random weights.

---

# Why Vectorization?

Instead of

```python
for sample in dataset:
```

ML libraries compute

```python
X @ W
```

on the **entire batch**.

---

# Where NumPy Appears

| Field | Example |
|--------|---------|
| AI | Neural Networks |
| ML | Scikit-Learn |
| Data Science | Pandas |
| Computer Vision | OpenCV |
| Robotics | Sensor Data |
| Games | Physics & Matrices |

---

# Mini Example

```python
import numpy as np

# Features
X = np.array([
    [1,2],
    [3,4]
])

# Weights
W = np.array([
    [2],
    [1]
])

# Bias
b = 1

# Prediction
output = X @ W + b

print(output)
```

Output

```python
[[5]
 [11]]
```

---

# Cheat Sheet

| ML Concept | NumPy |
|------------|--------|
| Dataset | `np.array()` |
| Normalize | `/255` |
| Matrix Multiply | `@` |
| Activation | `np.maximum()` |
| Prediction | `argmax()` |
| Random Weights | `randn()` |
| Statistics | `mean()` |

---

# Key Takeaways

- NumPy is the foundation of almost every ML library.
- Datasets are NumPy arrays.
- Neural networks rely heavily on matrix multiplication (`@`).
- Broadcasting adds biases efficiently.
- Vectorization allows processing entire batches without Python loops.