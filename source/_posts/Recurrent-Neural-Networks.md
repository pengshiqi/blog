---
title: Recurrent Neural Networks
date: 2018-01-03 17:31:33
tags: Machine Learning
categories: [Machine Learning]
---

Recurrent neural networks have connections that have loops, adding feedback and memory to the networks over time.

<!-- more -->

A powerful type of Recurrent Neural Network called the Long Short-Term Memory Network has been shown to be particularly effective when stacked into a deep configuration, achieving state-of-the-art results on a diverse array of problems from language translation to automatic captioning of images and videos.

After reading this post, you will know:

- The limitations of Multilayer Perceptrons that are addressed by recurrent neural networks.
- The problems that must be addressed to make Recurrent Neural networks useful.
- The details of the Long Short-Term Memory networks used in applied deep learning.

  ​	

  ### Long Short-Term Memory Network	

The Long Short-Term Memory, or LSTM, network is a type of Recurrent Neural Network (RNN) designed for sequence problems. Given a standard feedforward Multilayer Perceptron network, an RNN can be thought of as the addition of loops to the architecture. The recurrent connections add state or memory to the network and allow it to learn and harness the ordered nature of observations within input sequences.



Instead of neurons, LSTM networks have memory blocks that are connected into layers.

A block has components that make it smarter than a classical neuron and a memory for recent sequences. A block contains gates that manage the block’s state and output. A unit operates upon an input sequence and each gate within a unit uses the sigmoid activation function to control whether they are triggered or not, making the change of state and addition of information flowing through the unit conditional.

There are three types of gates within a memory unit:

- **Forget Gate**: conditionally decides what information to discard from the unit.
- **Input Gate**: conditionally decides which values from the input to update the memory state.
- **Output Gate**: conditionally decides what to output based on input and the memory of the unit.

Each unit is like a mini state machine where the gates of the units have weights that are learned during the training procedure.



#### Vanilla LSTM

It is the LSTM architecture defined in the original 1997 LSTM paper1 and the architecture that will give good results on most small sequence prediction problems.

The Vanilla LSTM is defined as:

1.  Input layer;
2.  Fully connected LSTM hidden layer;
3.  Fully connected output layer.



​	

#### Stacked LSTM



#### CNN LSTM



#### Encode-Decode LSTM	

​			

#### Bidirectional LSTM



#### Generative LSTM		

​	




​			
​		
​	









### Reference

[Crash Course in Recurrent Neural Networks for Deep Learning](https://machinelearningmastery.com/crash-course-recurrent-neural-networks-deep-learning/)