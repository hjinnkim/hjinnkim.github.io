---
title: "[AI toolkit] 10. Optimization (2)"
excerpt: "Neural Network"

categories:
  - AI toolkit
tags:
  - [Neural Network, Activation function]

permalink: /courses/ai-toolkit/010/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-23
last_modified_at: 2022-10-23
order: 11
published: true
use_math: true

---
# Lecture 10

## 0. Table of Contents

1. [Artificial Neural Network](#1-artificial-neural-networks)
2. [Deep Feed Forward Network](#2-deep-feed-forward-network)
3. [Loss](#3-loss)
4. [Optimization](#4-optimization)
5. [Backpropagation](#5-backpropagation)
6. [Overfitting & Solutions](#6-overfitting--solutions)

## 1. Artificial Neural Networks

### 1.1 Perceptrons

<p align="center">
<img src="{{site.gdrive_url_prefix}}1U0l3oyvWi1ElXsvPWVkvcvbNpZggU8EQ" title="lec10-p3 image" style="float: center; width:100%">
</p>

### 1.2 Activation Functions

Activation function : Introducing non-linearity

<figure>
<img src="{{site.gdrive_url_prefix}}1wV_AjI2eU-knsFzraMP6WSp1T2FAaWx6" title="lec10-p4 image" style="float: center; width:100%">
</figure>

<font size="2">Image reference: <a href="https://mc.ai/complete-guide-of-activation-functions/">https://mc.ai/complete-guide-of-activation-functions/</a></font>

### 1.3 Linear Algebra

<img src="{{site.gdrive_url_prefix}}1sAe0E-NsYQf54Gn4mo73paR-42ieF5kp" title="lec10-p5 image" style="float: center; width:100%">

<p>
$\begin{matrix}
x\in\mathbb{R}^3\hfill & \hfill\text{Input} \\
y\in\mathbb{R}^2\hfill & \hfill\text{Output} \\
W\in\mathbb{R}^{2\times 3}\hfill & \hfill\text{Weight} \\
b\in\mathbb{R}^{2}\hfill & \hfill\text{Bias} \\
\end{matrix}$<br>
$y=\sigma(Wx+b)$
</p>

### 1.4 Multi-Layered Perceptrons

<img src="{{site.gdrive_url_prefix}}1czvyIUDiqcNDLoGUX-uZW-b5Ww_AWFyz" title="lec10-p6 image" style="float: center; width:100%">

Hidden Layer(s) : Layer(s) between input and output layer

$y=W^{(3)}\sigma(W^{(2)}\sigma(W^{(1)}x+b^{(1)})+b^{(2)})+b^{(3)}
$

Without activation function,<br>
<p>
$\begin{align*}
y&=W^{(3)}\sigma(W^{(2)}\sigma(W^{(1)}x+b^{(1)})+b^{(2)})+b^{(3)}\\
&=W^{(3)}W^{(2)}W^{(1)}x+(W^{(3)}W^{(2)}b^{(1)}+W^{(3)}b^{(2)}+b^{(3)})\\
&=W^{(new)}x+b^{(new)}\\
\end{align*}$
</p>
Just a linear equation. 
<img src="{{site.gdrive_url_prefix}}1X9uwZ55qEadhovXodgPMvU6lnHBTWavP" title="lec10-p9 image" style="float: center; width:80%">

<font size="2">Image reference: <a href="https://towardsdatascience.com/visualizing-the-non-linearity-of-neural-networks-c55b2a14ad7a">https://towardsdatascience.com/visualizing-the-non-linearity-of-neural-networks-c55b2a14ad7a</a></font>

- Cannot classify these data only with a linear classfier.

## 2. Deep Feed Forward Network

- The point is to learn the (non-linear) representation

<img src="{{site.gdrive_url_prefix}}1G9itQ4TicwCOp0FVc2P9O7lFPZg6mFnW" title="lec10-p10 image" style="float: center; width:80%">

- What engineers do:
  - Find the right general function family $\phi(\cdot)$, that can parameterize the representation of input $X$ with $\theta$.
  - Use the optimization algorithm to fine $\theta$ that can build a good representation.

## 3. Loss

- Loss : The cost from incorrect preditions
<p>
$\text{Training data} : D=\{(x^{(i)},y^{(i)})\}_{i=1,\ldots,N}$
$\text{Neural network model} : \hat{y}^{(i)}=f(x^{(i)};\theta)$
$\text{Loss for i-th data} : \mathcal{L}(f(x^{(i)};\theta),y^{(i)})$
$\text{Empirical loss} : \mathcal{J}(\theta)=\frac{1}{N}\overset{N}{\underset{i=1}{\Large{\Sigma}}}\mathcal{L}(f(x^{(i)};\theta),y^{(i)})$
</p>

- Classification Problem
  - <p>$\hat{y}=\begin{bmatrix}\hat{y}_1 \\ \hat{y}_2 \\ \vdots \\ \hat{y}_{N_c} \\ \end{bmatrix}$, where $N_c$ is the number of classes, $\hat{y}_i$ is a predicted probability of $x$ being i-th class.</p>
  - Loss for i-th data : $\mathcal{L}(f(x^{(i)};\theta),y^{(i)})=\mathcal{L}(\hat{y}^{(i)},y^{(i)})=-\overset{N_c}{\underset{c=1}{\Large{\Sigma}}}y_c\log\hat{y}_c\rightarrow \text{Cross Entropy Loss}$
  - If there are only two labels, true / false
  - $\mathcal{L}(f(x^{(i)};\theta),y^{(i)})=\mathcal{L}(\hat{y}^{(i)},y^{(i)})=-[y_0^{(i)}\log\hat{y}_0^{(i)}+(1-y_0^{(i)})\log(1-\hat{y}_0^{(i)})]$
  $\rightarrow \text{Binary Entropy Loss}$
  - $y_0^{(i)}$=ground truth
- Regression Problem
  - $\hat{y}=\text{Estimated continuous real value}$
  - Loss for i-th data : $\mathcal{L}(f(x^{(i)};\theta),y^{(i)})=\mathcal{L}(\hat{y}^{(i)},y^{(i)})=(\hat{y}^{(i)}-y^{(i)})^2$
    $\rightarrow \text{Mean Squared Error Loss}$

## 4. Optimization

- Find parameters minimizing the Loss
<p align="center">
$\theta^*=arg\ \underset{\theta}{min}( \frac{1}{N}\overset{N}{\underset{i=1}{\Sigma}}\mathcal{L}(f(x^{(i)};\theta),y^{(i)}) )$
</p>

- A neural network based model is not linear
- The loss function would not be a convex
- We cannot get the analytic solution for $\theta^*$

Consider a set of parameters as a n-dimensional vector s.t. $\vec{\theta}\in\mathbb{R}^n$\\
Con we find $\Delta\vec{\theta}$, a direction to change $\vec{\theta}\rightarrow \vec{\theta}+\Delta\vec{\theta}$ to decrease the loss?
$\Rightarrow$ Opposite direction of gradients

### 4.1 Gradient Descenct

- Iteratively optimize parameters based on the training dataset
<p align="center">
$\text{Empirical loss} : \mathcal{J}(\theta)=\frac{1}{N}\overset{N}{\underset{i=1}{\Large{\Sigma}}}\mathcal{L}(f(x^{(i)};\theta),y^{(i)})$
</p>
1. Initialize a set of parameters $\theta$ in a random way
2. Loop below until $\mathcal{J}(\theta)$ converges:
  - Compute $\nabla_\theta\mathcal{J}(\theta)\leftarrow$ This would be heavy and expensive computation
  - Update $\theta\leftarrow \theta-\gamma\nabla_\theta\mathcal{J}(\theta)$
3. Return $\theta$

### 4.2 Stochastic Gradient Descent

1. Initialize a set of parameters $\theta$ in a random way
2. Loop below until $\mathcal{J}(\theta)$ converges:
    1. Pick one data point $(x^{(i)}, y^{(i)})$
    2. compute $\nabla_\theta\mathcal{L}(f(x^{(i)};\theta), y^{(i)})$
    3. Update $\theta\leftarrow \theta-\gamma\nabla_\theta\mathcal{J}(\theta)$
3. Return $\theta$

### 4.3 Mini-Batch Gradient Descent

1. Initialize a set of parameters $\theta$ in a random way
2. Loop below until $\mathcal{J}(\theta)$ converges:
    1. Pick a batch $B= \\{ (x^{(i)}, y^{(i)}) \\}_{i=1,\ldots,N_B}$ of $N_B$ data points
    2. compute $\\frac{1}{N_B}\overset{N_B}{\underset{i=1}{\Large{\Sigma}}}\nabla_\theta\mathcal{L}(f(x^{(i)};\theta), y^{(i)})$
    3. Update $\theta\leftarrow \theta-\gamma\frac{1}{N_B}\overset{N_B}{\underset{i=1}{\Large{\Sigma}}}\nabla_\theta\mathcal{J}(\theta)$
3. Return $\theta$

### 4.4 Learning rate

- $\gamma$ is called "Learning rate"
- If it is too big, loss will diverge
- It it is too small, learning will be too slow
<img src="{{site.gdrive_url_prefix}}14wYIEB-O9z5U6oRuxy0u0lG8n04C_QYy" title="lec10-p26 image" style="float: center; width:80%">

- There are a lot of optimization algorithm (optimizer), which includes adaptive algorithm (momentum)
<img src="{{site.gdrive_url_prefix}}1EqNiWdmKWn1YZ6lXnROBVXWeRr0E8GMt" title="lec10-p27 image" style="float: center; width:80%">

<font size="2">Image reference: <a href="https://www.deeplearningbook.org/">https://www.deeplearningbook.org/</a></font>

### 4.5 Training Error vs. Generalization Error

<img src="{{site.gdrive_url_prefix}}1r5foKYzIv93dB2L6dStDPQayEOLOfsoU" title="lec10-p28 image" style="float: center; width:80%">

<font size="2">Image reference: <a href="https://www.deeplearningbook.org/">https://www.deeplearningbook.org/</a></font>

- Convergence of loss in training data does not guarantee the generalization performance.
- There can be overfitting.
- Need to check validation error during the training.

## 5. Backpropagation

<p align="center">
$\text{Empirical loss} : \mathcal{J}(\theta)=\frac{1}{N}\overset{N}{\underset{i=1}{\Large{\Sigma}}}\mathcal{L}(f(x^{(i)};\theta),y^{(i)})$
</p>

- How to compute $\nabla_\theta\mathcal{J}(\theta)$

<img src="{{site.gdrive_url_prefix}}1GNGxVyyK4d2qpRNc4rjTyL7j7Vca5brN" title="lec10-p31 image" style="float: center; width:80%">
<img src="{{site.gdrive_url_prefix}}1qHJgEVVEj-6iylX2xjpVNJ3v_3T7b5JJ" title="lec10-p31 image" style="float: center; width:80%">
<img src="{{site.gdrive_url_prefix}}1Vu_OjQPqFXb7WRc9bdms5kFCR79bbZcJ" title="lec10-p31 image" style="float: center; width:80%">

Using Chain Rule

<img src="{{site.gdrive_url_prefix}}1UMerB646wclWl1rA1yHyRuyx9jJj_WXU" title="lec10-p31 image" style="float: center; width:80%">
<img src="{{site.gdrive_url_prefix}}1pj4tpWSzgfMeTTpZScBoUOxaDrF0RhZe" title="lec10-p31 image" style="float: center; width:80%">

## 6. Overfitting & Solutions

<img src="{{site.gdrive_url_prefix}}1zqdAKyDQsJZyX5YLEVKsxAwZG3bmpTLL" title="lec10-p40 image" style="float: center; width:80%">

<font size="2">Image reference: <a href="https://www.deeplearningbook.org/">https://www.deeplearningbook.org/</a></font>

- When overfitting occurs, even though the training loss is low, generalization error would be high. 

### 6.1 Regularization

> "Regularization is any modification we make to a learning algorithm that is intended to reduce its generalization error but not its tarining error."
(GoodFellow, 2016)

- L1/L2 Regularization
- Dropout
- Data Augmentation

### 6.2 L1/L2 Regularization

<div>
<ul>
<li>Penalize to the complexity</li>
<li>L1 Regularization : $\mathcal{L}(W)+\lambda\underset{w\in W}{\Large{\Sigma}}\left\lVert w\right\rVert_1$</li>
<li>L2 Regularization : $\mathcal{L}(W)+\lambda\underset{w\in W}{\Large{\Sigma}}\left\lVert w\right\rVert_2$</li>
<li>$\lambda\ :\ \text{hyperparameter}$</li>
</ul>
Gradient with L1/L2 Regularization
<ul>
<li>
<div>
$y=\vert x\vert\rightarrow$ penalty independent to weight size<br>
<body>
    <table border="0" align="center">
 <th>Function</th>
 <th>Differentiation</th>
 <tr><!-- 첫번째 줄 시작 -->
     <td><img src="{{site.gdrive_url_prefix}}1S2MJWedo9Zfa_o3L_9RWoOiEfJVChVif" title="lec10-p43-1 image" style="float: center; width:100%"></td>
     <td><img src="{{site.gdrive_url_prefix}}1K4jb28cfQ34WFs1ptoWODDab7PuWjTrQ" title="lec10-p43-3 image" style="float: center; width:75%"></td>
 </tr>
    </table>
</body>
</div>
</li>
<li>
<div>
$y=x^2\rightarrow$ penalty is proportional to weight size<br>
<body>
    <table border="0" align="center">
 <th>Function</th>
 <th>Differentiation</th>
 <tr><!-- 첫번째 줄 시작 -->
     <td><img src="{{site.gdrive_url_prefix}}1LdYjugsLCcC4WJuQG459P9dkCdGJ-pZO" title="lec10-p43-1 image" style="float: center; width:100%"></td>
     <td><img src="{{site.gdrive_url_prefix}}1LRci5aWSbiohv_MVRFso6LulBReCC7HB" title="lec10-p43-3 image" style="float: center; width:75%"></td>
 </tr>
    </table>
</body>
</div>
</li>
</ul>
</div>

### 6.3 Dropout

- Activate the neuron with probability of $p$ : $\text{hyperparameter}$
- "Sampling the Neural Network"

<img src="{{site.gdrive_url_prefix}}11LWwg19656saOQIh_EMi1cCgw71xS6eC" title="lec10-p44 image" style="float: center; width:80%">

<font size="2">Srivastava, Nitish, et al. "Dropout: a simple way to prevent neural networks from overfitting." The journal of machine learning research 15.1 (2014): 1929-1958.<a href="https://cs231n.github.io/neural-networks-2">https://cs231n.github.io/neural-networks-2</a></font>

### 6.4 Data Augmentation

<img src="{{site.gdrive_url_prefix}}1VAk7GJIfzOOcdqI-hJsXnNPYqakXNBxH" title="lec10-p45 image" style="float: center; width:80%">
