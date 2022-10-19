---
title: "[AI toolkit] 02. Analytic Geometry"
excerpt: "Geometric features of linear algebra"

categories:
  - AI toolkit
tags:
  - [Linear Algebra]

permalink: /courses/ai-toolkit/002/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-20
last_modified_at: 2022-10-20
order: 3
published: true
use_math: true

---
# Lecture 02

## 0. Table of Contents

1. [Norms](#1-norms)
2. [L1 and L2 Norm and](#2-l1-and-l2-norm-and-dot-product)
3. [Angles and Distance](#3-angles-and-distance)
4. [Geometry of Linear Transformations](#4-geometry-of-linear-transformations)
5. [Hyperplanes](#5-hyperplanes)
6. [Orthogonality](#6-orthogonality)
7. [Projection onto a Line](#7-projection-onto-a-line)
8. [Projection Matrix](#8-projection-matrix)
9. [Orthonormal Basis and Gram-Schmidt Process](#9-orthonormal-basis-and-gram-schmidt-process)

## 1. Norms

<ul>
  <li> function which assigns a length $\left\lVert\mathbf{x}\right\rVert$ to each vector $\mathbf{x}$. <br>$f(\cdot): \mathbb{V}\rightarrow\mathbb{R}$</li>
  <li>For all scalars $\lambda\in\mathbb{R}$, the following hold for $\mathbf{x, y}\in\mathbb{V}$:</li>
    <ul>
<li>Absolutely homogeneous</li><ul>
$\left\lVert\lambda\mathbf{x}\right\rVert=\left\lvert\lambda\right\rvert\left\lVert\mathbf{x}\right\rVert$<img src="{{site.gdrive_url_prefix}}1G8OnnpWsI6p_cldhN62Ee8CB1s9nrHDY" title="lec02-p5-triangle image" style="float: right; width:50%"></ul>
<li>Triangular inequality</li>
<ul>
$\left\lVert\mathbf{x}+\mathbf{y}\right\rVert\leq \left\lVert\mathbf{x}\right\rVert+\left\lVert\mathbf{y}\right\rVert$
</ul>
<li>Positive semi definite</li>
<ul><li>$\left\lVert\lambda\mathbf{x}\right\rVert\geq 0$</li>
<li>$\left\lVert\lambda\mathbf{x}\right\rVert=0 \Leftrightarrow \mathbf{x}=0$</li>
</ul>
</ul>
</ul>

## 2. L1 and L2 Norm and Dot Product

<div><img src="{{site.gdrive_url_prefix}}17ogxNPXwg4z3yHXv2dGysFO_LF6Q7sz0" title="lec02-p6-1 image" style="float: right; width:20%">
<ul>
<li><b>Manhattan Norm (L1 Norm)</b></li>
<ul>
<li>For $\mathbf{x}\in\mathbb{R}^n$,</li>
<ul>$\left\lVert\mathbf{x}\right\rVert_1\:=\sum_{i=1}^{n} \left\lvert x_i\right\rvert$<br><br></ul></ul>
</ul>
</div>

<div><img src="{{site.gdrive_url_prefix}}1vbG04bNWO23GK23wRcKKwwa-K3wUTbr-" title="lec02-p6-2 image" style="float: right; width:20%;">
<ul>
<li><b>Euclidean Norm (L2 Norm)</b></li>
<ul>
<li>For $\mathbf{x}\in\mathbb{R}^n$,</li>
<ul>$\left\lVert\mathbf{x}\right\rVert_2:=\sqrt{\sum_{i=1}^{n}x_i^2}=\sqrt{\mathbf{x}^T\mathbf{x}}$</ul>
</ul>
</ul></div>

<ul>
<li><b>p-Norm</b></li>
<ul>
<li>For $\mathbf{x}\in\mathbb{R}^n$</li>
<ul>$\left\lVert\mathbf{x}\right\rVert_p:=\big(\sum^n_{i=1}\left\lvert x_i\right\rvert^p\big)^{\frac{1}{p}}$</ul>
</ul>
</ul>
1zS5Wd-D2UEhvP1euNCMr1yDT3nLF8Fz8

## 3. Angles and Distance

<div>
<ul>
<li>Dot Product</li>
<ul>$\mathbf{x}\cdot\mathbf{y}=\mathbf{x}^Y\mathbf{y}=\sum_{i=1}^nx_iy_i=\left\lVert\mathbf{x}\right\rVert\left\lVert\mathbf{y}\right\rVert\cos(\theta)$</ul>
</ul><img src="{{site.gdrive_url_prefix}}1zS5Wd-D2UEhvP1euNCMr1yDT3nLF8Fz8" title="lec02-p6-2 image" style="float: right; width:40%; ">
<li>The angle between two vectors</li>
<ul>$\theta=\arccos(\frac{\mathbf{x}\cdot\mathbf{y}}{\left\lVert\mathbf{x}\right\rVert\left\lVert\mathbf{y}\right\rVert})$<br>
$\cos(\theta)=\frac{\mathbf{x}\cdot\mathbf{y}}{\left\lVert\mathbf{x}\right\rVert\left\lVert\mathbf{y}\right\rVert}$
</ul>
<li>The distance between two vectors</li>
<ul>$d(\mathbf{x}, \mathbf{y}):=\left\lVert\mathbf{x-y}\right\rVert$</ul>
</div>

## 4. Geometry of Linear Transformations
  
  <div><div>
  $\mathbf{A}=\begin{bmatrix}
  a & b \\
  c & d \\
  \end{bmatrix}$<br>
  $
  \begin{align*}
  \mathbf{Ax} & =\begin{bmatrix}
  a & b \\
  c & d \\
  \end{bmatrix}\begin{bmatrix}
  x \\
  y \\
  \end{bmatrix} = \begin{bmatrix}
  ax + by \\
  cx + dy \\
  \end{bmatrix} \\
  & = x\begin{bmatrix}
  a \\
  c \\
  \end{bmatrix} +
  y\begin{bmatrix}
  b \\
  d \\
  \end{bmatrix} = x\begin{Bmatrix}
  \mathbf{A}\begin{bmatrix}
  1 \\
  0 \\
  \end{bmatrix}\end{Bmatrix}
  + y\begin{Bmatrix}\mathbf{A}\begin{bmatrix}
  0 \\
  1 \\
  \end{bmatrix}\end{Bmatrix} \\
  \end{align*}$</div>
  <div>
  We can write a matrix transformation of any vector in terms of how $\mathbf{A}$ transforms two specific vectors, $[1, 0]^T$, $[0, 1]^T$</div>
  <div>
  <img src="{{site.gdrive_url_prefix}}11ZwR_zTA2YUuzaKJ-GAlKEUWKNWw8e_I" title="lec02-p6-2 image" style="float: center; width:80%; ">
  </div>
  <div>Matrix multipliation can skew, rotate, and scale the grid, and grid structure is remained</div>
  </div>

## 5. Hyperplanes

<div>
<img src="{{site.gdrive_url_prefix}}1s-_vko9vB9MNQNgRUCm5mZ7JbXDiPkbU" title="lec02-p6-2 image" style="float: left; width:40%; ">
<ul>
<li>$\mathbf{v}\cdot\mathbf{w}=1$ when the length of projection $\mathbf{v}$ onto the direction of $\mathbf{w}$ is exactly $\frac{1}{\left\lVert\mathbf{w}\right\rVert}$</li>
<li>$\mathbf{v}\cdot\mathbf{w} \gt 1$ or $\mathbf{v}\cdot\mathbf{w}\lt1$ divides the space.</li>
<li>Linear Classification<br>
$\Rightarrow$ a method finding hyperplanes (decision planes) that separates the different target classes</li>
</ul><br>
</div>

## 6. Orthogonality

<div>
<ul>
<li>
$\mathbf{x}$ and $\mathbf{y}$ are orthogonal vectors if and only if $\mathbf{x}\cdot$\mathbf{y}=0$.</li>
<ul><li>If $\left\lVert\mathbf{x}\right\rVert=\left\lVert\mathbf{y}\right\rVert=1, \mathbf{x} and \mathbf{y}$ are orthonormal</li></ul>
<li>If nonzero vectors $\mathbf{v}_1, \ldots, \mathbf{v}_k$ are mutuall orthogonal (every vector is perpendicular to every other), then those vectors are linearly independent</li>
<ul>$\mathbf{v}_i^T\mathbf{v}_j=\begin{cases}
\begin{matrix}
0 & if & i\ne j\\
\left\lVert\mathbf{v}_i\right\rVert^2 & if & i=j
\end{matrix}
\end{cases}$<br>
$c_1\mathbf{v}_1+\cdots+c_k\mathbf{v}_k=0$
by multiplying $\mathbf{v}_i^T$, we can get $c_i=0$<br>
$\mathbf{v}_i^T(c_1\mathbf{v}_1+\cdots+c_k\mathbf{v}_k)=0$<br>
$c_i\left\lVert\mathbf{v}_i\right\rVert^2=0\rightarrow c_i=0\ (\left\lVert\mathbf{v}_i\right\rVert\ne 0)$
</ul>
<li>A square matrix $\mathbf{A}\in\mathbb{R}^{n\times n}$ is an orthogonal matrix if and only if its columns are orthonormal, so that $\mathbf{AA}^T=\mathbf{I}=\mathbf{A}^T\mathbf{A}\rightarrow \mathbf{A}^{-1}=\mathbf{A}^T$</li>
<li>Linear transformations by orthogonal matrices does not chane...(they just rotate/flip!)</li>
<ul>
<li>The length of vector</li>
<li>The angle between any two vectors</li>
<ul>$\left\lVert\mathbf{Ax}\right\rVert^2=\mathbf{x}^T\mathbf{A}^T\mathbf{A}\mathbf{x}=\mathbf{x}^T\mathbf{I}\mathbf{x}=\mathbf{x}^T\mathbf{x}=\left\lVert\mathbf{x}\right\rVert^2=1$.<br>
$angle=\frac{\mathbf{x}^T\mathbf{y}}{\left\lVert\mathbf{x}\right\rVert\left\lVert\mathbf{y}\right\rVert}$</ul>
</ul>
</ul>
</div>

## 7. Projection onto a Line

<div>
<img src="{{site.gdrive_url_prefix}}1iuKIKYyezZlxopZ9Ivs4LxavTd5c4MT0" title="lec02-p6-2 image" style="float: right; width:30%; ">
<ul>
<li>Looking for a line for the point $\mathbf{p}$ on $\mathbf{a}$ closest to $\mathbf{b}$</li>
<ul><li>The line connecting $\mathbf{b}$ to $\mathbf{p}$ is perpendicular to $\mathbf{a}$</li></ul>
<li>The point must be some $\mathbf{p}=k\mathbf{a},$</li>
<ul>$k=\frac{\mathbf{a}^T\mathbf{b}}{\mathbf{a}^T\mathbf{a}}$<br>
$\Rightarrow \mathbf{p}^T(\mathbf{b}-\mathbf{p})=0$<br>
$k\mathbf{a}^T(\mathbf{b}-k\mathbf{a})=k\mathbf{a}^T\mathbf{b}-k^2\mathbf{a}^T\mathbf{a}=0$<br>
$k\mathbf{a}^T\mathbf{a}=\mathbf{a}^T\mathbf{b}$</ul>
<li>And, $\left\lVert\mathbf{e}\right\rVert^2=\left\lVert\mathbf{b}-k\mathbf{a}\right\rVert^2=\left\lVert\mathbf{b}-\frac{\mathbf{a}^T\mathbf{b}}{\mathbf{a}^T\mathbf{a}}\right\rVert^2\geq 0\Rightarrow$ Schwarz inequality $\left\lvert\mathbf{a}^T\mathbf{b}\right\rvert\leq\left\lVert\mathbf{a}\right\rVert\left\lVert\mathbf{b}\right\rVert$, eqaulity holds for $\theta=0$ or $\theta=\pi$</li>
</ul>
</div>

## 8. Projection Matrix

<div>
<ul>
<li>Projection of $\mathbf{b}$ on $\mathbf{a}$ is carried out with matrix $\mathbf{P}$,</li>
<ul><li>$\mathbf{p}=k\mathbf{a}=\mathbf{Pb}$</li>
<li>$k=\frac{\mathbf{a}^T\mathbf{b}}{\mathbf{a}^T\mathbf{a}}\Rightarrow \mathbf{P}=\frac{\mathbf{a}\mathbf{a}^T}{\mathbf{a}^T\mathbf{a}}$</li></ul>
</ul>
<ol>
<li>$\mathbf{P}$ is a symmetric matrix</li>
<li>Its square is itself: $\mathbf{P}^2=\mathbf{P}$</li>
<li>Its rank is 1</li></ol></div>

## 9. Orthonormal Basis and Gram-Schmidt Process

<div>
<ul>
<li>A basis ${\mathbf{q}_1, \ldots, \mathbf{q}_n}$ of a $\mathbb{V}$ is orthonormal if $\left\lVert\mathbf{q}_i\right\rVert=1$ and mutually orthogonal.</li>
<li>Gram-Schmidt Process: Find a proper way to make vectors "orthonormal"</li>
<ul>Example) $\mathbf{a}, \mathbf{b}, \mathbf{c}\rightarrow \mathbf{q}_1, \mathbf{q}_2, \mathbf{q}_3$<br>
$\mathbf{q}_1=\mathbf{a}/\left\lVert\mathbf{a}\right\rVert$<br>
$\mathbf{m}=\mathbf{b}-(\mathbf{q}_1^T\mathbf{b})\mathbf{q}_1, \mathbf{q}_2=\mathbf{m}/\left\lVert\mathbf{m}\right\rVert$<br>
$\mathbf{n}=\mathbf{c}-(\mathbf{q}_1^T\mathbf{c})\mathbf{q}_1-(\mathbf{q}_2^T\mathbf{c})\mathbf{q}_2, \mathbf{q}_3=\mathbf{n}/\left\lVert\mathbf{n}\right\rVert$</ul>
<li>To substract from every new vector, its components in the directions that are already settled.</li>
</ul></div>
