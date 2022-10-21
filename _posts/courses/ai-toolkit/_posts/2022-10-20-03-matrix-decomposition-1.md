---
title: "[AI toolkit] 03. Matrix Decomposition (1)"
excerpt: "Determinant and eigenvalue/eigenvector"

categories:
  - AI toolkit
tags:
  - [Linear Algebra]

permalink: /courses/ai-toolkit/003/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-20
last_modified_at: 2022-10-20
order: 4
published: true
use_math: true

---
# Lecture 00

## 0. Table of Contents

1. [Determinant](#1-determinant)
2. [Recall: Geometry of Linear Transformations](#2-recall-geometry-of-linear-transformations)
3. [Properties of Determinant](#3-properties-of-determinant)
4. [Trace](#4-trace)
5. [Characteristic Polynomial](#5-characteristic-polynomial)
6. [Eigenvalues and Eigenvectors](#6-eigenvalues-and-eigenvectors))
7. [Graphical Intuition in Two Dimensions](#7-graphical-intuition-in-two-dimensions)
8. [Eigenvalues and Eigenvectors: Properties](#8-eigenvalues-and-eigenvectors-properties)

## 1. Determinant

- A function that maps $\mathbf{A}$ onto a real number
- $det(\mathbf{A}) or \left\lvert\mathbf{A}\right\rvert, where \mathbf{A}\in\mathbb{R}^{m\times n}$.
- For $\mathbf{A}\in\mathbb{R}^{2\times 2}$
  - $\mathbf{A}^{-1}=\frac{1}{a_{11}a_{22}-a_{12}a_{21}}\begin{bmatrix}
  a_{22} & -a_{12} \\
  -a_{21} & a_{11} \\
  \end{bmatrix}, det(\mathbf{A})=\begin{vmatrix}
  a_{22} & -a_{12} \\
  -a_{12} & a_{11} \\
  \end{vmatrix} = a_{11}a_{22}-a_{12}a_{21}$
- For any square matrix $\mathbf{A}\in\mathbb{R}^{n\times n}$, $\mathbf{A}$ is invertible iff $det(\mathbf{A})\ne 0$, otherwise singular
- Equals the **volume** of a box in n-dimensional space, where the edges of the box come from columns.
<img src="{{site.gdrive_url_prefix}}1G24x-suUr75b-0RNBRx-eIfpDtzEPLuK" title="lec02-p2 image" style="float: center; width:50%">

## 2. Recall: Geometry of Linear Transformations

<img src="{{site.gdrive_url_prefix}}1xWe72i5cgDLR8U_j36Gs_f58E12I86Jx" title="lec03-p3-linear transformation image" style="float: center; width:100%">

- We can call a matrix is **invertible** when we can revert this linear transformation.
- Then what would happen if $det(\mathbf{A})=0$?
- The box area becomes zero, w cannot revert this linear transformation when $\mathbf{Ax}$ is given.

## 3. Properties of Determinant

- For a lower/upper triangular matrix $\mathbf{T}\in\mathbb{R}^{n\times n}$, the determinant is the product of the diagonal elements, $det(\mathbf{T})=\Pi_{i=1}^{n}\mathbf{T}_{ii}$
  - "(Row 1)-scalar*(Row 2)" does not change a determinant

    $\mathbf{A}=\begin{bmatrix}
    a & b \\
    c & d \\
    \end{bmatrix}, \mathbf{A}'=\begin{bmatrix}
    a-kc & b-kd \\
    c & d \\
    \end{bmatrix}\rightarrow det(\mathbf{A}')=ad-kdc-bc+kcd=ad-bc=det(\mathbf{A})$
- $det(\mathbf{A}^{-1})=1/det(\mathbf{A}), det(\mathbf{A})=det(\mathbf{A}^T), det(\mathbf{AB})=det(\mathbf{A})det(\mathbf{B}), det(\lambda\mathbf{A})=\lambda^ndet(\mathbf{A})$
- Swapping two rows/columns change the sign of determinant.
- A square matrix has $det(\mathbf{A})\ne 0$ iff $rank(\mathbf{A})=n$.

## 4. Trace

- The trace of a square matrix $\mathbf{A}\in\mathbb{R}^{n\times n} \Rightarrow tr(\mathbf{A}):=\Sigma_{i=1}^na_{ii}$
- $tr(\mathbf{A}+\mathbf{B})=tr(\mathbf{A})+tr(\mathbf{B})$
- $ tr(\alpha\mathbf{A})=\alpha tr(\mathbf{A})$
- $tr(\mathbf{A}\mathbf{B})=tr(\mathbf{B}\mathbf{A})$
- $tr(\mathbf{I}_n)=n$
- $tr(\mathbf{xy}^T)=tr(\mathbf{yx}^T)=\mathbf{yx}^T$

## 5. Characteristic Polynomial

- $p_\mathbf{A}(\lambda):=det(\mathbf{A}-\lambda\mathbf{I})
= c_0+c_1\lambda+c_2\lambda^2+\cdots+c_{n-1}\lambda^{n-1}+(-1)^n\lambda^n$
- $c_0=det(\mathbf{A})$
- $c_{n-1}=(-1)^{n-1}tr(\mathbf{A})$

## 6. Eigenvalues and Eigenvectors

- For a square matrix, $\mathbf{A}\in\mathbb{R}^{n\times n}, \lambda\in\mathbb{R}$ is an eigenvalues of $\mathbf{A}$, and $x\in\mathbb{R}^{n}\backslash\{0\}$ is the corresponding eigenvector of $\mathbf{A}$, if:
  <p align="center">
  $
  \mathbf{Ax}=\lambda\mathbf{x}
  $
  </p>
- An eigenvector is stretched by a factor of its eigenvalue.
- We need to find a solution of $(\mathbf{A}-\lambda\mathbf{I})x=\mathbf{0}$
    $\Rightarrow$ The vector $\mathbf{x}$ is in the nullspace of $\mathbf{A}-\lambda\mathbf{I}$
    $\Rightarrow \lambda\in\mathbb{R}$ is chosen so that $\mathbf{A}-\lambda\mathbf{I}$ has a nullspace with non-zero vectors
    $\Rightarrow \mathbf{A}-\lambda\mathbf{I}$ needs to be singular, such that $det(\mathbf{A}-\lambda\mathbf{I})=\mathbf{0}$

## 7. Graphical Intuition in Two Dimensions

<img src="{{site.gdrive_url_prefix}}1AsQgd-TV3VPcg2ufNe1ku3OYBQoQASHk" title="lec03-p3-linear transformation image" style="float: center; width:100%">
<img src="{{site.gdrive_url_prefix}}1vXGSDzVc0Y2Dfn1mLDUZFburRfjRYjcO" title="lec03-p3-linear transformation image" style="float: center; width:100%">
<img src="{{site.gdrive_url_prefix}}1Bwej3kezkN-9ybvJGzOaRqXJFtRr4_T5" title="lec03-p3-linear transformation image" style="float: center; width:100%">

## 8. Eigenvalues and Eigenvectors: Properties

- The eigenvectos $\mathbf{x}_1, \ldots, \mathbf{x}_n$ of a matrix $\mathbf{A}\in\mathbb{R}^{n\times n}$ with $n$ distinct eigenvalues $\lambda_1, \ldots, \lambda_n$ are linearly independent.
  - <p>$c_1\mathbf{x}_1+c_2\mathbf{x}_2=\mathbf{0} : \times\mathbf{A}\rightarrow c_1\mathbf{A}\mathbf{x}_1+c_2\mathbf{A}\mathbf{x}_2=c_1\lambda_1\mathbf{x}_1+c_2\lambda_2\mathbf{x}_2=0$<br>
  $c_1\mathbf{x}_1+c_2\mathbf{x}_2=\mathbf{0} : \times\lambda_1\rightarrow c_1\lambda_1\mathbf{x}_1+c_2\lambda_1\mathbf{x}_2=0$<br>
  $\Rightarrow c_2(\lambda_2-\lambda_1)\mathbf{x}_1=0,$
  because $\lambda_1\ne\lambda_2, c_2=0$<br>
  Adding $\mathbf{x}_i$ one by one, we can show L.I.</p>
- Given a matrix $\mathbf{A}\in\mathbb{R}^{n\times n}$, we can always obtain a symmetric, positive semidefinite matrix $\mathbf{S}\in\mathbf{R}^{n\times n}$ by defining $\mathbf{S}:=\mathbf{A}^T\mathbf{A}$
  - If $rank(\mathbf{A})=n, \mathbf{S}:=\mathbf{A}^T\mathbf{A}$ is symmetric, positive definite
    $\Rightarrow \mathbf{x}^T\mathbf{S}\mathbf{x}=\mathbf{x}^T\mathbf{A}^T\mathbf{A}\mathbf{x}=\left\lVert\mathbf{Ax}\right\rVert^2\geq0$
- The deteminant of a matrix $\mathbf{A}\in\mathbb{R}^{n\times n}$ is the product of its eigenvalues
    
    From Characteristic Polynomial, $p_\mathbf{A}(\lambda):=det(\mathbf{A}-\lambda\mathbf{I})=(-1)^n(\lambda-\lambda_1)(\lambda-\lambda_2)\cdots(\lambda-\lambda_n)$
    $=c_0+c_1\lambda+c_2\lambda^2+\cdots+c_{n-1}\lambda^{n-1}+(-1)^n\lambda^n, c_0$
    $=det(\mathbf{A})=p_\mathbf{A}(0)=(-1)^n(-\lambda_1)(-\lambda_2)\cdots(-\lambda_n)$
    $=(-1)^{2n}\lambda_1\lambda_2\cdots\lambda_n=\lambda_1\lambda_2\cdots\lambda_n=c_0=det(\mathbf{A})$
- The trace of a matrix is the sum of its eigenvalues
    $p_\mathbf{A}(\lambda):=det(\mathbf{A}-\lambda\mathbf{I})=(-1)^n(\lambda-\lambda_1)(\lambda-\lambda_2)\cdots(\lambda-\lambda_n)$
    $=c_0+c_1\lambda+c_2\lambda^2+\cdots+c_{n-1}\lambda^{n-1}+(-1)^n\lambda^n$

    Here, $c_{n-1}=tr(A)$, by comparing coefficients of $\lambda^{n-1}$, we can proove it.