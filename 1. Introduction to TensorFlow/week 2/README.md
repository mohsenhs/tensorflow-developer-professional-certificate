# Introduction to Computer Vision

Computer vision is the field of having a computer understand and label what is present in an image. Can you figure out how can we tell the computer to recognize fashion image? Yes, we use lots of pictures of clothing and tell the computer what that's a picture of and then have the computer figure out the patterns that give you the difference between a shoe, and a shirt, and a handbag, and a coat

## Fashion MNIST Dataset
The [Fashion MNIST Dataset](https://github.com/zalandoresearch/fashion-mnist) is a collection of grayscale 28x28 pixel clothing images.
<p align="center">
    <img src="img/capture-1.PNG" width="400" alt="fashion-mnist"><br>
    <i>Image 1. Fashion Images Dataset</i>
</p>

For image resolution, the smaller the better it becomes because the computer has less processing to do. But of course, we need to retain enough information to be sure that the features and the object can still be distinguished.

## Writing code to load training data
```python
import tensorflow as tf
from tensorflow import keras

# declare object, loading it from the keras database
fashion_mnist = tf.keras.dataseets.fashion_mnist

# (training data, training labels), (testing data, testing labels)
(train_images, train_labels), (test_images, test_labels) = fashion_minst.load_data()
```

We use 60,000 images for training the model and 10,000 left for testing it. 

## The structure of Fashion MNIST data
The labels are represented by a number. Using a number is a first step in avoiding bias (instead of labelling it with words in a specific language).

Learn more about how to avoid bias [here](https://ai.google/responsibilities/responsible-ai-practices/).

## Coding a Computer Vision Neural Network
```python
import tensorflow as tf
from tf import keras

model = keras.Sequential([
    # img resolution 28 x 28
    # turns in into a simple linear array
    keras.layers.Flattern(input_shape=(28,28)), 

    # hidden layer

    keras.layers.Dense(128, activation=tf.nn.relu),

    # units = 10, represents 10 classes of clothing
    keras.layers.Dense(10, activation=tf.nn.softmax) 
])
```

<p align="center">
    <img src="img/capture-2.PNG" width="400" alt="fashion-mnist"><br>
    <i>Image 2. Neural Network Layers </i>
</p>

## Using Callbacks to control training

When you’re done with that, the next thing to do is to explore callbacks. One of the things you can do with that is to train a neural network until it reaches a threshold you want, and then stop training. You’ll see that in the next video.

How can I stop training when I reach a point that I want to be at? Training loop does support callbacks.

```python
mnist = tf.keras.datasets.fashion_mnist
(training_images, training_labels), (test_images, test_labels) = mnist.load_data()
training_images = training_images/255.0
test_images = test_images/255.0
model = tf.keras.models.Sequential([
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(512, activation=tf.nn.relu),
    tf.keras.layers.Dense(10, activation=tf.nn.softmax)
])
model.compile(optimizer='adam', loss='sparse_categorical_crossentropy')
model.fit(training_images, training_labels, epochs=5)

model.evaluate(test_images, test_labels)
```
-==============================

from:https://datascience.stackexchange.com/questions/41921/sparse-categorical-crossentropy-vs-categorical-crossentropy-keras-accuracy

Use sparse categorical crossentropy when your classes are mutually exclusive (e.g. when each sample belongs exactly to one class) and categorical crossentropy when one sample can have multiple classes or labels are soft probabilities (like [0.5, 0.3, 0.2]).

Formula for categorical crossentropy (S - samples, C - classess, s∈c
 - sample belongs to class c) is:

$−1N∑s∈S∑c∈C1s∈clogp(s∈c)$

For case when classes are exclusive, you don't need to sum over them - for each sample only non-zero value is just  −logp(s∈c)
  for true class c.

This allows to conserve time and memory. Consider case of 10000 classes when they are mutually exclusive - just 1 log instead of summing up 10000 for each sample, just one integer instead of 10000 floats.

Formula is the same in both cases, so no impact on accuracy should be there.
==================

Callbacks function.
```python
class myCallback(tf.keras.callbacks.Callback):
    def on_epoch_end(self, epoch, logs={}):
        if(logs.get('loss')<0.4):
            print("\nLoss is low so canceling training!")
            self.model.stop_training = True
```

Then the code will look like this.
```python
# here is your Callbacks function
...

# calling callbacks
callbacks = myCallback()

# here is the rest of uour code
...
...
...

# modification
model.fit(training_images, training_labels, epochs=5, callbacks=[callbacks])
```
=========

https://www.cs.cmu.edu/~bhiksha/courses/deeplearning/Spring.2019/archive-f19/www-bak11-22-2019/document/note/hwnotes/HW1p1.html
Sigmoid: $y = \frac{1}{1 + e^{-z}}$
Tanh: $y = \tanh(z)$
Relu: $y = \max(z,0)$
Softplus: $y = \log(1 + e^z)$
Leaky Relu: $y = \max(z, \alpha z)$, where $0 \lt \alpha \lt 1$ and is typically a small number like 0.01.
Elu: \[ y = \begin{cases} z,~~z\gt 0 \\ \alpha(e^z - 1)~~z \leq 0 \end{cases} \]


![image](https://user-images.githubusercontent.com/7461963/227137766-c82a9cb5-d501-430a-b673-2d16756403a5.png)


======
## Ungraded Lab
* Lab 1: [Get hands-on with computer vision](https://github.com/https-deeplearning-ai/tensorflow-1-public/blob/main/C1/W2/ungraded_labs/C1_W2_Lab_1_beyond_hello_world.ipynb)
* Lab 2: [See how to implement Callbacks](https://github.com/https-deeplearning-ai/tensorflow-1-public/blob/main/C1/W2/ungraded_labs/C1_W2_Lab_2_callbacks.ipynb)

## Other Datasets
Find more `tf.keras.datasets` API [here](https://www.tensorflow.org/api_docs/python/tf/keras/datasets).

## Week 2 Graded Assignments
* Quiz 2: [week-2-quiz](Graded%20Assignment/week2-quiz.md)
* Programming assignment: [notebook](Graded%20Assignment/C1W2_Assignment.ipynb)
