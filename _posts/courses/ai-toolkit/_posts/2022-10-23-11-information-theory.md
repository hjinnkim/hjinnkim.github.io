---
title: "[AI toolkit] 11. Information Theory"
excerpt: "Information Theory Basics and Loss Functions"

categories:
  - AI toolkit
tags:
  - [Information Theory, Entropy, KL Divergence, ]

permalink: /courses/ai-toolkit/011/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-23
last_modified_at: 2022-10-23
order: 12
published: true
use_math: true

---
# Lecture 11

## 0. Table of Contents

1. [Self-Information](#1-self-information)
2. [Entropy](#2-entropy)
3. [Joint Entropy](#3-joint-entropy)
4. [Conditional Entropy](#4-conditional-entropy)
5. [Mutual Information](#5-mutual-information)
6. [Kullback-Leibler Divergence](#6-kullback-leibler-divergence)
7. [Cross-Entropy](#7-cross-entropy)
8. [Cross-Entropy and Multi-Class Classification](#8-cross-entropy-and-multi-class-classification)

## 1. Self-Information

- Bit: the unit of information, 0 or 1.
  - Any information can be encoded by a series of 0 and 1.
  - Binary digits of length $n$ contains $n$ bits of information.
- Self information: $I(X)=-\log_{2}(p)$
  - A function which can transfer the probability $p$ to the number of bits.
  - Bits of information we can receive from the event $X$.
  - example)
    - $I("0010")=-\log(p("0010"))=-\log(\frac{1}{2^4})=$4bits
  - This measures the information of a single discrete event.

## 2. Entropy

- Generalized measure for any random variable of either discrete/continuous distribution.
- For any random variable $X$ that follows probability distribution $P$ with a probability density function or a probability mass function $p(x)$,
  - The expected amount of information is measured through entropy,
    $H(X)=-\mathbb{E}_{x\sim P}[\log p(x)]$
  - If $X$ is discrete,
    $H(X)=-\underset{i}{\Large{\Sigma}}p_i\log p_i,\ \text{where }p_i=P(X_i)$
  - If $X$ is discrete,
    $H(X)=-\int_x p(x)\log p(x)dx$
- $H(X)=-\mathbb{E}_{x\sim P}[\log p(x)]$
  1. Logarithm function
      - Entropy can be additive over independent random variables.
      - For instance, $p(x)=f_1(x)f_2(x),\ldots,f_n(x)$ can become $\log p(x)=\log f_1(x)+\log f_2(x)+\ldots+\log f_n(x)$
  2. Negative log
      - More frequent events should contain less information than less common events.
      - We need monotonically decreasing relationship between probability of events and their entropy
  3. Expectation function
      - $(-\log p)$ can be considered as the amount of surprise, after seeing a particular outcome.
      - The entropy equals to the average self-information from observing each output.

## 3. Joint Entropy

- For a pair random variables $(X,Y)$, what information is contained in $X$ and $Y$ together, compared to each seperately?
- Joint Entropy for a pair of random variables $(X,Y)$ is defined as below:
  - <p>$H(X,Y)=-\mathbb{E}_{(x,y)\sim P} [\log p_{X,Y}(x,y)]$</p>
  - For discrete random variables, $H(X,Y)=-\underset{x}{\Large{\Sigma}}\underset{y}{\Large{\Sigma}}p_{X,Y}(x,y)\log p_{X,Y}(x,y)$
  - For continuous random variables, $H(X,Y)=-\int_{x,y}p_{X,Y}(x,y)\log p_{X,Y}(x,y)dxdy$
- If $X=Y$ are two identical random variables,
  - $H(X,Y)=H(X)=H(Y)$
- If $X$ and $Y$ are independent
  - $H(X,Y)=H(X)+H(Y)$
- $H(X),H(Y)\leq H(X,Y)\leq H(X)+H(Y)$

## 4. Conditional Entropy

- Conditional entropy is defined as: $H(Y\vert X)=-\mathbb{E}_{(x,y)\sim P}[\log p(y\vert x)]$
- For discrete random variables, $H(Y\vert X)=-\underset{x}{\Large{\Sigma}}\underset{y}{\Large{\Sigma}}p(x,y)\log p(y\vert x)$
- For continuous random variables, $H(Y\vert X)=-\int_x\int_y p(x,y)\log p(y\vert x)dxdy$
- Since $p(y\vert x)=\Large{\frac{p_{X,Y}(x,y)}{p_X(x)}}$, $H(Y\vert X)=H(X,Y)-H(X)$.
  - The information of $Y$ given $X$ is same as information in $Y$ in which is not presented in X.

## 5. Mutual Information

- How much information is shared by $X$ and $Y$?
  - $I(X,Y)=H(X,Y)-H(Y\vert X)-H(X\vert Y)$
  - From information contained in both $X$ and $Y$,
    - Extract information in $Y$ but not in $X$.
    - Extract information in $X$ but not in $Y$.
    <p>$\Rightarrow I(X,Y)=\mathbb{E}_X\mathbb{E}_Y\begin{Bmatrix}p_{X,Y}(x,y)\log\Large{\frac{p_{X,Y}(x,y)}{p_X(x)p_Y(y)}}\end{Bmatrix}$</p>

  <p align="center">
  <img src="{{site.gdrive_url_prefix}}1U0GG26MhUSbqicqHcVUzOV0WCaTLc0x_" title="lec11-p7 image" style="float: center; width:50%">
  </p>

  - $I(X,Y)=H(X)-H(X\vert Y)=H(Y)-H(Y\vert X)=H(X)+H(Y)-H(X,Y)$
  - $I(X,Y)=I(Y,X)$
  - $I(X,Y)\geq 0$
  - $I(X,Y)=0 \Leftrightarrow X\ \text{and}\ Y\ \text{are independent}$

## 6. Kullback-Leibler Divergence

- To measure if two distributions are close together or not.
- Given a random variable $X$ that follows the probability distribution $P$ with p.d.f or p.m.f $p(x)$
- Estimate $P$ by another probability distribution $Q$ with p.d.f or p.m.f $q(x)$.
- Kullback-Leibler(KL) divergence (or relative entropy) between $P$ and $Q$ is
  - $D_{KL}(P \Vert Q)=\mathbb{E}_{x\sim P}\Large{[\large{\log} \frac{p(x)}{q(x)}]} $
  - $D_{KL}(P \Vert Q)\ne D_{KL}(Q \Vert P)$
  - $D_{KL}(P \Vert Q)\geq 0$

## 7. Cross-Entropy

- For true distribution $P$ with probability distribution $p(x)$, and the estimated distribution $Q$ with probability $q(x)$,
- Measure the divergence between the estimating distribution $Q$ and the true distribution $P$ via cross-entropy.
- $CE(P,Q)=-\mathbb{E}_{x\sim P}[\log(q(x))]$

  $\Rightarrow CE(P,Q)=H(P)+D_{KL}(P\Vert Q)$
- Three followings are equivalent:
  1. Maximizing predictive probability $Q$ for distribution $P$ (i.e., $\mathbb{E}_{x\sim P}[\log(q(x))]$)
  2. Minimizing cross-entropy $CE(P,Q)$
  3. Minimizing the KL-divergence $D_{KL}(P\Vert Q)$.

## 8. Cross-Entropy and Multi-Class Classification

<div>
<ul>
<li>Minimizing Cross-Entropy is equivalent to maximizing the log-likelihood function.</li>
<li>For data example $i$, represent $k$-class label $\mathbf{y}_i=(y_{i1},\ldots,y_{ik})$ by one-hot encoding.</li>
<li>Our prediction would be
  $\hat{\mathbf{y}}_i=p_\theta(\mathbf{y}_i\vert\mathbf{x}_i)$.</li>
  <ul>수업 내용에는 $\overset{k}{\underset{j=1}{\Large{\Pi}}}y_{ij}p_\theta(y_{ij}\vert \mathbf{x}_i)$ 까지 전개되어 있으나, 이 경우 $\hat{\mathbf{y}}_i$가 벡터가 아니게 되어 Cross Entropy가 유도되지 않는다.</ul>
<li>The cross-entropy loss would be,</li>
<ul>$\Rightarrow CE(\mathbf{y},\hat{\mathbf{y}})=\overset{n}{\underset{i=1}{\Large{\Sigma}}}\mathbf{y}^T_i\log \hat{\mathbf{y}}_i=\overset{n}{\underset{i=1}{\Large{\Sigma}}}\overset{k}{\underset{j=1}{\Large{\Sigma}}}y_{ij}\log p_\theta(y_{ij}\vert \mathbf{x}_i)$
<li>수업 내용에서는 $\mathbf{y}^T_i$에서 transpose가 생략되어 있는데, transpose가 있어야 내적으로 정의되어 정확한 Cross Entropy가 나온다.</li>
</ul>
<li>Log-likelihood function would be</li>
<ul>$\Rightarrow l(\theta)=\log L(\theta)=\log\overset{n}{\underset{i=1}{\Large{\Pi}}}\overset{k}{\underset{j=1}{\Large{\Pi}}}y_{ij}\log\pi_j$<br>
where $\mathbf{\pi}=(\pi_1,\ldots,\pi_k)$ is a $k$-class multinoulli distribution probability<br>
In maximum likelihood exstimation, we maximize $l(\theta)$ by having $\pi_j=p_\theta(y_{ij}\vert\mathbf{x}_i)$<br>
$\Rightarrow$ same as minimizing CE loss: $CE(\mathbf{y},\hat{\mathbf{y}})$
</ul>

</ul>
</div>