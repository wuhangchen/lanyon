---
title: TF Stanford Tutorial
comments: true
layout: post
comments: true
category: tutorial
tags: [tensorflow, software]
---


```python
import numpy as np
import seaborn
import matplotlib.pyplot as plt
import tensorflow as tf
%matplotlib inline
```


```python
# define input data
data_range = 100
steps = .1
npoints = int(data_range/steps)
x_data = np.arange(data_range, step=steps)
y_data = x_data + 20 *np.sin(x_data/10)
```


```python
# plot input data
plt.scatter (x_data, y_data)
```




    <matplotlib.collections.PathCollection at 0x110835a90>




![png](output_2_1.png)



```python
# define data size and batch size
n_samples = npoints
batch_size = 200
```


```python
# Tensorflow is finicky about shapes, so resize
x_data = np.reshape(x_data, (n_samples,1))
x_data = np.reshape(y_data, (n_samples,1))
```


```python
# Define placeholders for input
X = tf.placeholder(tf.float32, shape=(batch_size, 1))
y = tf.placeholder(tf.float32, shape=(batch_size, 1)) 
```


```python
# Define variables to be learned
with tf.variable_scope("linear-regression"):
 W = tf.get_variable("weights", (1, 1),
                        initializer=tf.random_normal_initializer())
 b = tf.get_variable("bias", (1,),
                        initializer=tf.constant_initializer(0.0))
 y_pred = tf.matmul(X, W) + b
 loss = tf.reduce_sum((y - y_pred)**2/n_samples)
```

Right now, we're essentially trying to minimize

$$
\frac{1}{N}\sum^N_{i=1}(y_i-(W x_i + b))^2

$$

where $$ y_{pred} = (W x_i + b) $$ and $$ N=n_{samples} $$

(p.s., you can read more about `tf.reduce_sum()` [here](https://www.tensorflow.org/api_docs/python/math_ops/reduction#reduce_sum) - I needed to)


```python
optimizer = tf.train.AdamOptimizer()
minimize_operation = optimizer.minimize(loss)
```


```python
def get_batch(x_data, y_data):
    indices = np.random.choice(x_data.shape[0], batch_size)
    x_batch, y_batch = x_data[indices], y_data[indices]
    # again, Tensorflow is finnicky about shapes...
    # without will get: ValueError: Cannot feed value of shape (200,) for Tensor 'Placeholder_1:0', which has shape '(200, 1)
    x_batch = np.reshape(X_batch, (batch_size,1))
    y_batch = np.reshape(y_batch, (batch_size,1))
    return x_batch, y_batch

# A single gradient descent operation
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    # Select random minibatch
    x_batch, y_batch = get_batch(x_data, y_data)
    sess.run([minimize_operation], feed_dict={X: x_batch, y: y_batch})
```


```python
# Full gradient descent for 500 epochs
# Here we also compute and return the loss
with tf.Session() as sess:
    # Initialize Variables in graph
    sess.run(tf.global_variables_initializer())
    # Gradient descent loop for 500 steps
    for _ in range(500):
        # Select random minibatch
        x_batch, y_batch = get_batch(x_data, y_data)
        # Do gradient descent step
        _, loss_val = sess.run([minimize_operation, loss], feed_dict={X: x_batch, y: y_batch})
```


```python

```


```python

```
