# 1. Introduction

[TensorFlow](http://tensorflow.org) is an open source library for numerical computation, specializing in machine learning applications. In this codelab, you will learn how to install and run TensorFlow on a single machine, and will train a simple classifier to classify images of flowers.

What are we going to be building?

In this lab, we will be using transfer learning, which means we are starting with a model that has been already trained on another problem. We will then be retraining it on a similar problem. Deep learning from scratch can take days, but transfer learning can be done in short order.

We are going to use a model trained on the [ImageNet](http://image-net.org/) Large Visual Recognition Challenge [dataset](http://www.image-net.org/challenges/LSVRC/2012/). These models can differentiate between 1,000 different classes, like Dalmatian or dishwasher. You will have a choice of model architectures, so you can determine the right tradeoff between speed, size and accuracy for your problem.

We will use this same model, but retrain it to tell apart a small number of classes based on our own examples.

What you will learn:

*   How to use Python and TensorFlow to train an image classifier
*   How to classify images with your trained classifier

What you need:

*   A basic understanding of Linux commands