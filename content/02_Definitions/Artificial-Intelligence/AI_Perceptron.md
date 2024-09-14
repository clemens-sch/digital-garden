#Definitions 

[Video](https://www.youtube.com/watch?v=bfmFfD2RIcg)
[Website](https://www.simplilearn.com/tutorials/deep-learning-tutorial/what-is-neural-network)

---
## # Perceptron / Neural Network

They form the base of Deep-Learning (subfield of ML)
- [[AI_AI-ML-DL]]

Algorithms are inspired by human brain.
Neural Networks take data - train themselfes - predict the output.
- Recognizing patterns, solving common problems
NN rely on their traning data.

NN layers:
- **input layer** - takes input data
- **hidden layer** - perform computation
- **output layer** - predicts final output

Each neuron (node):
- represents an individual linear regression model
- is connected to neurons of next layer (through channels - each channel has numerical value - "weight")
- has an input-data, weight, bias, output

---
## # Weight, Bias

**Weights** are values that represent the strength of connections between neurons in layers of NN.
You can say, that the weight represents the intelligence of a neural network.

**Biases** are constants added to influence the activation function of each neuron.

---
## # Feed Forward

Forward Propagation = Feed Forward

Passing input data through the network in forward direction
- input layer - hidden layer - output layer
- without any adjustments being made
---
## # Backward Propagation

Backpropagation = Backward Propagation

Takes output data and goes steps back.
It helps learning from its mistakes by adjusting parameters (weights, biases), based on errors when making predictions
- repeated & repeated until network gets better at tasks

---
## # Use-Cases

- Face recognition
- Forecasting
- Music composition

---
### # Example: Predict Shape

Let's take an example. The neural network should differentiate between a square, circle, triangle.

At first, we differentiate the image in 28x28 pixels. Each pixel gets their own neuron.
Then we multiply the neuron-value with their weight & sum their values up.
- After, we add their BIAS

Now we activate the values (using activation function).
An activated neuron transmits data to the next layer over channels (= "Forward Propagation").

In the end, we multiply & sum & ... till we get the output.

In this example, we have a wrong prediction. So we gonna use Backward Propagation.

![[Pasted image 20240511105845.png]]
