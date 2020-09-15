例子出自Hackers guide to machine learning with python

使用的数据文件非常小，只有两列，分别为Speed和Stopping Distances of Cars.
[数据文件](https://vincentarelbundock.github.io/Rdatasets/datasets.html)
其中的cars

代码如下：
```
# coding: utf-8

import pandas as pd
import tensorflow as tf
import matplotlib
matplotlib.use('Qt5Agg')
import matplotlib.pyplot as plt
import seaborn as sns

sns.set(style='whitegrid', palette='muted', font_scale=1.5)

data_csv=pd.read_csv('datasets/cars.csv', usecols=['speed', 'dist'])
print(data_csv.head())

target = data_csv['dist']
speed = data_csv['speed']

#plot datasets
sns.scatterplot(speed, target);
plt.xlabel("speed")
plt.ylabel("stopping distance");


dataset=tf.data.Dataset.from_tensor_slices((speed.values, target.values))
for feat, targ in dataset.take(5):
    print('Feature: {}, Target: {}'.format(feat, targ))
    

from tensorflow.keras import layers
lin_reg=tf.keras.Sequential([layers.Dense(1, activation='linear',input_shape=[1]),])
optimizer = tf.keras.optimizers.RMSprop(0.001)

lin_reg.compile(loss='mse', optimizer=optimizer, metrics=['mse'])
history = lin_reg.fit(x=speed, y=target, shuffle=True, epochs=1000, validation_split=0.2, verbose=0)

def plot_error(history):
    hist = pd.DataFrame(history.history)
    hist['epoch'] = history.epoch
    
    plt.figure()
    plt.xlabel('Epoch')
    plt.ylabel('Mean Square Error')
    plt.plot(hist['epoch'], hist['mse'],
                label='Train Error')
    plt.plot(hist['epoch'], hist['val_mse'],
                label = 'Val Error')
    
    plt.legend()
    plt.show()

plot_error(history)

print(lin_reg.summary())
#get weights
weights = lin_reg.get_layer("dense").get_weights()
intercept = weights[0][0][0]
slope = weights[1][0]

print('weigths: {}'.format(weights))
print('intercept: {}'.format(intercept))
print('slope: {}'.format(slope))


#build simple neural network
def build_neural_net():
    net = tf.keras.Sequential([
        layers.Dense(32, activation='relu', input_shape=[1]),
        layers.Dense(16, activation='relu'),
        layers.Dense(1),
    ])
    
    optimizer = tf.keras.optimizers.RMSprop(0.001)
    
    net.compile(loss='mse',
                    optimizer=optimizer,
                    metrics=['mse', 'accuracy'])
    
    return net

net = build_neural_net()

history = net.fit(
    x=speed,
    y=target,
    shuffle=True,
    epochs=1000,
    validation_split=0.2,
    verbose=0
)

plot_error(history)

#stop training early
early_stop = tf.keras.callbacks.EarlyStopping(
    monitor='val_loss',
    patience=10
)


net = build_neural_net()

#simple neural network summary
print(net.summary())
print(net.weights)


history = net.fit(
    x=speed,
    y=target,
    shuffle=True,
    epochs=1000,
    validation_split=0.2,
    verbose=0,
    callbacks=[early_stop]
    )

plot_error(history)

#save & restore model
net.save('simple_net.h5')
simple_net = tf.keras.models.load_model('simple_net.h5')
print(simple_net.summary())
```

simple linear regression得出的slope 和intercept
试过几次，差距很大
```
intercept: 2.5482685565948486
slope: 0.8580179810523987

intercept: 0.6354206800460815
slope: 1.985124111175537
那么得出的equation就会分别是
y = 0.8580179810523987 * x + 2.5482685565948486
y = 1.985124111175537 * x + 0.6354206800460815
```
为什么?
