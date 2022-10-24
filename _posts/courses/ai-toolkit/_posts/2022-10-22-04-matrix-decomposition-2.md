---
title: "[AI toolkit] 04. Matrix Decomposition (2)"
excerpt: "Eigen decomposition, SVD"

categories:
  - AI toolkit
tags:
  - [Linear Algebra, Eigen decomposition, SVD]

permalink: /courses/ai-toolkit/004/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-22
last_modified_at: 2022-10-22
order: 5
published: true
use_math: true

---
# Lecture 04

## 0. Table of Contents

1. [Cholesky Decomposition](#1-cholesky-decomposition)
2. [Eigendecomposition and Diagnoalization](#2-eigendecomposition-and-diagnoalization)
3. [Singular Value Decomposition (SVD)](#3-singular-value-decomposition-svd)

## 1. Cholesky Decomposition

- A symmetric($A=A^T$), positive definite($x^TAx>0$) matrix $A$ can ve factorized into a product, $A=LL^T$, where $L$ is a lower-triangular matrix with positive diagonal elements.
  
    <p align="center">
    $$\begin{bmatrix}
    a_{11} & \cdots & a_{1n} \\
    \vdots & \ddots & \vdots \\
    a_{n1} & \cdots & a_{nn} \\
    \end{bmatrix} = \begin{bmatrix}
    l_{11} & \cdots & 0 \\
    \vdots & \ddots & \vdots \\
    l_{n1} & \cdots & l_{nn} \\
    \end{bmatrix} \begin{bmatrix}
    l_{11} & \cdots & l_{n1} \\
    \vdots & \ddots & \vdots \\
    0 & \cdots & l_{nn} \\
    \end{bmatrix}
    $$
    </p>

- $L$ is called the **Cholesky** factor of $A$, and $L$ is unique.
- If possible, calculating the determinant of $A$ becomes much easier
    <p align="center">
    $$A=LL^T, det(A)=det(LL^T)=det(L)det(L^T)\\
    =det(L)^2=(\Pi_{i=1}^nl_{ii})^2$$
    </p>

 - $3\times 3$ matrix example

    <p align="center">
    $$
    \begin{align*}
    & A = \begin{bmatrix}
    a_{11} & a_{12} & a_{13} \\
    a_{21} & a_{22} & a_{23} \\
    a_{31} & a_{32} & a_{33} \\
    \end{bmatrix}
     = LL^T = \begin{bmatrix}
    l_{11} & 0 & 0 \\
    l_{21} & l_{22} & 0 \\
    l_{31} & l_{32} & l_{33} \\
    \end{bmatrix}\begin{bmatrix}
    l_{11} & a_{21} & a_{31} \\
    0 & l_{22} & a_{32} \\
    0 & 0 & l_{33} \\
    \end{bmatrix} \\
    \Rightarrow\ & A = \begin{bmatrix}
    l_{11}^2 & l_{21}l_{11} & l_{31}l_{11} \\
    l_{21}l_{11} & l_{22}^2+l_{22}^2 & l_{31}l_{21}+l_{32}l_{22} \\
    l_{31}l_{11} & l_{32}l_{21}+l_{32}l_{22} & l_{31}^2+l_{32}^2+l_{33}^2 & \\
    \end{bmatrix} \\
    \end{align*}
    $$
    </p>

## 2. Eigendecomposition and Diagnoalization

- A matrix $A\in\mathbb{R}^{n\times n}$ is diagonalizable if there exist and **invertible** matrix $P\in\mathbb{R}^{n\times n}$ such that
<p align="center">
    $D=P^{-1}AP, A=PDP^{-1}$
</p>

- Define $P:=[p_1,\ldots, p_n]$, and let $D\in\mathbb{R}^{n\times n}$ be a diagonal matrix with diagonal entries $\lambda_1, \ldots, \lambda_n$.
- Then, $AP=PD$ iff $\lambda_1,\ldots, \lambda_n$ are the eigenvalues of $A\in\mathbb{R}^{n\times n}$ and $p_1,\ldots, p_n$ are corresponding eigenvectors of $A$.
- $P\in\mathbb{R}^{n\times n}$ is invertible $\Leftrightarrow$ has full rank $\Leftrightarrow$ linearly independent engenvectors $p_1,\ldots, p_n$ $\Leftrightarrow$ eivenctors form a basis of $\mathbb{R}^{n}$

<p align="center">
$ \begin{bmatrix}
Ap_1 = \lambda_1p_1 \\
\vdots \\
Ap_n = \lambda_np_n \\
\end{bmatrix} \Rightarrow [Ap_1, \ldots, Ap_n] = AP$ $ = [\lambda_1p_1, \ldots, \lambda_np_n]=\begin{bmatrix}
\lambda_1 & \ldots & 0 \\
\vdots & \ddots & \vdots \\
0 & \ldots & \lambda_n \\
\end{bmatrix}P = \Lambda P
$
</p>

- If $A\in\mathbb{R}^{n\times n}$ is symmetric, there exists an orthonormal basis of the corresponding vector space $V$ consisting of eigenvectors of $A$, and each eigenvalues is real ([<span style="color:blue">*Spectral Theorem*</span>](https://mast.queensu.ca/~br66/419/spectraltheoremproof.pdf)).
- A symmetric matrix can always be diagonalized, and $A=PDP^T\ \& \ D=P^TDP$.

    <img src="{{site.gdrive_url_prefix}}1xlRA_GzkmMa5QYqjFx6ufiIjqqcl-cFv" title="lec04-p5-pca image" style="float: center; width:75%">

    - $P^{-1}$ : performs a basis change
    - $D$ : scales the vectors
    - $P$ : transforms back to standard coordinate
- $A^k=PD^kP^{-1}$

    $\Rightarrow A=PDP^{-1}$
    $A^k\rightarrow PDP^{-1}PDP^{-1},\ldots, PDP^{-1}\rightarrow PD(P^{-1}P)DP^{-1},\ldots, PDP^{-1}$
    $\rightarrow PDIDP^{-1},\ldots, PDP^{-1}\rightarrow PD^kP^{-1}$

- $det(A)=\Pi\lambda_i$

    $det(PDP^{-1})=det(P)det(D)det(P^{-1})=det(P)det(D)1/det(P)=det(D)$
    $det(D)=\begin{vmatrix}
    \lambda_i & & \\\\\\
     & \ddots &  \\\\\\
   & & \lambda_n \\\\\\
    \end{vmatrix}=\Pi\lambda_i$

## 3. Singular Value Decomposition (SVD)

- Eigendecomposition requires squre matrix.
- Let $A\in\mathbb{m\times n}$ be a rectangular matrix of rank $r\in[0, min(m, n)]$

    <img src="{{site.gdrive_url_prefix}}1d-KHoM1ob620IdWJgaRVUXytwaMpNR-X" title="lec04-p7-svd image" style="float: center; width:75%">
- $U$: orthogonal matrix with column vectors $u_i$, left-singular vectors
- $V$: orthogonal matrix with column vectors $v_i$, right-singular vectors
- $\Sigma$ : $m\times n$ matrix with $\Sigma_{ii}=\sigma_i\geq0, \Sigma_{ij}=0, i\ne j$
  - diagonal entries $\sigma_i, i=1,\ldots, r$ are singular values where $\sigma_1\geq\sigma_2\geq,\ldots, \sigma_r\geq0$
- For symmetric and positive definite matrix $S\in\mathbb{R}^{n\times n}, S=S^T=PDP^T=U\Sigma V^T$
- For $V^T$,

    $\Rightarrow A^TA=PDP^T=P\begin{bmatrix}
    \lambda_1 & \cdots & 0 \\\\\\
    \vdots & \ddots & \vdots \\\\\\
    0 & \cdots & \lambda_n \\\\\\
    \end{bmatrix}P^T, V^T=P^T, \sigma_i^2=\lambda_i$
    - $(A^TA)P=PD$, ($P$=eigenvectors, $D$=eigenvalues of $A^TA$)
    - $A=U\Sigma V^T, A^TA=V\Sigma^T U^TU\Sigma V^T=V\Sigma^2V^T, (U^TU=I, \Sigma^2=D)$
- For $U$,

    $\Rightarrow (AA^T)Q=QE$, ($Q$=eigenvectors, $E$=eigenvalues of $AA^T$)
  - $A=U\Sigma V^T, AA^T=U\Sigma V^TV\Sigma^T U^T=U(\Sigma\Sigma^T)U^T, (V^TV=I, \Sigma\Sigma^T=E)$
- $A^TA$ and $AA^T$ have a smae eigenvalues.

    $\Rightarrow A(A^TA)v_i=A\lambda_i v_i \rightarrow (AA^T)(Av_i)=\lambda_i (Av_i)=\lambda_i u_i$

- For orthonormal $v_i, v_j, i\ne j$, from their linear transformation by $A$ are orthogonal

    $\Rightarrow (Av_i)^TAv_j=v_i^T(A^TAv_j)=v_i^T\lambda_j v_j=0$
- For $m\geq r, \{Av_i, \ldots, Av_r\}$ is a basis of an r-dimensional subspace of $\mathbb{R}^m$
    $u_i:=\frac{Av_i}{\left\lVert Av_i \right\rVert}=\frac{1}{\sqrt{\lambda_i}}Av_i=\frac{1}{\sigma_i}Av_i, Av_i=\sigma_iu_i,\ i=1, \ldots, r$
  $\left\lVert Av_i\right\rVert^2=v_i^TA^TAv_i=v_i^T\lambda_i v_i=\lambda_i v_i^Tv_i=\lambda_i$
  - If $m \lt n$ and $i\gt m$, $Av_i=0$
      Since $v_i$ are orthonormal, it supplies an orthonormal basis of null space.


### 3.1 Geometric Intuitions for the SVD when $A\in\mathbb{R}^{3\times 2}$

<img src="{{site.gdrive_url_prefix}}193xVv1-ErSfPk1YmZyrq5o8kZ1En6Ic6" title="lec04-p7-svd image" style="float: center; width:75%">

- $V^T$ : performs a basis change in $\mathbb{R}^2$
- $\Sigma$ : scales and maps from $\mathbb{R}^2$ to $\mathbb{R}^3$
- $U$ : performs a basis change with $\mathbb{R}^3$

