---
title: "[AI toolkit] 05. Matrix Approximation, PCA"
excerpt: "Principal Componene Analysis, PCA"

categories:
  - AI toolkit
tags:
  - [Linear Algebra, Principal Component Analysis, PCA]

permalink: /courses/ai-toolkit/005/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-22
last_modified_at: 2022-10-22
order: 6
published: true
use_math: true

---
# Lecture 05

## 0. Table of Contents
1. [Matrix Approximation](#1-matrix-approximation)
2. [Dimensionality Reduction](#2-dimensionality-reduction)
3. [Feature Extraction](#3-feature-extraction)
4. [Principal Component Analysis](#4-principal-component-analysis-pca)
5. [Maxiumum Variance Formulation](#5-maximum-variance-formulation)
6. [PCA in General](#6-pca-in-general)
7. [PCA algorithm: Disadvantages](#7-pca-algorithm-disadvantages)

## 1. Matrix Approximation

<div>
<img src="{{site.gdrive_url_prefix}}15yBYG4uz6f4a6tvlQQ2kjRK7fPBJ6gUQ" title="lec05-p2-svd image" style="float: right; width:50%">
<ul>
<li>$A_i:=u_iv_i^T$</li>
<ul><li>Rank-1 Matrix formed by outer product</li></ul>
<li>$A:=\Sigma_{i=1}^r\sigma_iu_iv_i^T=\Sigma_{i=1}^r\sigma_iA_i$</li>
<ul><li>Rank-r matrix formed by summing r number of rank-1 matrices</li></ul>
<li>$\hat{A}(k):=\Sigma_{i=1}^k\sigma_iu_iv_i^T=\Sigma_{i=1}^k\sigma_iA_i$</li>
<ul><li>Rank-k approximation matrix formed by summing k number of rank-1 matrices<br><br>$\frac{\left\lVert A-\hat{A}(100)\right\rVert}{\left\lVert A \right\rVert}\lt \delta$</li></ul>
</ul>
<img src="{{site.gdrive_url_prefix}}1a8Qn8gI0FiokMM8zep1u-wQM3taav6rS" title="lec05-p3 image" style="float: center; width:100%">
</div>

## 2. Dimensionality Reduction

<img src="{{site.gdrive_url_prefix}}1rRYXq4kSmbOdrBW-Jh5c3xk8lRepbDyU" title="lec05-p5 image" style="float: center; width:100%">

- Consider 3-class patter recognition task with equally spaced bins.
- If the data dimension increases, the number of possible patters exponentially increases.
- So, we need dimensionality reduction,
  - To improve accuracy by learning a target function only from relevant features.
  - And intrinsic dimensionality of data can be smaller than the number of used features.

<ol>
<li>Feature extraction</li>
<ul>Creating a subset of new features by combinations of existing features $m\gt k$.<br>
$\begin{bmatrix}
x_1\\ x_2\\ \vdots \\x_m\\ \end{bmatrix}\rightarrow \begin{bmatrix}
y_1\\ y_2\\ \vdots \\y_k\\ \end{bmatrix}=f\begin{pmatrix}
x_1 \\ x_2 \\ \vdots \\ x_m \end{pmatrix}$</ul>
<li>Feature selection</li>
<ul>Choosing a subset of features that are more informative<br>
$\begin{bmatrix}
x_1 \\ x_2 \\ \vdots \\ x_m \end{bmatrix} \rightarrow 
\begin{bmatrix}
x_{i_1} \\ x_{i_2} \\ \vdots \\ x_{i_k} \end{bmatrix}$</ul>
</ol>

## 3. Feature Extraction

- Given a feature space, to find a mapping $y=f(x)$ such that the reduced feature vector $y\in\mathbb{R}^k$ preserves most of the information or structure of original feature $x$.
- We want optimal mapping functino $f(x)$ that does not increase the min. probability of error.
- In general, the mapping function can be a nonlinear function.
  - But often the feature extraction is limited to linear transformations.

<p align="center">
$\begin{bmatrix}
x_1\\ x_2\\ \vdots \\x_m\\ \end{bmatrix}\rightarrow \begin{bmatrix}
y_1\\ y_2\\ \vdots \\y_k\\ \end{bmatrix}=\begin{bmatrix}
u_{11} & u_{12} & \cdots & u_{1m} \\
u_{21} & u_{22} & \cdots & u_{2m} \\
& & \vdots & \\
u_{k1} & u_{k2} & \cdots & u_{km} \\
\end{bmatrix}\begin{bmatrix}
x_1 \\ x_2 \\ \vdots \\ x_m \\ \end{bmatrix}$
</p>

- Unsupervised approaches
  - Principal Component Analysis
  - Singular Value Decomposition
  - Independent Components Analysis
  - Hidden Layers of Neural Network (VAE)
- Supervised approachees
  - Fisher Linear Discriminant Analysis
  - Hidden Layers of Neural Network (general)

## 4. Principal Component Analysis (PCA)

- Given data points ($x\in\mathbb{R}^m$) in m-dimensional space, project them into lower-dimensional space ($y\in\mathbb{R}^k$) while preserving as much information as possible
    <p><img src="{{site.gdrive_url_prefix}}1ooum-VsxDwk6Go7avPJejU-No8CcBrmZ" title="lec05-p9 image" style="float: center; width:100%">
    </p>

  - Choose a line that fits the data, so the points are spread out well along line.
  - Seek a projection representing data, by minimizing sum of squares of distances to the line.
  - PCA project the data onto a smaller space **while maximizing the variance of the projected data**.

## 5. Maximum Variance Formulation

- Project the data onto a space having dimensionality $k\lt m$ while maximing the variance of the projected data.
  - Consider $k=1$, and m-dimensional unit vector $u_1=[u_{11}, u_{12}, \ldots, u_{1m}]^T, u_1^Tu_1=1$.
  - Each data point $x^{(i)}$ is projected to the scalar $y^{(i)}=u_1^Tx^{(i)}$
  - Mean and Covariance of original data $x^{(i)}$,
    - $\mu=\frac{1}{n}\Sigma_{i=1}^nx^{(i)}, S=\Sigma_{i=1}^n(x^{(i)}-\mu)(x^{(i)}-\mu)^T$
  - Mean of projected data: $u_1^T\mu$
  - Variance of projected data: $\frac{1}{n}\Sigma_{i=1}^n(u_1^Tx^{(i)}-u_1^T\mu)(u_1^Tx^{(i)}-u_1^T\mu)^T=u_1^TSu_1$
  - Maximize the projected data's variance $u_1^TSu_1$ with respect to $u_1$
      <p align="center">
        $\frac{\partial}{\partial u_1}(u_1^TSu_1-\lambda_1(1-u_1^Tu_1))=0, Su_1=\lambda_1u_1$
      </p>
  - Variance will be maximum when $u_1$ is the eigenvector with the largest eigenvalue $\lambda_1$.
  - If $k$ increase, choose $u_1, \ldots, u_k$ with eig.vec. to $k$ largest eig.val. $\lambda_1, \lambda_2, \ldots, \lambda_k$.

## 6. PCA in General
<p align="center">
$\begin{bmatrix}
x_1\\ x_2\\ \vdots \\x_m\\ \end{bmatrix}\rightarrow \begin{bmatrix}
y_1\\ y_2\\ \vdots \\y_k\\ \end{bmatrix}=\begin{bmatrix}
u_{11} & u_{12} & \cdots & u_{1m} \\
u_{21} & u_{22} & \cdots & u_{2m} \\
& & \vdots & \\
u_{k1} & u_{k2} & \cdots & u_{km} \\
\end{bmatrix}\begin{bmatrix}
x_1 \\ x_2 \\ \vdots \\ x_m \\ \end{bmatrix}$
</p>

- $\{u_j\}$ are orthogonal basis vectors
- $u_1=[u_{11}, u_{12}, \ldots, u_{1m}]^T$ is 1st eigenvector of covariance matrix, the first principal component.
- $u_2=[u_{21}, u_{22}, \ldots, u_{2m}]^T$ is 2nd eigenvector of covariance matrix, the second principal component.
- $u_k=[u_{k1}, u_{k2}, \ldots, u_{km}]^T$ k-th 1st eigenvector of covariance matrix, the k-th principal component.
- Often the data is made zero-mean as a preprocess step.

## 7. PCA algorithm: Disadvantages

<div>
<p><img src="{{site.gdrive_url_prefix}}1eW2_dshjxHiC1ym8Ad7j4gkquX-hC-6t" title="lec05-p14 image" style="float: right; width:50%">
</p>
<ul>
<li>PCA does not consider class separability since it does not take into account class label of the feature vector.</li>
<li>There is no guarantee that the directions of maximum variance will contain good features for discrimination.</li>
<li>PCA simply performs a coordinate rotation that aligns the transformed axes with the directions of maximum variance.</li>
</ul>
</div>