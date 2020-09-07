---
weight: "20"
title: "Machine Learning"
description: "Neural Networks and Machine Learning"
---

These are stories primarily aimed at beginners who want to get started with machine learning. They target the Julia programming language as there is little material online for Julia aimed at complete beginners.

I try to write articles as well about the kind mathematics which is important to learn to understand machine learning, such as various matrix operations.

Machine learning is basically a ton of matrix operations.

## Flux Machine Learning Library

- [Gentle Introduction to Machine Learning](https://medium.com/@Jernfrost/machine-learning-for-dummies-in-julia-6cd4d2e71a46). A story focused on understanding what machine learning is actually about in general. We cover important topics like:
	- Automatic Differentiation
	- What is a Gradient and why are they important to machine learning.
	- The Gradient Descent method. 
- [Implementation of a Modern Machine Learning Library](https://medium.com/@Jernfrost/implementation-of-a-modern-machine-learning-library-3596badf3be). We look under the hood of Flux, to see how it is implemented. Flux is a machine learning library based on automatic differentiation of any regular Julia function. This is the modern approach to ML.
- [Recognize Handwriting Using an Artificial Neural Network](https://medium.com/better-programming/handwriting-recognition-using-an-artificial-neural-network-78060d2a7963). Introduces standard neural network concepts such as perceptrons, layers and activation functions using the Flux ML library.
- [Working with and emulating References in Julia](https://medium.com/@Jernfrost/working-with-and-emulating-references-in-julia-e02c1cae5826)
- [Working with Data in Tables for Machine Learning](https://medium.com/@Jernfrost/working-with-data-in-tables-for-machine-learning-6d7e1bb5bcd7). A lot of the data you are going to feed into your Machine Learning algorithms will come in the shape of tables or data frames. Hence to do machine learning it is useful to have a good handle on how to manipulate data in tables, as well as visualizing that data with a plotting library.
- [Flux vs TensorFlow Misconceptions](https://medium.com/@Jernfrost/flux-vs-tensorflow-misconceptions-2737a8b464fb) People comming from TensorFlow and PyTorch often get the immediate impression that Flux is missing a lot of functionality. Here I try to explain how that is a misconception.

## Mathematics Relevant to Machine Learning

- [The Core Idea of Linear Algebra](https://medium.com/@Jernfrost/the-core-idea-of-linear-algebra-7405863d8c1d)
- [Why Does Matrix Multiplication Work the Way it Does?](https://medium.com/@Jernfrost/why-does-matrix-multiplication-work-the-way-it-does-7a8ed9739254)
- [Functions in Mathematics and Programming](https://medium.com/@Jernfrost/functions-in-mathematics-and-programming-9741cbeb8d4b)