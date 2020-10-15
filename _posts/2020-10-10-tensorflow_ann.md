在尝试用ann来classifiy MNIST的过程中，我尝试了几个不同的`activation function`
分别是LeakyReLU，Relu，elu，softplus.得到的loss 和 accuracy.如下

```
Lrelu               0.07303281348586316, 0.9797
Relu                0.06841484235865646, 0.9798
elu                 0.06728434420697159, 0.9814
softplus            0.07191591464017984, 0.9797
```

彼此之间差别很小

代码如下

```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# Author: 
# Created Time: Sat 10 Oct 2020 02:20:05 PM CST

#multi classification

import tensorflow as tf
from tensorflow import keras
import numpy as np
import pandas as pd
import matplotlib
matplotlib.use('Qt5Agg')
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib import rc
from pylab import rcParams
import joblib
from sklearn.model_selection import train_test_split

from tensorflow.keras import backend as k
k.clear_session()

mnist = tf.keras.datasets.mnist
(X_train, y_train), (X_test, y_test) = mnist.load_data()
#scale X to (0,1)
X_train, X_test = X_train/255.0, X_test/255.0
print("X_train shape {}".format(X_train.shape))

#Build the model
model = tf.keras.models.Sequential([
    tf.keras.layers.Flatten(input_shape=(28, 28)),
    tf.keras.layers.Dense(128, activation='relu'),
    #tf.keras.layers.Dense(128, activation=tf.keras.layers.LeakyReLU(alpha=0.01)),
    #tf.keras.layers.Dense(128, activation='elu'),
    #tf.keras.layers.Dense(128, activation='softplus'),
    tf.keras.layers.Dropout(0.2),
    tf.keras.layers.Dense(10, activation='softmax')
    ])

#compile the model
model.compile(optimizer='adam', loss='sparse_categorical_crossentropy',metrics=['accuracy'])

#train the model
r = model.fit(X_train, y_train, validation_data=(X_test, y_test), epochs=10)

#plot loss per iteration
plt.plot(r.history['loss'], label='loss')
plt.plot(r.history['val_loss'], label='val_loss')
plt.legend()
plt.show()

#plot accuracy per iteration
plt.plot(r.history['accuracy'], label='acc')
plt.plot(r.history['val_accuracy'], label='val_accuracy')
plt.legend()
plt.show()

#Evaluate the model
print(model.evaluate(X_test, y_test))


import itertools
#plot confustion matrix
from sklearn.metrics import confusion_matrix
def plot_confusion_matrix(cm, classes, normalize=False, title='confustion matrix', cmap=plt.cm.Blues):
    
    if normalize:
        cm = cm.astype('float') / cm.sum(axis=1)[:, np.newaxis]
        print('Normalized confustion matrix')
    else:
        print('Confustion matrix without normalization')
    
    print(cm)
    plt.imshow(cm, interpolation='nearest', cmap=cmap)
    plt.title(title)
    plt.colorbar()
    tick_marks = np.arange(len(classes))
    plt.xticks(tick_marks, classes, rotation=45)
    plt.yticks(tick_marks, classes)
    
    fmt = '.2f' if normalize else 'd'
    thresh = cm.max() / 2
    for i ,j in itertools.product(range(cm.shape[0]),range(cm.shape[1])):
        plt.text(j, i, format(cm[i,j], fmt),horizontalalignment='center',
        color='white' if cm[i,j] > thresh else 'black')
    
    plt.tight_layout()
    plt.ylabel('True label')
    plt.xlabel('predicted label')
    plt.show()


#predict
P_test = model.predict(X_test).argmax(axis=1)
cm = confusion_matrix(y_test, P_test)
plot_confusion_matrix(cm, list(range(10)))

#show some missclassified examples
misclassified_idx = np.where(P_test != y_test)[0]
i = np.random.choice(misclassified_idx)
plt.imshow(X_test[i], cmap='gray')
plt.title('True label: {}  Predicted: {}'.format(y_test[i], P_test[i]))
plt.show()

#release memory
import gc; 
gc.collect()

```
