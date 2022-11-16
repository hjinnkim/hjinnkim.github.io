---
title: "[Nonparametric Bayesian] 02. Linear Algebra 2"
excerpt: "Linear Algebra"

categories:
  - Nonparametric bayesian
tags:
  - [Linear Algebra, Determinant, Norm, Orthogonalization, Eigenvalue, Eigenvector, SVD, Pseudo Inverse, Positive definite]

permalink: /courses/nonparametric-bayesian/002/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-11-14
last_modified_at: 2022-11-14
order: 1
published: true
use_math: true

---
# Lecture 02

## 0. Table of Contents

1. [Determinant](#1-n-tuple-vector)
2. [Vector Norm](#2-vector-norm)
3. [Matrix Norm](#3-matrix-norm)
4. [Orthogonralization](#4-orthogonalization)
5. [Eigenvalues and Eigenvectors](#5-eigenvalues-and-eigenvectors)
6. [SVD](#6-svd-singular-value-decomposition)
7. [Pseudo Inverse](#7-pseudo-inverse)
8. [Derivative of Matrix/Vector Function](#8-derivative-of-matrix-vector-function)
9. [Positive Definite Matrices](#9-positive-definite-matrices)

## 1. Determinant

- 행렬식(Determinant)의 특징들:
  - $det(AB)=det(A)det(B)$
  - $\underset{\text{Row Swap}}{A\rightarrow B} \Rightarrow det(A)=-det(B)$
  - $\underset{\text{Scalar Multiplication}}{A\rightarrow B} \Rightarrow det(A)=-det(B)$
  - $\underset{\text{Row Addition}}{A\rightarrow B} \Rightarrow det(A)=det(B)$
  - $det(A)=det(A^T)$
  - For $Ax=b$ $\rightarrow$ $x_k = \frac{1}{det(A)}det(a_1, a_2,\ldots, b, \ldots, a_n)$ where $a_i:=\text{i-th column of A}$ : Cramer's rule

## 2. Vector Norm

- Norm
  - 벡터의 길이(Length) 혹은 크기(Magnitude)를 측정
  - For a vector $x=[x_1,\ldots, x_n]^T$
    - $\Vert x\Vert_p=(\vert x_1\vert^p+\vert x_2\vert^p+\cdots+\vert x_n\vert^p)^\frac{1}{p}$ : p-norm
    - $\Vert x\Vert_\infty=\underset{i}{\max}\vert x_i\vert$

## 3. Matrix Norm

- Induced Norm
  - $\Vert A\Vert_p=\underset{\Vert x\Vert_p=1}{\sup}\Vert Ax\Vert_p$
  - $\Vert A\Vert_1=\underset{1\leq j\leq n}{\max}\Sigma^m_{i=1}\vert a_{ij}\vert$
  - $\Vert A\Vert_\infty=\underset{1\leq i\leq m}{\max}\Sigma^n_{i=1}\vert a_{ij}\vert$
- Schatten Norm
  - <p>
  $\Vert A\Vert_p=(\Sigma^{\min\{m, n\}}_{i=1}\sigma^p_{i}(A))^\frac{1}{p}\rightarrow \sigma=eigenvalue$</p>
- Spectral Norm(Maximum Singular Value Norm) [참고](http://faculty.washington.edu/trogdon/105A/html/Lecture17.html)
  - <p>$f(A)=\Vert A\Vert_2=\sigma_{\max}(A)=(\lambda_{\max}(A^TA))^{1/2}$<br>$\begin{matrix}
  \Vert A\Vert_2& =\underset{\Vert x\Vert_2=1}{\sup}\Vert{Ax}\Vert_2\hfill \\
  & =\underset{\Vert x\Vert_2=1}{\sup}(x^TA^TAx)^{1/2}\hfill &\\
  & =\underset{\Vert x\Vert_2=1}{\sup}(x^TV\Lambda V^Tx)^{1/2}\hfill& (A=U\Sigma V^T\rightarrow \text{SVD})\\
  & =\underset{\Vert x\Vert_2=1}{\sup}(x^TQ^T\Lambda Qx)^{1/2}\hfill&\\
  & =\underset{\Vert y\Vert_2=1}{\sup}(y^T\Lambda y)^{1/2}\hfill& (y^Ty=x^TQ^TQx=x^TVV^Tx=x^Tx=1)\\
  & =\underset{\Vert y\Vert_2=1}{\sup}(\Sigma y^2_i\lambda_i)^{1/2}\hfill&\\
  & =(\lambda_{\max}(A^TA))^{1/2}\hfill&
  \end{matrix}$</p>
- Frobenius Norm
  - $\Vert{A}\Vert_F=\sqrt{\Sigma^m_{i=1}\Sigma^n_{j=1}\vert{a_{ij}}\vert^2}=\sqrt{trace(A^TA)}=\sqrt{\Sigma^{\min(m,n)}_{i=1}\sigma^2_i(A)}$
  
- 언젠가 좀 더 [자세히...](/list/000/#1-nonparametric-bayesian)

## 4. Orthogonalization

- 대각화
  <p align="center">
      <img src="{{site.gdrive_url_prefix}}1QHt-arGTRZx0gpef6YQ7NkSW2s39EkAF" title="lec3-p9 image" style="float: center; width:90%">
    </p>

## 5. Eigenvalues and Eigenvectors

- 고윳값(Eigenvalue)과 고유벡터(Eigenvector):
  - <p>$\begin{matrix}Av_i=\lambda_iv_i, & i=1,2,\ldots,N\end{matrix}$<br>
  $\begin{matrix}[A=\lambda_i I]v_i=0 & \Rightarrow \vert{A-\lambda_i I}\vert=0 \text{ has N $\lambda_i$ (can be duplicated)}& \\
  & \Rightarrow \exists\ v_i\neq 0 \text{ since $\rho[A-\lambda_i I]\leq N-1\ \ \forall\lambda_i$}\hfill & \text{$\vert{A-\lambda_i I}\vert=0\rightarrow$ not full rank}\\
  & \Rightarrow \exists \text{ N linearly independent eigenvectors {$v_i$}} \hfill & \\
  & \Rightarrow \{v_i\} \text{ becomes a basis of the eigenspace $E$ of $A$} \hfill & \\
  & \Rightarrow \{v_i\} \text{ spans the eigenspaceof $A$} \hfill & \\ & \Rightarrow A\text{ is diagonalizable if $\lambda_i$ are distinctive} \hfill & \\ \end{matrix}$</p>
  - 만약 $\lambda_i$가 distict하지 않는다면, A는 대각화가 불가할 수도 있다.
  - $Av_i=\lambda_iv_i\rightarrow AV=V\Lambda$
    - $V=[v_1\ v_2\cdots v_n]$, $\small \Lambda=\begin{bmatrix}
    \lambda_1 & 0 & \cdots & 0 \\\\\\ 
    0 & \lambda_2 & \cdots & 0 \\\\\\ 
    \vdots & \vdots & \ddots & 0 \\\\\\ 
    0 & 0 & \cdots & \lambda_n \end{bmatrix}$
    - $AV=V\Lambda\rightarrow A=V\Lambda V^{-1}\rightarrow \Lambda=V^{-1}AV$
    - 복소수 고윳값에 대해서는 $V^{-1}=V^*$
- Jordan Canonical Form
  - Generalized Eigenvector $v_{ip}$ for $\lambda_i:(A-\lambda_i I)^pv_{ip}=0$
  - $AV=VJ$ where $J$ has 1 above the repeated eigenvalue
  - 더 상세한 설명[[1]](https://www.justinmath.com/generalized-eigenvectors-and-jordan-form/)[[2]](https://mathcs.holycross.edu/~spl/old_courses/304_fall_2008/handouts/jordan.pdf)[[3]](<https://www.math.purdue.edu/~eremenko/dvi/lect4.2a.pdf>)
  - 그래도 [언젠가...](/list/000/#1-nonparametric-bayesian)

## 6 SVD (Singular Value Decomposition)

- 설명은 다음 [링크](/courses/ai-toolkit/004/#3-singular-value-decomposition-svd)로 대체한다.

## 7. Pseudo Inverse

- 의사역행렬. 원래 이름은 Moore–Penrose inverse
- $Ax=b$를 풀기 위해 $A^{-1}$를 사용하려면 A가 정방행렬이어야 한다. 하지만 많은 경우 A는 정방행렬이 아니다.
- 정방행렬이 아닌 A의 역행렬처럼 작동하는 것이 의사역행렬이다.
  - $A^+=(A^TA)^{-1}A^T$ 
  - $A^+A=(A^TA)^{-1}A^TA=I$
  - $Ax=b\rightarrow A^+Ax=x=A^+b=(A^TA)^{-1}A^Tb$
- SVD 관점에서의 $A^+$
  - $A=U\Sigma V^T\rightarrow (A^TA)^{-1}A^T=(V\Sigma^TU^TU\Sigma V^T)^{-1}V\Sigma^T U^T$
    $=(V\Sigma^T\Sigma V^T)^{-1}V\Sigma U^T=V(\Sigma^T\Sigma)^{-1}V^{-1}V\Sigma U^T=V(\Sigma^T\Sigma)^{-1}\Sigma^T U^T$
  - <p>이때, $(\Sigma^T\Sigma)^{-1}$의 경우 대각행에 $1/\sigma^2_i $ 를 가지고 있으므로, $(\Sigma^T\Sigma)^{-1}\Sigma^T=diag_{n,m}(\sigma^{-1}_1, \sigma^{-1}_2,\ldots,\sigma^{-1}_{\min{\{m,n\}}})$<br>$=diag_{n,m}(\sigma^{+}_1, \sigma^{+}_2,\ldots,\sigma^{+}_{\min{\{m,n\}}})=\Sigma^+$로 나타낸다. ($\Sigma=diag_{m,n}$)</p>
  - 따라서 $A^+=V\Sigma^+U^T$
- 참고: [위키백과](https://ko.wikipedia.org/wiki/%EB%AC%B4%EC%96%B4-%ED%8E%9C%EB%A1%9C%EC%A6%88_%EC%9C%A0%EC%82%AC%EC%97%AD%ED%96%89%EB%A0%AC#%ED%8A%B9%EC%9E%87%EA%B0%92_%EB%B6%84%ED%95%B4%EB%A1%9C%EC%9D%98_%ED%91%9C%ED%98%84), [의사역행렬의 기하학적 의미](https://angeloyeo.github.io/2020/11/11/pseudo_inverse.html)
- 의사역행렬을 이용하여 구한 해의 의미
  - $x=A^+b=(A^TA)^{-1}A^Tb$
  - <p align="center">
      <img src="{{site.gdrive_url_prefix}}1vUGPRrxSPQ1h9qQ_seSrwRtp0DDKKl3I" title="lec3-p33 image" style="float: center; width:90%">
    </p>
  - $Ax$와 $b$의 오차($e$)를 최소화시키는 해
- 이를 Least Squares 관점에서 바라볼 경우
  - $\hat{x}=arg\underset{x}{\min}\Vert{Ax-b}\Vert^2_2\triangleq E(x)$
  - $\nabla_xE(x)=\frac{dE(x)}{dx}=\frac{d}{dx}(Ax-b)^T(Ax-b)=2A^T(Ax-b)=0$
  - $\hat{x}=(A^TA)^{-1}A^Tb$
- 얘도 [언젠가](/list/000/#1-nonparametric-bayesian)

## 8. Derivative of Matrix-Vector Function

- Gradient of $f(x): \nabla_xf(x)=\small \begin{bmatrix}\frac{\partial f}{\partial x_1}\\\\\\ \frac{\partial f}{\partial x_2}\\\\\\ \vdots\\\\\\ \frac{\partial f}{\partial x_n} \end{bmatrix}$
- Hessian of $f(x): H(f)=\small \begin{bmatrix}
\frac{\partial^2 f}{\partial x^2_1} & \frac{\partial^2 f}{\partial x_1\partial x_2} & \cdots & \frac{\partial^2 f}{\partial x_1\partial x_n} \\\\\\
\frac{\partial^2 f}{\partial x_2\partial x_1} & \frac{\partial^2 f}{\partial x^2_2\partial x_2} & \cdots & \frac{\partial^2 f}{\partial x_2\partial x_n} \\\\\\
\vdots & \vdots & \ddots & \vdots \\\\\\
\frac{\partial^2 f}{\partial x_nx_1} & \frac{\partial^2 f}{\partial x_n\partial x_2} & \cdots & \frac{\partial^2 f}{\partial x^2_n}\end{bmatrix}$
- Jacobian of $\mathbf{f}(x)$: First derivative of a vector function $\mathbf{f}(x)=[f_1(x),\ldots,f_m(x)]^T$
  - $\mathbf{f}(x)=\begin{bmatrix}
  f_1(x) \\\\\ f_2(x) \\\\\ \vdots \\\\\\ f_m(x)\end{bmatrix}\rightarrow \mathbf{J}=[\frac{\partial\mathbf{f}}{\partial x_1}\cdots \frac{\partial\mathbf{f}}{\partial x_n}]=\begin{bmatrix}\frac{\partial f_1}{\partial x_1} & \cdots & \frac{\partial f_1}{\partial x_n} \\\\\\ \vdots & \ddots & \vdots \\\\\\ \frac{\partial f_m}{\partial x_1} & \cdots & \frac{\partial f_m}{\partial x_n}\end{bmatrix}$
- $\frac{d}{dx}Ax=A^T$, $\frac{d}{dx}x^TA=A$

## 9. Positive Definite Matrices

- <p align="center">
      <img src="{{site.gdrive_url_prefix}}1WvWVQdoE9bg2DI6TgMHiTAcCiu0Eijc2" title="lec3-p48 image" style="float: center; width:90%">
    </p>
- Definiteness of Matrix
  - $f(x)=x^THx+b^Tx$
    - If $H>0$ is positive definite matrix, all $\lambda_i(H)>0$
    - If $H\geq 0$ is positive definite matrix, all $\lambda_i(H)\geq 0$
    - If $H<0$ is negative definite matrix, all $\lambda_i(H)<0$
    - If $H\leq 0$ is negative definite matrix, all $\lambda_i(H)\leq 0$
    - If $H$ is indefinite matrix, some $\lambda_i(H)>0, some $\lambda_i(H)<0$
  - A is positive definite matrix, $i.e.\ A>0$ iff $v^TAv>0$ for $\forall v\neq 0$
  - A is positive semi-definite matrix, $i.e.\ A\leq 0$ iff $v^TAv\leq 0$ for $\forall v\neq 0$
- 얘도 [언젠가...](/list/000/#1-nonparametric-bayesian)
