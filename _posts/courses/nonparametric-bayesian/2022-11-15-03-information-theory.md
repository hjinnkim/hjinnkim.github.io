---
title: "[Nonparametric Bayesian] 03. Information Theory"
excerpt: "Linear Algebra"

categories:
  - Nonparametric bayesian
tags:
  - [Information Theory, Entropy, KL Divergence, Cross Entropy, Mutual Information]

permalink: /courses/nonparametric-bayesian/003/

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
# Lecture 03 Information Theory

## 0. Table of Contents

1. []()

## 1. Information

- 정보의 기댓값(Expectation of information)
- $H(X)=E[I(X)]=\Sigma^K_{k=-K}p_kI(x_k)=-\Sigma^K_{k=-K}p_k\log{p_k}$
- 엔트로피 최댓값
  - $p_k$=uniform
  - $0\leq H(X)\leq -\Sigma^K_{k=-K}\frac{1}{2K+1}\log{\frac{1}{2K+1}}=\log{2K+1}$
- $p_k=$0 or 1이면 정보는 0$\rightarrow $따라서 엔트로피도 0
- Theorem(Gray, 1990)
  - $\Sigma_Kp_k\log{\frac{p_k}{q_k}}\geq 0$
- Relative entropy(or Kullback-Leibler divergence, KL divergence)
  - $D_{p\Vert{q}}=\Sigma_{x\in X}p_X(x)\log{\frac{p_X(x)}{q_X(x)}}$
  - 이때 $p_X(x)$는 기준 pmf, $q_X(x)$는 상대 pmf
- KL divergence for NN
  - $D_{p\Vert{q}}=\Sigma_{x\in X}p_X(x)\log{\frac{p_X(x)}{q_X(x;W)}}=\Sigma_{x\in X}p_X(x)\log{p_X(x)}-\Sigma_{x\in X}p_X(x)\log{q_X(x;W)}$
  - 인풋 x에 대해 label p(x)에 NN weight로부터 구해지는 (weight를 parameter로 가지는) q(x;W)를 근사하는 것
  - 최적화 측면에서 W에 속하는 매개변수에 대해서만 미분하기 때문에 초반 p(x)로만 이루어진 텀은 필요 없다.
- Cross Entropy
  - One-hot-encoding
    - $C_{p\Vert{q}}=-\Sigma_x\Sigma_kp_k(x)\log{q_k(x;W)}$
    - k=class
  - Multi-label classification
    - $C_{p\Vert{q}}=-\Sigma_x\Sigma_k[p_k(x)\log{p_k(x;W)+(1-p_k(x))\log{(1-p_k(x;W))}}]$
- Mutual Information
  - Conditional Entropy(조건부 불확실량)
    - Y가 관측되고 난 후의 X의 정보기대치(Entropy)
    - Y와 연관있는 X의 정보는 제외됨
    <p align="center">
      <img src="{{site.gdrive_url_prefix}}1rS10rPF_k7hIWISk19s8NIeqEyxmf7xy" title="lec4-p20 image" style="float: center; width:75%">
    </p>
