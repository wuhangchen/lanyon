---
title: Variable Scope in Tensorflow
comments: true
layout: post
comments: true
category: tutorial
tags: [tensorflow, software]
---

Assume you define a function to do a series of matrix multiplications follows:

```python
def function(X):
    matrix1 = tf.Variable(tf.random_normal([3,3], stddev=0.1), name="matrix1")
    bias1 = tf.Variable(tf.zeros([1]), name="bias1")
    output1 = tf.matmul(X, matrix1) + bias1

    matrix2 = tf.Variable(tf.random_normal([3,3], stddev=0.1), name="matrix1")
    bias2 = tf.Variable(tf.zeros([1]), name="bias2")
    return tf.matmul(m1, matrix2)
```

And call it as follows

```python
result1 = function(some_matrix)
result2 = function(another_matrix)
````

This would result in 4 variables (matrix1, bias1, matrix2, bias2) being created twice. Which you don't want to do. You want to create the same matrices and reuse them every time that `function(x)` is called. Tensorflow's solution are the `tf.get_variable(<name>, <shape>, <initializer>)` and `tf.variable_scope(<scope_name>)` functions. The former creates or returns a variable with a given name, while the latter manages a namespace for variables. The code above can now be re-written as

```python
def matrix_multiplication(input, matrix_shape, bias_shape):
    matrix = tf.get_variable(tf.random_normal(matrix_shape, stddev=0.1), name="matrix")
    bias = tf.get_variable(tf.zeros(bias_shape), name="bias")
    return tf.matmul(X, matrix1) + bias1

def function(X):
    with tf.variable_scope("matrix1"):
        output = matrix_multiplication(X, [3, 3], [3])
    with tf.variable_scope("matrix2"):
        return matrix_multiplication(output, [3, 3], [3])

```



You can read an expanded version of this on the [tensorflow website](https://www.tensorflow.org/how_tos/variable_scope/).