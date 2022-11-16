---
title: "[Nonparametric Bayesian] 01. Linear Algebra 1"
excerpt: "Linear Algebra"

categories:
  - Nonparametric bayesian
tags:
  - [Linear Algebra, LU Decomposition, PA=LU]

permalink: /courses/nonparametric-bayesian/001/

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
# Lecture 01

## 0. Table of Contents

1. [n-tuple vector](#1-n-tuple-vector)
2. [PA=LDU](#2-paldu)
3. [Gaussian-Jordan Methods for Finding inv(A)](#3-gauss-jordan-method-for-finding-inva)
4. [Associate Memory](#4-associate-memory)

## 1. n-tuple vector

- Inner product, Dot product
    <p align="center">
      <img src="{{site.gdrive_url_prefix}}1wxc_v9SOvGPLrt2L2LTjfq_EYuSWLw4T" title="lec2-p3 image" style="float: center; width:50%">
    </p>
  - 실수
    - $<x,y>=x\cdot y=x^Ty=\begin{bmatrix} x_1 & x_2 \\ \end{bmatrix} \begin{bmatrix}y_1 \\\\ y_2 \\ \end{bmatrix}=x_1y_1+x_2y_2$
  - 복소수
    - $<x,y>=x\cdot y=y^*x=\begin{bmatrix} \overline{y_1} & \overline{y_2} \\ \end{bmatrix} \begin{bmatrix}x_1 \\\\ x_2 \\ \end{bmatrix}=x_1\overline{y_1}+x_2\overline{y_2}$

## 2. PA=LDU

- 행렬 A는 하삼각행렬(Lower triangular matrix) L과 상삼각행렬(Upper triangular matrix) U의 곱으로 나타낼 수 있다. A=LU
  - 이때 U의 대각행의 원소를 1로 맞추는 U=DU'로 더 나눌 수 있다.
    - $U'=\begin{bmatrix}d_1 & u_{12} & \cdots & u_{1n} \\\\\\
    0 & d_2 & \cdots & u_{2n} \\\\\\ 
    \vdots & \vdots & \ddots & \vdots \\\\\\
    0 & 0 & \cdots & u_{nn} \\\\ \end{bmatrix} = \begin{bmatrix}d_1 & 0 & \cdots & 0 \\\\\\
    0 & d_2 & \cdots & 0 \\\\\\
    \vdots & \vdots & \ddots & \vdots \\\\\\
    0 & 0 & \cdots & d_n \\ \end{bmatrix}\begin{bmatrix}1 & u_{12}/d_1 & \cdots & u_{1n}/d_1 \\\\\\
    0 & 1 & \cdots & u_{2n}/d_2 \\\\\\
    \vdots & \vdots & \ddots & \vdots \\\\\\
    0 & 0 & \cdots & u_{nn}/d_n \\\\ \end{bmatrix}$
  - A를 Gaussian elimination하여 U로 만들면 다음과 같은 형태가 된다.
    - $(E_mE_{m-1}\cdots E_2E_1)A=U$
      - Gaussian elimination은 행렬에 기본 행렬(Elementary matrix)을 곱하는 것으로 볼 수 있다.
      - 이때 Gaussian elimination은 i열을 작업할 때, i행 밑의 원소들을 0으로 만들도록 작업하며, 1열부터 시작한다.
    - $(E_mE_{m-1}\cdots E_2E_1)$의 역함수를 양변에 취해주면 다음과 같다.
      - $A = (E_mE_{m-1}\cdots E_2E_1)^{-1}U=LU$
      - 따라서 $L=(E_mE_{m-1}\cdots E_2E_1)^{-1}=E^{-1}_1E^{-1}_2\cdots E^{-1}_m$
      - $\small E_i=\begin{bmatrix}1 & & & & \\\\\\ & 1 & & & \\\\\\
      & & \ddots & \\\\\\ & & l & 1 & \\\\\\ & & & & 1  \end{bmatrix}$, $E^{-1}_i=\begin{bmatrix}1 & & & & \\\\\\ & 1 & & & \\\\\\
      & & \ddots & \\\\\\ & & -l & 1 & \\\\\\ & & & & 1  \end{bmatrix}$
  - 이를 LU 분해(LU Decomposition)이라고 한다.
    - LU 분해는 왜 하는가?
    - $Ax=y\rightarrow LUx=y$에서 $Ux$와 $L(Ux)$의 계산이 훨씬 간편해진다.
      - A가 나타내는 시스템에 대해 미리 LU 분해로 분석해놓으면, 인풋 x에 대한 아웃풋 y의 계산이 훨씬 간편해진다.
    - $Ax=y\rightarrow x = A^{-1}y, A^{-1}=U^{-1}D^{-1}L^{-1}$
- 만약 Gaussian Elimination 과정 중 두 행을 교환해야하는 경우 치환행렬(Permutation matrix)를 사용해야 한다. ([LU Decomposition with Pivoting](https://www2.math.upenn.edu/~kazdan/320F18/Notes/Gauss-Elim/LU-P.html))
  - 이 경우 전체 식은 다음과 같은 형태가 된다.
    - $L_3P_3L_2P_2L_1P_1A=U$
  - 위 식은 다음과 같은 형태로 변형할 수 있다.
    - $L'_3L'_2L'_1P_3P_2P_1A=U$
    - $L'_3=L_3,\ L'_2=P_3L_2P^{-1}_3,\ L'_1=P_3P_2L_1P^{-1}_2P^{-1}_3$
  - 이 경우 PA=LU는 다음과 같은 형태가 된다.
    - $L=(L'_3L'_2L'_1)^{-1},\ P=P_3P_2P_1$
- 하지만, L'를 구하는 과정이 상당히 많은 계산이 요구된다(역행렬이 포함됨).
  - $L_i$와 $P_j$를 교환하는 과정을 조금 더 효율적으로 계산하는 방법이 있다.([링크](https://www.youtube.com/watch?v=9YmssxzMhAo#t=9m33
))
  - 다음의 경우를 고려해보자.
    - $\scriptsize P=\begin{bmatrix}1 & & & & \\\\\\
    & & & & 1\\\\\\
    & & 1 & & \\\\\\
    & & & 1 & \\\\\\
    & 1 & & & \\\\ \end{bmatrix}$, $\scriptsize A=\begin{bmatrix}1 & & & & \\\\\\
    & 1 & & & \\\\\\
    & 2 & 1 & & \\\\\\
    & 3 & & 1 & \\\\\\
    & 4 & & & 1 \\\\ \end{bmatrix}$
  - P는 위에서 본 치환행렬이다. P가 행렬의 왼쪽에 위치하면 행렬의 행을 바꾸는 역할을 하며, P가 행렬의 오른쪽에 위치하면 행렬의 열을 바꾸는 역할을 한다.
    - $\scriptsize PA=\begin{bmatrix}1 & & & & \\\\\\
    & & & & 1\\\\\\
    & & 1 & & \\\\\\
    & & & 1 & \\\\\\
    & 1 & & & \\\\ \end{bmatrix}\begin{bmatrix}1 & & & & \\\\\\
    & 1 & & & \\\\\\
    & 2 & 1 & & \\\\\\
    & 3 & & 1 & \\\\\\
    & 4 & & & 1 \\\\ \end{bmatrix}=\begin{bmatrix}1 & & & & \\\\\\
    & 4 & & & \\\\\\
    & 2 & 1 & & \\\\\\
    & 3 & & 1 & \\\\\\
    & 1 & & & 1 \\\\ \end{bmatrix}$
    - $\scriptsize AP=\begin{bmatrix}1 & & & & \\\\\\
    & 1 & & & \\\\\\
    & 2 & 1 & & \\\\\\
    & 3 & & 1 & \\\\\\
    & 4 & & & 1 \\\\ \end{bmatrix}\begin{bmatrix}1 & & & & \\\\\\
    & & & & 1\\\\\\
    & & 1 & & \\\\\\
    & & & 1 & \\\\\\
    & 1 & & & \\\\ \end{bmatrix}=\begin{bmatrix}1 & & & & \\\\\\
    & & & & 1\\\\\\
    & & 1 & & 2\\\\\\
    & & & 1 & 3\\\\\\
    & 1 & & & 4\\\\ \end{bmatrix}$
  - 우린 $L_3P_3L_2P_2L_1P_1A=U$의 형태를 $L'_3L'_2L'_1P_3P_2P_1A=U$로 변형하고 싶기 때문에 $PA=A'P$를 위한 $A'$를 찾기를 원한다. 이를 위해 양변에 $P^{-1}$를 오른쪽에서 곱해보자.
    - 치환행렬의 역행렬은 자기 자신이다.
      - 행열을 바꾸는 행위는 한 번 더 바꿈으로써 원래대로 돌아올 수 있다.
    - $PAP=PAP^{-1}=A'PP^{-1}=A'$
    - $\scriptsize (PA)P=\begin{bmatrix}1 & & & & \\\\\\
    & 4 & & & \\\\\\
    & 2 & 1 & & \\\\\\
    & 3 & & 1 & \\\\\\
    & 1 & & & 1 \\\\ \end{bmatrix}\begin{bmatrix}1 & & & & \\\\\\
    & & & & 1\\\\\\
    & & 1 & & \\\\\\
    & & & 1 & \\\\\\
    & 1 & & & \\\\ \end{bmatrix}=\begin{bmatrix}1 & & & & \\\\\\
    & & & & 4\\\\\\
    & & 1 & & 2\\\\\\
    & & & 1 & 3\\\\\\
    & 1 & & & 1\\\\ \end{bmatrix}=A'$
    - A와 A'를 비교해보자. $\scriptsize A=\begin{bmatrix}1 & & & & \\\\\\
    & 1 & & & \\\\\\
    & 2 & 1 & & \\\\\\
    & 3 & & 1 & \\\\\\
    & 4 & & & 1 \\\\ \end{bmatrix}$, $\scriptsize A'=\begin{bmatrix}1 & & & & \\\\\\
    & & & & 4\\\\\\
    & & 1 & & 2\\\\\\
    & & & 1 & 3\\\\\\
    & 1 & & & 1\\\\ \end{bmatrix}$
  - 위의 예시에서 P는 2, 5 행/열을 바꾸는 치환행렬이다. 따라서 A 행렬의 2행과 5행, 2열과 5열을 모두 바꾸어 A'를 얻을 수 있다.
  - 이를 일반화시켜보면, 만약 P가 i, j 행/열을 바꾸는 치환행렬이라면, A'는 A의 i, j 행과 열을 바꾸어 얻을 수 있다.
    - 대각행에 있는 i번째 원소와 j번째 원소는 서로 교체된다.
  - 다시 LU Decomposition의 경우를 생각해보자.
    - $L_1$은 $P_2$에 의해 변형되는데, $P_2$는 2, m(m>2) 행/열을 바꾸는 치환행렬이므로 대각행을 제외하고는 1열에만 값이 있는 $L_1$의 경우 대각행을 제외한 2행과 m행의 원소를 서로 교환하여 $L'_1$을 구할 수 있다. 이때, $L'_1$은 여전히 1열에만 값을 가지고 있으며, 대각행의 원소는 모두 1이므로 서로 교환되어도 변화가 없으므로 대각행을 제외한 원소만을 고려한다.
    - 위 변환을 통해 $L_2P_2L_1P_1$은 $L_2L'_1P_2P_1$이 되었다. $L_2$의 경우 대각행 아래 2열에만 값을 가지고 있으므로 $L_2L'_1$은 대각행 아래 1, 2열에만 원소를 가지고 있다. 
    - $L_2L'_1P_2P_1$은 $P_3$에 의해 변환되는데 $P_3$는 3, n(n>3) 행/열을 바꾸는 치환행렬이므로 대각행 제외 1, 2열에만 값을 가지고 있는 $L_2L'_1$는 대각행을 제외한 3, n행의 원소를 서로 교환하여 $L'_2L''_1$을 을 구할 수 있다.
    - 이와 같은 방식으로 $P_i$를 오른쪽으로 옮김과 동시에 앞서 계산한 $L'$행렬의 대각행을 제외한 행렬의 행을 교환하여 최종 $L'$와 $P$을 구할 수 있다.

## 3. Gauss-Jordan Method for Finding inv(A)

- $AA^{-1}=A^{-1}A=(E_nE_{n-1}\cdots E_2E_1)A=I$
- 즉, A를 I로 바꾸는 기본행렬들의 곱을 찾으면 된다.
- 따라서 기본 행 연산(Elementary Row Operation)을 통해 A를 I로 바꾸면 $A^{-1}$를 구할 수 있다.
  - $[A\vert I]\rightarrow [I\vert A^{-1}]$
- 만약 $A$가 $n\times n$ 행렬이라면 위 연산의 계산복잡도는 다음과 같다.
  - $[A\vert I]$를 기준으로 한 행에 scalar 곱을 하는 연산 (scalar multiplication) : $2n$
  - scalar 곱을 한 행을 다른 행에서 빼는 연산 (addition) : $2n$
    - 행렬의 한 원소를 0으로 바꾸기 위해서는 총 $4n$의 연산이 필요함.
  - 한 열 안에 있는 n-1행의 원소를 0으로 만들기 : $4n\times (n-1)\approx 4n\times n$
  - 모든 열에 대해 위의 연산 시행 : $4n\times n\times n$
- 시간 복잡도 : $4n^3=O(n^3)$

## 4. Associate Memory

이러한 개념이 있다고 소개만 해주셨다. 원본은 논문 여러 장... $\rightarrow$ [언젠가...](/list/000/#1-nonparametric-bayesian)
