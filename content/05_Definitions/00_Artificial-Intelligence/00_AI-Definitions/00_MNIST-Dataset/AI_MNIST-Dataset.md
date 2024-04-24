#Definitions 

[Main-Source](https://towardsdatascience.com/image-classification-in-10-minutes-with-mnist-dataset-54c35b77a38d)

---
## # Assignment

Our trained model should predict on which number is written.

The output should look like the following:

![[Pasted image 20240424101701.png]]

---
## # Intro: MNIST dataset



---
## # Implementation

Get MNIST-dataset

```python
import matplotlib.pyplot as plt
import tensorflow as tf
(x_train, y_train), (x_test, y_test) = tf.keras.datasets.mnist.load_data()
```

visualization

```python
%matplotlib inline
image_index = 7777 
print(y_train[image_index]) 
plt.imshow(x_train[image_index], cmap='Greys')
```
= 8

get to know dataset shape

```python
x_train.shape
```
= (60000, 28, 28) - amount of data, size of image

reshaping & normalizing

```python
# Reshaping the array to 4-dims so that it can work with the Keras API 
x_train = x_train.reshape(x_train.shape[0], 28, 28, 1) 
x_test = x_test.reshape(x_test.shape[0], 28, 28, 1) 
input_shape = (28, 28, 1)
# Making sure that the values are float so that we can get decimal points after division 
x_train = x_train.astype('float32') 
x_test = x_test.astype('float32') 
# Normalizing the RGB codes by dividing it to the max RGB value. 
x_train /= 255
x_test /= 255 
print('x_train shape:', x_train.shape) 
print('Number of images in x_train', x_train.shape[0]) 
print('Number of images in x_test', x_test.shape[0])
```
= x_train shape: (60000, 28, 28, 1)
Number of images in x_train 60000
Number of images in x_test 10000

build convolutional neural network

```python
# Importing the required Keras modules containing model and layers
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Conv2D, Dropout, Flatten, MaxPooling2D

# Creating a Sequential Model and adding the layers
model = Sequential()
model.add(Conv2D(28, kernel_size=(3,3), input_shape=input_shape))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Flatten()) # Flattening the 2D arrays for fully connected layers
model.add(Dense(128, activation=tf.nn.relu))
model.add(Dropout(0.2))
model.add(Dense(10,activation=tf.nn.softmax))
```

compiling & fitting the model

```python
model.compile(optimizer='adam', 
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy']) 
model.fit(x=x_train,y=y_train, epochs=30)
```
= Epoch 1/10
1875/1875 ━━━━━━━━━━━━━━━━━━━━ 23s 12ms/step - accuracy: 0.6364 - loss: 122.8531
Epoch 2/30
1875/1875 ━━━━━━━━━━━━━━━━━━━━ 22s 12ms/step - accuracy: 0.6373 - loss: 1.2093

evaluating

```python
model.evaluate(x_test, y_test)
```
313/313 ━━━━━━━━━━━━━━━━━━━━ 2s 5ms/step - accuracy: 0.9617 - loss: 0.2242
...
Acc. ~ 96%

prediction for image

```python
image_index = 4444
plt.imshow(x_test[image_index].reshape(28, 28), cmap='Greys')
pred = model.predict(x_test[image_index].reshape(1, 28, 28, 1))
print(pred.argmax())
```
= 1/1 ━━━━━━━━━━━━━━━━━━━━ 0s 72ms/step
9

![[Pasted image 20240424110221.png]]

---