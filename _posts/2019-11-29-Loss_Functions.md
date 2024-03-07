---
layout: paper-note
title: "Loss Functions for Multi-label and Multi-class Classification"
description: Choosing the right TensorFlow loss for multi-label vs. multi-class tasks; A guide
date: 2019-1-29

bibliography: paper-notes.bib

---

If you are using Tensorflow and confused with dozen of loss functions for multi-label and multi-class classification, Here you go : In both cases, classes should be one hot encoded

## For Multi-label classification

- [tf.nn.sigmoid_cross_entropy_with_logits](https://www.tensorflow.org/api_docs/python/tf/nn/sigmoid_cross_entropy_with_logits)
- [tf.nn.weighted_cross_entropy_with_logits](https://www.tensorflow.org/api_docs/python/tf/nn/weighted_cross_entropy_with_logits)
- [tf.contrib.losses.sigmoid_cross_entropy](https://www.tensorflow.org/api_docs/python/tf/contrib/losses/sigmoid_cross_entropy)

```python
import tensorflow as tf

cross_entropy = tf.nn.sigmoid_cross_entropy_with_logits(logits=logits, labels=tf.cast(targets,tf.float32))
loss = tf.reduce_mean(tf.reduce_sum(cross_entropy, axis=1))
prediction = tf.sigmoid(logits)
output = tf.cast(prediction > threshold, tf.int32)
train_op = tf.train.AdamOptimizer(0.001).minimize(loss)

  ```

## For Multi-class classification

- [tf.nn.softmax_cross_entropy_with_logits](https://www.tensorflow.org/api_docs/python/tf/nn/softmax_cross_entropy_with_logits)
- [tf.nn.softmax_cross_entropy_with_logits_v2](https://www.tensorflow.org/api_docs/python/tf/compat/v1/nn/softmax_cross_entropy_with_logits_v2)
- [tf.losses.softmax_cross_entropy](https://www.tensorflow.org/api_docs/python/tf/compat/v1/losses/softmax_cross_entropy)

```python
cross_entropy = tf.nn.softmax_cross_entropy_with_logits_v2(
    logits=logits, labels=one_hot_y
)
loss = tf.reduce_mean(cross_entropy)

optimizer = tf.train.AdamOptimizer(learning_rate=0.001).minimize(loss)


predictions = tf.argmax(logits, axis=1, name="predictions")
y_true = tf.argmax(true_labels, axis=1, name="tru_pre")
accuracy = tf.reduce_mean(tf.cast(tf.equal(predictions, y_true), tf.float32))


# or

cross_entropy = tf.nn.softmax_cross_entropy_with_logits(logits=Ylogits, labels=Y_)
cross_entropy = tf.reduce_mean(cross_entropy)

# accuracy of the trained model, between 0 (worst) and 1 (best)
correct_prediction = tf.equal(tf.argmax(Y, 1), tf.argmax(Y_, 1))
accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))


# more detailed

cross_entropy = tf.nn.softmax_cross_entropy_with_logits_v2(
    logits=logits, labels=tf.cast(targets, tf.float32)
)
loss = tf.reduce_mean(cross_entropy)
optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate).minimize(oss)

logit_soft = tf.nn.softmax(logits, name="prob")
predictions = tf.argmax(logit_soft, axis=1, name="predictions")

y_true = tf.argmax(targets, axis=1, name="ground_truth")
accuracy = tf.reduce_mean(tf.cast(tf.equal(predictions, y_true), tf.float32))
```

There is still doubt between :

- [tf.nn.softmax_cross_entropy_with_logits](https://www.tensorflow.org/api_docs/python/tf/nn/softmax_cross_entropy_with_logits)
- [tf.nn.softmax_cross_entropy_with_logits_v2](https://www.tensorflow.org/api_docs/python/tf/compat/v1/nn/softmax_cross_entropy_with_logits_v2)

From [Stack Exchange](https://stats.stackexchange.com/questions/327348/how-is-softmax-cross-entropy-with-logits-different-from-softmax-cross-entropy-wi) here is really clear explanation

In supervised learning, one doesn’t need to backpropagate to labels. They are considered fixed ground truth and only the weights need to be adjusted to match them.

But in some cases, the labels themselves may come from a differentiable source, another network. One example might be [adversarial learning](https://en.wikipedia.org/wiki/Generative_adversarial_network). In this case, both networks might benefit from the error signal. That’s the reason why tf.nn.softmax_cross_entropy_with_logits_v2 was introduced

Note that when the labels are the placeholders (which is also typical), there is no difference if the gradient through flows or not, because there are no variables to apply the gradient to.

So when you are dealing with simple multi-class classification, Go with tf.nn.softmax_cross_entropy_with_logits_v2


## Resources

- [How is softmax_cross_entropy_with_logits different from softmax_cross_entropy_with_logits_v2?](https://stats.stackexchange.com/questions/327348/how-is-softmax-cross-entropy-with-logits-different-from-softmax-cross-entropy-wi)
- [What loss function for multi-class, multi-label classification tasks in neural networks?](https://stats.stackexchange.com/questions/207794/what-loss-function-for-multi-class-multi-label-classification-tasks-in-neural-n/435713#435713)
- [How to choose cross-entropy loss in TensorFlow?](https://stackoverflow.com/questions/47034888/how-to-choose-cross-entropy-loss-in-tensorflow)
- [What are the differences between all these cross-entropy losses in Keras and TensorFlow?](https://stackoverflow.com/questions/44674847/what-are-the-differences-between-all-these-cross-entropy-losses-in-keras-and-ten)