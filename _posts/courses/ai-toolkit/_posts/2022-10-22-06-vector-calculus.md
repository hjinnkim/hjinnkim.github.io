---
title: "[AI toolkit] 06. Vector Calculus"
excerpt: "Vector & Matrix Calculus"

categories:
  - AI toolkit
tags:
  - [Linear Algebra, Calculus, Gradient, Jacobian, Hessian]

permalink: /courses/ai-toolkit/006/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-22
last_modified_at: 2022-10-22
order: 7
published: true
use_math: true

---
# Lecture 06

## 0. Table of Contents
1. [Function and Machine Learning](#1-function-and-machine-learning)
2. [Differential Rules](#2-differential-rules)
3. [Derivative and Approximation](#3-derivative-and-approximation)
4. [Taylor Series](#4-taylor-series)
5. [Partial Differentiation and Gradient](#5-partial-differentiation-and-gradient)
6. [Rules of Partial Differentiation](#6-rules-of-partial-differentiation)
7. [Gradient of Vector-Valued Functions](#7-gradients-of-vector-valued-functions)
8. [Jacobian and Hessican](#8-jacobian-and-hessian)
9. [Useful Identities for Computing Gradients](#9-useful-identities-for-computing-gradients)
10. [Higher Dimension Differentiation and Approximation](#10-higher-dimension-differentiation-and-approximation)
11. [Gradients, Backpropagation, Chain Rule, and Neural Network](#11-gradients-backpropagation-chain-rule-and-neural-network)

## 1. Function and Machine Learning

- Goal of machine learning is to approximate the unknow real function $y=f^*(x)$ with $f_\theta(x)$,
  - where $\theta\in\mathbb{R}^d$ is a model parameter.
  - We want to find optimal $\theta$, which can optimize the objective function.
    - Classification : $-\Sigma p^*(i)\log p(i)$, 
    
      ($i$ is a class index, $p^*(i)$ is a real probability, $p(i)$ is an estimated probability distribution of class.)
    - Regression : $\left\lVert y-f_\theta (x)\right\rVert_{1 or 2}$
- If objective functions are differentiable, $\frac{df}{dx}:=\underset{\delta x \to 0}{\lim}\frac{f(x+\delta x)-f(x)}{\delta x}$, can we find optimal $\theta$?
<p align="center">
  <img src="{{site.gdrive_url_prefix}}1RboC3s5cdCo6Q3TC81JnzPuku6tI4Krd" title="lec06-p3 image" style="float: center; width:60%">
</p>

## 2. Differential Rules

- Product rule
    $\Rightarrow (f(x)g(x))' = f'(x)g(x)+f(x)g'(x)$
- Quoient rule
    $\Rightarrow (\frac{f(x)}{g(x)})'=\frac{f'(x)g(x)-f(x)g'(x)}{g(x)^2}$
- Sum rule
    $\Rightarrow (f(x)+g(x))'=f'(x)+g'(x)$
- <span style="color:blue">Chain rule</span>
    $(g(f(x)))'=g'(f(x))f'(x)$
- $\frac{d}{dx}x^n=nx^{n-1}$

  $\frac{d}{dx}e^x=e^x$

  $\frac{d}{dx}\ln x=\frac{1}{x}$

## 3. Derivative and Approximation

- $\frac{df}{dx}(x)=\underset{\epsilon \to 0}{\lim}\frac{f(x+\epsilon)-f(x)}{\epsilon}\Rightarrow \frac{df}{dx}(x)\approx \frac{f(x+\epsilon)-f(x)}{\epsilon}$
$\Rightarrow \epsilon\frac{df}{dx}(x)\approx f(x+\epsilon)-f(x)\Rightarrow f(x+\epsilon)\approx\epsilon\frac{df}{dx}(x)+f(x)$
- $f(x_0+\epsilon)=f(x)\cong(x-x_0)\frac{df}{dx}(x_0)+f(x_0)$
<p align="center">
  <img src="{{site.gdrive_url_prefix}}19N58bXYpNbV3mlT-Bdx-0oxoeWdSC5EP" title="lec06-p5 image" style="float: center; width:60%">
</p>

  $\Rightarrow$ Approximate the value of $f$ by a line which passes $(x, f(x))$ with slope of $\frac{df}{dx}(x)$

## 4. Taylor Series

<div>
<img src="{{site.gdrive_url_prefix}}142Sjg03CZAGDuyAA3qVqLZQkwlULY2CY" title="lec06-p6-1 image" style="float: right; width:30%">
<ul>
<li>The Taylor polynomial of degree $n$ of $f:\mathbb{R} \to \mathbb{R}$ at $x_0$ is defined as:</li>
<ul>$\Rightarrow T_n(x)=\overset{n}{\underset{k=0}{\Sigma}}\frac{f^{(k)}(x_0)}{k!}(x-x_0)^k$</ul><br><br>
<img src="{{site.gdrive_url_prefix}}1Bc1cPRJ5nrbTAI5mnAEsVpJnesrc9CJT" title="lec06-p6-2 image" style="float: right; width:30%">
<li>If $n\to\infty$, the Taylor series of $f$ at $x_0$ is defined as:</li>
<ul>$\Rightarrow T_\infty (x)=\overset{\infty}{\underset{k=0}{\Sigma}}\frac{f^{(k)}(x_0)}{k!}(x-x_0)^k$</ul>
</ul>
</div>

## 5. Partial Differentiation and Gradient

- For a function $f:\mathbb{R}^n\to\mathbb{R}$, partial derivative is defined as:
  $\Rightarrow\frac{\partial f}{\partial x_i}=\underset{h\to 0}{\lim}\frac{f(x_1,\ldots, x_i+h,\ldots, x_n)-f(x)}{h}$
- If partial derivatives are collected into the row vector, gradient of a function is obtained.

  $\Rightarrow \nabla_x f=\frac{df}{d\mathbf{x}}=[\frac{\partial f(x)}{\partial x_1}\ \frac{\partial f(x)}{\partial x_2}\cdots \frac{\partial f(x)}{\partial x_n}]\in\mathbb{R}^{1\times n}$
  It depends on the situation. Mostly, the gradient is represented as a column vector.

## 6. Rules of Partial Differentiation

- Product rule, sum rule, chain rule also applies for partial differentiation.
- $\frac{\partial}{\partial\mathbf{x}}(f(\mathbf{x})g(\mathbf{x}))=\frac{\partial f}{\partial \mathbf{x}}g(\mathbf{x})+\frac{\partial g}{\partial \mathbf{x}}f(\mathbf{x}),\ \frac{\partial}{\partial \mathbf{x}}(f(\mathbf{x})+g(\mathbf{x}))=\frac{\partial}{\partial\mathbf{x}}(f(\mathbf{x})+g(\mathbf{x})),$ $\frac{\partial}{\partial\mathbf{x}}(g(f(\mathbf{x}))=\frac{\partial g}{\partial f}\frac{\partial f}{\partial\mathbf{x}}$
- $\frac{df}{dt}=[\frac{\partial f}{\partial x_1}\ \frac{\partial f}{\partial x_2}] \begin{bmatrix}
\frac{\partial x_1(t)}{\partial t} \\\\\\
\frac{\partial x_2(t)}{\partial t} \\\\\\
\end{bmatrix}=\frac{\partial f}{\partial x_1}\frac{\partial x_1(t)}{\partial t}+\frac{\partial f}{\partial x_2}\frac{\partial x_2(t)}{\partial t}
$
- If $f(x_1, x_2)$ and $x_1(s, t)\ \&\ x_2(s, t)$

  $\Rightarrow \frac{\partial f}{\partial s}=\frac{\partial f}{\partial x_1}\frac{\partial x_1}{\partial s}+\frac{\partial f}{\partial x_2}\frac{\partial x_2}{\partial s}$

  $\quad\ \frac{\partial f}{\partial t}=\frac{\partial f}{\partial x_1}\frac{\partial x_1}{\partial t}+\frac{\partial f}{\partial x_2}\frac{\partial x_2}{\partial t}$
  $\Rightarrow \frac{df}{d(s, t)}=\frac{df}{\partial\mathbf{x}}\frac{\partial\mathbf{x}}{\partial (s, t)}=[\frac{\partial f}{\partial x_1}\ \frac{\partial f}{\partial x_2}]\begin{bmatrix}
  \frac{\partial x_1}{\partial s} & \frac{\partial x_1}{\partial t} \\\\\\
  \frac{\partial x_2}{\partial s} & \frac{\partial x_2}{\partial t} \\\\\\
  \end{bmatrix}$
  - The chain rule can be written as a matrix multiplication

## 7. Gradients of Vector-Valued Functions

- For $x\in\mathbb{R}^n$, a vector-valued function $f:\mathbb{R}^n\to\mathbb{R}^m$ is $f(x)=\begin{bmatrix}
f_1(x) \\\\\\
f_2(x) \\\\\\
\vdots \\\\\\
f_m(x) \\\\\\
\end{bmatrix}\in\mathbb{R}^m$
- Its partial derivative is given as a vector,
  $\frac{\partial\mathbf{f}}{\partial x_i}=\begin{bmatrix}
\frac{\partial f_1}{\partial x_i} \\\\\\
\frac{\partial f_2}{\partial x_i} \\\\\\
\vdots \\\\\\
\frac{\partial f_m}{\partial x_i} \\\\\\
\end{bmatrix}=\begin{bmatrix}
\underset{h\to 0}{\lim}\frac{f_1(x_1,\ldots,x_{i-1}, x_i+h, x_{i+1},\ldots, x_n)-f_1(\mathbf{x})}{\partial x_i} \\\\\\
\underset{h\to 0}{\lim}\frac{f_2(x_1,\ldots,x_{i-1}, x_i+h, x_{i+1},\ldots, x_n)-f_1(\mathbf{x})}{\partial x_i} \\\\\\
\vdots \\\\\\
\underset{h\to 0}{\lim}\frac{f_m(x_1,\ldots,x_{i-1}, x_i+h, x_{i+1},\ldots, x_n)-f_1(\mathbf{x})}{\partial x_i} \\\\\\
\end{bmatrix}\in\mathbb{R}^m$.
- And its gradient is given as a matrix, by collecting these partial derivatives:
  $\frac{\partial\mathbf{f}}{\partial \mathbf{x}}=\begin{bmatrix}
\frac{\partial \mathbf{f}(\mathbf{x})}{\partial x_1}\ \cdots \frac{\partial \mathbf{f}(\mathbf{x})}{\partial x_n} \\\\\\
\end{bmatrix}=\begin{bmatrix}
\frac{\partial f_1(\mathbf{x})}{\partial x_1} & \cdots & \frac{\partial f_1(\mathbf{x})}{\partial x_n} \\\\\\
\vdots & \ddots & \vdots \\\\\\
\frac{\partial f_m(\mathbf{x})}{\partial x_1} & \cdots & \frac{\partial f_m(\mathbf{x})}{\partial x_n}
\end{bmatrix}\in\mathbb{R}^{m\times n}$.

## 8. Jacobian and Hessian

- Jacobian: The collection of all 1st-order partial derivatives of a function $f:\mathbb{R}^n\to \mathbb{R}^m$.

  $\Rightarrow J = \nabla_x f=\frac{df}{d\mathbf{x}}=[\frac{\partial f(x)}{\partial x_1}\ \cdots \frac{\partial f(x)}{\partial x_n}]$
  $ = \begin{bmatrix}
\frac{\partial f_1(\mathbf{x})}{\partial x_1} & \cdots & \frac{\partial f_1(\mathbf{x})}{\partial x_n} \\\\\\
\vdots & \ddots & \vdots \\\\\\
\frac{\partial f_m(\mathbf{x})}{\partial x_1} & \cdots & \frac{\partial f_m(\mathbf{x})}{\partial x_n}
\end{bmatrix}$
- Hessian: The collection of all second-order partial derivatives
  - If $f:\mathbb{R}^2\to\mathbb{R}$, $H=\begin{bmatrix}
\frac{\partial^2 f}{\partial x^2} & \frac{\partial^2 f}{\partial x\partial y} \\\\\\
\frac{\partial^2 f}{\partial x\partial y} & \frac{\partial^2 f}{\partial y^2}
\end{bmatrix}$
  - If $f:\mathbb{R}^n\to\mathbb{R}^m$, the Hessian is an $(m\times n\times n)$ tensor.

## 9. Useful Identities for Computing Gradients

$\frac{\partial}{\partial \mathbf{X}}f(\mathbf{X})^T = (\frac{\partial}{\partial \mathbf{X}}f(\mathbf{X}))^T$

$\frac{\partial}{\partial \mathbf{X}}tr(f(\mathbf{X})) = tr(\frac{\partial f(\mathbf{X})}{\partial \mathbf{X}})$

$\frac{\partial}{\partial \mathbf{X}}det(f(\mathbf{X})) = det(f(\mathbf{X}))tr(f(\mathbf{X})^{-1}\frac{\partial f(\mathbf{X})}{\partial \mathbf{X}})$

$\frac{\partial}{\partial \mathbf{X}}f(\mathbf{X})^{-1} = -f(\mathbf{X})^{-1}\frac{\partial f(\mathbf{X})}{\partial \mathbf{X}}f(\mathbf{X})^{-1}$

$\frac{\partial a^T\mathbf{X}^{-1}b}{\partial \mathbf{X}} = -(\mathbf{X}^{-1})ab^T(\mathbf{X}^{-1})^T$

$\frac{\partial x^Ta}{\partial x} = a^T$

$\frac{\partial a^Tx}{\partial x} = a^T$

$\frac{\partial a^T\mathbf{X}b}{\partial \mathbf{X}} = ab^T$

$\frac{\partial x^T\mathbf{B}x}{\partial x} = x^T(B+B^T)$

$\frac{\partial}{\partial s}(x-As)^TW(x-As)=-2(x-As)^TWA,\ W^T=W$

These are based on the definition introduced in this slice. If you adopt column shape gradient, this will be little bit different.

## 10. Higher-Dimension Differentiation and Approximation

- Recall $f(x+\epsilon)\approx\epsilon\frac{df}{dx}(x)+f(x)$.
- Similarliy, $f(x_1+\epsilon_1, x_2, \ldots, x_n)\approx f(x_1, x_2, \ldots, x_n)+\epsilon_1\frac{\partial}{\partial x_1}f(x_1, x_2,\ldots, x_n)$
  - If we repeat this, $f(x_1+\epsilon_1, x_2+\epsilon_2, \ldots, x_n+\epsilon_n)\approx f(x_1, x_2, \ldots, x_n)+\underset{i}{\Sigma}\epsilon_i\frac{\partial}{\partial x_i}f(x_1, x_2,\ldots, x_n)$
  - Let $\mathbf{\epsilon}=[\epsilon_1, \ldots, \epsilon_n]^T$ and $\nabla_xf=[\frac{\partial f}{\partial x_1},\cdots, \frac{\partial f}{\partial x_n}]^T$, then $f(\mathbf{x}+\mathbf{\epsilon})\approx f(\mathbf{x})+\mathbf{\epsilon}^T\nabla_xf(\mathbf{x})$
  - Consider $f(\cdot)$ as our objective function $L(\cdot)$, and $\left\lVert \mathbf{\epsilon}\right\rVert=1$
    
    $\Rightarrow L(\theta+\epsilon)\approx L(\theta)+\epsilon\nabla_\theta L(\theta)=L(\theta)+\left\lVert \nabla_\theta L(\theta)\right\rVert\cos(\alpha)$
  - This has the minimul value when $\alpha=\pi$

## 11. Gradients, Backpropagation, Chain Rule, and Neural Network

<p align="center">
<img src="{{site.gdrive_url_prefix}}1DikD2svov6oHiK4EGA7BGP3UrOnDEweb" title="lec06-p13 image" style="float: center; width:100%">
</p>
<p>
$\frac{\partial L}{\partial\mathbf{\theta}_{K-1}}=\frac{\partial L}{\partial f_{K}}\frac{\partial f_K}{\partial\mathbf{\theta}_{K-1}}$<br>
$\frac{\partial L}{\partial\mathbf{\theta}_{K-2}}=\frac{\partial L}{\partial f_{K}}\frac{\partial f_K}{\partial f_{K-1}}\frac{\partial f_{K-1}}{\partial \mathbf{\theta}_{K-2}}$<br>
$\frac{\partial L}{\partial\mathbf{\theta}_{K-2}}=\frac{\partial L}{\partial f_{K}}\frac{\partial f_K}{\partial f_{K-1}}\frac{\partial f_{K-1}}{\partial f_{K-2}}\frac{\partial f_{K-2}}{\partial \mathbf{\theta}_{K-3}}$
</p>
