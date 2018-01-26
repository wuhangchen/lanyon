---
title: Tensorflow Basics
comments: true
layout: post
comments: true
category: tutorial
tags: [tensorflow, software]
---















## Plug and play with Tensorflow

For simple testing, you can use `tf.InteractiveSession()`. This will leave the default session open. In ipython, this will keep the default session open throughout. Then, in order to get a value out of any tensorflow variable, you can use `.eval()`.
```python
import tensorflow as tf
tf.InteractiveSession()
a = tf.zeros((2,2)); b = tf.ones((2,2))
print (tf.reduce_sum(b, reduction_indices=1).eval())
```

## Sessions in Tensorflow
Sessions encapsulate the environment in which tensorflow objects are created
```python
a = tf.constant(5.0)
b = tf.constant(6.0)
c = a * b
with tf.Session() as sess:
  print(sess.run(c))          # these are actually the same thing
  print(c.eval())             # run the graph again
```

## Tensor Flow Fetch

The `sess.run(c)` we saw before was an example of this.


#### References

[1]: https://cs224d.stanford.edu/lectures/CS224d-Lecture7.pdf "Lecture on Tensorflow"
[2]: https://www.tensorflow.org/how_tos/variable_scope/ "Tensorflow: Variable Scopes"
<!-- [3]: https://www.tensorflow.org/api_docs/ "Tensorflow API" -->