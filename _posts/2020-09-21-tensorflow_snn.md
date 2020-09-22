该neuron network例子是出自Hacker guide to machine learning with python
代码在[https://github.com/curiousily/Deep-Learning-For-Hackers](https://github.com/curiousily/Deep-Learning-For-Hackers)

我几乎是照着原代码重新又敲了一遍，只是碰到一个很怪异的问题

```
Exception ignored in: <bound method _RandomSeedGeneratorDeleter.__del__ of <tensorflow.python.data.ops.dataset_ops._RandomSeedGeneratorDeleter object at 0x7f69cc06e470>>
Traceback (most recent call last):
  File "/python-env/TensorEnv/lib/python3.5/site-packages/tensorflow_core/python/data/ops/dataset_ops.py", line 3462, in __del__
AttributeError: 'NoneType' object has no attribute 'device'
Exception ignored in: <bound method _RandomSeedGeneratorDeleter.__del__ of <tensorflow.python.data.ops.dataset_ops._RandomSeedGeneratorDeleter object at 0x7f69bc49cc18>>
Traceback (most recent call last):
  File "/python-env/TensorEnv/lib/python3.5/site-packages/tensorflow_core/python/data/ops/dataset_ops.py", line 3462, in __del__
AttributeError: 'NoneType' object has no attribute 'device'
```

猜了半天猜到是内存问题，网上的解决方案有两种

```
1.
from tensorflow.keras import backend as k
k.clear_session()
2.
import gc; 
gc.collect()
```

从`RandomSeedGenerator`来看，原因是代码里`shuffle`的使用，要解决这个问题，
可以把`shuffle`这一步给去掉
代码如下：

```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# Author: 
# Created Time: Sun 20 Sep 2020 07:31:41 PM CST

import tensorflow as tf
from tensorflow import keras
import numpy as np
import pandas as pd
import matplotlib
matplotlib.use('Qt5Agg')
import matplotlib.pyplot as plt
import seaborn as sns

(X_train, y_train),(X_test, y_test) = keras.datasets.fashion_mnist.load_data()



print('X_train shape:{}, X_test shape:{}'.format(X_train.shape,X_test.shape))
print('y_train shape:{}, y_test shape:{}'.format(y_train.shape, y_test.shape))

plt.imshow(X_train[0])
plt.grid(False)

class_names = ['T-shirt/top', 'Trouser', 'Pullover','Dress',
    'Coat','Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot']


plt.figure(figsize=(10,10))
for i in range(25):
    plt.subplot(5,5,i+1)
    plt.xticks([])
    plt.yticks([])
    plt.imshow(X_train[i], cmap=plt.cm.binary)
    plt.xlabel(class_names[y_train[i]])
plt.show()


def preprocess(X, y):
    x = tf.cast(X, tf.float32) / 255.0
    y = tf.cast(y, tf.int64)

    return x, y

def create_dataset(Xs, ys, n_classes=10):
    ys = tf.one_hot(ys, depth=n_classes)
    #return tf.data.Dataset.from_tensor_slices((Xs, ys)).map(preprocess).shuffle(shuffle_buffer_size=len(ys)).batch(batch_size=128)

    return tf.data.Dataset.from_tensor_slices((Xs, ys)).map(preprocess).batch(batch_size=128)


train_dataset = create_dataset(X_train, y_train)
test_dataset = create_dataset(X_test, y_test)

model = keras.Sequential([

    #input layer, convert (2D 28*28 array)  -> (1D 284 array)
    keras.layers.Reshape(
        target_shape=(28 * 28,), input_shape=(28,28)
    ),
    
    #Dense layers
    keras.layers.Dense(
        units=256, activation='relu'
    ),
    keras.layers.Dense(
        units=192, activation='relu'
    ),
    keras.layers.Dense(
        units=128, activation='relu'
    ),
    
    #output layer
    keras.layers.Dense(
        units=10, activation='softmax'
    )
])


    
#train your model
model.compile(optimizer='adam',
    loss=tf.losses.CategoricalCrossentropy(from_logits=True),
    metrics=['accuracy']
)

history = model.fit(
    train_dataset.repeat(),
    epochs=10,
    steps_per_epoch=500,
    validation_data= test_dataset.repeat(),
    validation_steps = 2
)



plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('model accuracy')
plt.ylabel('accuracy')
plt.xlabel('epoch')
plt.ylim((0,1))
plt.legend(['train', 'test'],loc='upper left')


plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.ylim(1.5, 2)
plt.legend(['train', 'test'], loc='upper left')
plt.show()


#predication
predictions = model.predict(test_dataset)
print(predictions[0])    
print(np.argmax(predictions[0]))

'''release memory to deal with 
Exception ignored in: <bound method _RandomSeedGeneratorDeleter.__del__ of <tensorflow.python.data.ops.dataset_ops._RandomSeedGeneratorDeleter object at 0x7f8c4c093358>>
'''
#from tensorflow.keras import backend as k
#k.clear_session()



def plot_image(i, predictions_array, true_label, img):
    predictions_array, true_label , img =( predictions_array[i],true_label[i], img[i])
    plt.grid(False)
    plt.xticks([])
    plt.yticks([])
    plt.imshow(img, cmap=plt.cm.binary)
    predicted_label = np.argmax(predictions_array)
    if predicted_label == true_label:
        color = 'green'
    else:
        color = 'red'
    plt.xlabel("Predicted: {} {:2.0f}% (True: {})".format(class_names[predicted_label],
        100*np.max(predictions_array),
        class_names[true_label]),
        color = color)
    plt.show()

i = 0
plot_image(i, predictions, y_test, X_test)


#release memory : approach 2
import gc; 
gc.collect()
```



