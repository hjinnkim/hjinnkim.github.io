---
title: "[AI toolkit] 07. Probability Distribution (1)"
excerpt: "Probability Basics"

categories:
  - AI toolkit
tags:
  - [Probability]

permalink: /courses/ai-toolkit/007/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-22
last_modified_at: 2022-10-22
order: 8
published: true
use_math: true

---
# Lecture 07

## 0. Table of Contents
1. [Coin Toss Example](#1-coin-toss-example)
2. [Sample space, event, and probability](#2-sample-space-event-and-probability)
3. [Random Variables](#3-random-variables)
4. [Discrete Probabilities](#4-discrete-probabilities)
5. [Continuous Probabilities](#5-continuous-probabilities)
6. [Rules of Probability](#6-rules-of-probability)
7. [Means](#7-means)
8. [Covariance](#8-covariance)
9. [Correlation](#9-correlation)
10. [Empirical Mean and Covariance](#10-empirical-mean-and-covariance)
11. [Sums and Transformations of Random Variables](#11-sums-and-transformations-of-random-variables)
12. [Independence](#12-independence)

## 1. Coin Toss Example

- How can we "quantify" the likelihood of observing heads
  - Assuming the "fair" coin, both outcomes are equally likely.
- If we get $n_t$ tails and $n_h$ heads, let $n=n_h+n_t$.
  - We "expect" to see $n_t/n=1/2$ and $n_h/n=1/2$.
  - But can we say observed frequencies as "probabilities"? isn't it "statistics"?
- Probability : theoretical quantities that underlies the data generation process.
- Statistics : empirical quantities computed as functions of the observed data.

## 2. Sample space, event, and probability

- Sample space $\Omega$ : a set of all possible outcomes of the experiment.
  - i.i., if we toss coins for two times (H : Head, T : Tail)
    - $\Omega = \{HH, HT, TH, TT\}$
- Event $A$: subsets of the sample space.
  - i.e., the event of "the first coin toss comes up heads", $A=\{HH, HT\}$
- Probability function : maps events onto real values, $P : A \subseteq \Omega\to [0, 1]$
  - The probability of any event is a non-negative real number, $P(A)\geq0$
  - The probability of the entire sample space is 1, $P(\Omega)=1$.
  - For any countable even sequence $A_1, A_2,\ldots$, if $A_j\cap A_i=\emptyset$ for $i\ne j, P(\cup_{i=1}^\infty A_i)=\Sigma_{i=1}^\infty P(A_i)$

## 3. Random Variables

- Mapping from an underlying sample space to a set of values.
- For coin-tossing example, assume that you consider "number of heads"
  - A random variable $X$ maps $X(HH)=2, X(HT)=1, X(TH)=1, X(TT)=0$
  - Here, if $X : \Omega \to T, T=\{0, 1, 2\}$
  - For any subset $S\subseteq T, P_X(S)\in[0,1]$
  - $P_X(S)=P(X\in S)=P(X^{-1}(S))=P(\{w\in\Omega : X(\omega)\in S\})$
  - Remember $\Omega=\{TT, HH, TH, HT\}$ in this example
    - If $T$ is finite or countably infinite (i.e., integer), $X$ is discrete random variable
    - If $T\in\mathbb{R}^n$, $X$ is coutinuous random variable

## 4. Discrete Probabilities

<div>
<img src="{{site.gdrive_url_prefix}}1A-2CVxtNW1Hkaux6RTXO75RZebXuiq9h" title="lec07-p5 image" style="float: center; width:70%">
<ul>
<li>When target space $T$ is discrete</li>
<li>We define "Probability mass function" (pmf) of a discrete probability distribution</li>
<li>For two random variables $X,\ Y$, $p(X=x, Y=y)=\frac{n_{ij}}{N}$<br>
where $n_{ij}$ denotes the number of events for states $x_i$ and $y_j$<br>
where $N$ denotes the total number of events</li>
<li>This <b>joint probability</b> is often written as $p(x, y)$ for $X=x, Y=y$</li>
<li>The <b>marginal probability</b>: $p(x)=\Sigma_yp(x, y)$</li>
<li>The conditional probability of y given x : $p(y|x)$</li>
</ul>
</div>

## 5. Continuous Probabilities

- A function $f\ :\ \mathbb{R}^D\to\mathbb{R}$ is called a probability density function (pdf) i
  1. $\forall\mathbf{x}\in\mathbb{R}^D\ :\ f(\mathbf{x})\geq0$
  2. $\int_{\mathbb{R}^D}f(\mathbf{x})d\mathbf{x}=1$
- $P(a\leq \mathbf{X}\leq b)=\int_a^bf(x)dx, P(X=x)=0$
- A cumulative distribution function (cdf) of a multivariate real-valued random variable $X$ with states $\mathbf{x}\in\mathbb{R}^D$ is:
  - $F_X(\mathbf{x})=P(X_1\leq x_1, \ldots, X_D\leq x_D)$
  - $F_X(\mathbf{x})=\int_{-\infty}^{x_1}\cdots\int_{-\infty}^{x_D}f(z_1, \ldots, z_D)dz_1\cdots dz_D $

## 6. Rules of Probability

- Sum Rule : $p(x)=\begin{cases}
\underset{y\in\mathcal{Y}}{\LARGE{\Sigma}}p(x, y) & \text{if}\ x,\ y\ \text{is discret} \\\\\\
\int_{\mathcal{Y}}p(x, y)dy &  \text{if}\ x,\ y\ \text{is continuous} \\\\\\
\end{cases}$

- Product Rule : $p(x, y) = p(y\vert x)p(x)$
- Baye's Rule : $p(x\vert y)=\frac{p(y\vert x)p(x)}{p(y)}$
  - which is called $posterior\ = \ \frac{likelihood\times prior}{evidence} $
  - making an inference of unobserved (latent) random variable $\mathbf{x}$, from observed values $\mathbf{y}$.

## 7. Means

<p>
<ul>
<li>The expected value of a function $g\ :\ \mathbb{R}\to\mathbb{R}$ of a univariate continuous random variable $X\sim p(x)$ is given by:</li>
<ul>
<li>$\mathbb{E}_{X}[g(x)]=\int_{\mathcal{X}}g(x) p(x) dx $ , and for discrete random variable, $\mathbb{E}_X[g(x)]=\underset{x\in\mathcal{X}}{\large{\Sigma}}g(x)p(x)$.</li>
<li>This is a linear operator</li>
</ul>
<li>The mean of a random variable $X$ with states $\mathbf{x}\in\mathbb{R}^D$ is defined as:</li>
<ul>
<li>$\mathbb{E}_X[x]=\begin{bmatrix}
\mathbb{X_1}[x_1]\\
\vdots \\
\mathbb{x_D}[x_D]\\
\end{bmatrix}\in\mathbb{R}^D$, where $\mathbb{E}_{X_d}[x_d]:=\begin{cases}
\int_{\mathcal{X}}x_dp(x_d)dx_d & \text{if}\ X\ \text{is a continuous random variable} \\
\underset{x_i\in\mathcal{X}}{\large{\Sigma}}x_ip(x_d=x_i) & \text{if}\ X\ \text{is a discrete random variable} \\
\end{cases}$</li></ul>
<li>Modes : most frequent value></li>
<li>Median : the middle value.</li>
</ul>
</p>

## 8. Covariance

- For univariate random variables, $X,\ Y\in\mathbb{R}$, the covariance between two is given as the expected product of their deviations from their respective means:
  - $Cov_{X,Y}[x, y]:=\mathbb{E}_{X,Y}[(x-\mathbb{E}_X[x])(y-\mathbb{E}_Y[y])]$
  - The covariance of itself is called as variance, $V[X]=Cov_{X,X}$, and its root is standard deviation, $\sigma[X]=\sqrt{Var[X]}$
  - By using the linearity of expectations, $Cov_{X,Y}=\mathbb{E}[XY]-\mathbb{E}[X]\mathbb{E}[Y]$.

<ul>
<li>For multivariate random variables, $X,\ Y$ where $\mathbf{x}\in\mathbb{R}^D,\ \mathbf{y}\in\mathbb{R}^E$, covariance is a matrix as:</li>
<ul>
<li>$Cov[\mathbf{x}, \mathbf{y}]=\mathbb{E}[\mathbf{x}\mathbf{y}^T]-\mathbb{E}[\mathbf{x}]\mathbb{E}[\mathbf{y}]^T=Cov[\mathbf{y}, \mathbf{x}]^T\in\mathbb{R}^{D\times E}$</li>
<li>The covariance of itself is also called a variance as below</li>
    $\begin{align*}
    \mathbb{V}_X[\mathbf{x}] & =Cov_X[\mathbf{x},\mathbf{x}] \\
    & = \mathbb{E}_X[(\mathbf{x}-\mathbf{\mu})(\mathbf{x}-\mathbf{\mu})^Y]=\mathbb{E}_X[\mathbf{x}\mathbf{x}^T]-\mathbb{E}_X[x]\mathbb{E}_X[x]^T \\
    & = \begin{bmatrix}
    Cov[x_1, x_1] & Cov[x_1, x_w] & \cdots & Cov[x_1, x_D] \\
    Cov[x_2, x_1] & Cov[x_2, x_2] & \cdots & Cov[x_2, x_D] \\
    \vdots & \vdots & \ddots & \vdots \\
    Cov[x_D, x_1] & \cdots & \cdots & Cov[x_D, x_D] \\
    \end{bmatrix}
    \end{align*}
    $
    where A symmetric and positive-semidefinite Covariance matrix.
</ul>
</ul>

## 9. Correlation

<div>
<ul>
<li>The correlation between two random variables $X,\ Y\in\mathbb{R}$ is given by</li>
<ul>
<li>$corr[x,y]=\frac{Cov[x, y]}{\sqrt{\mathbb{V}[x]\mathbb{V}[y]}}\in[-1,1]$</li>
<li>Indicates how two random variables are related</li>
</ul>
</ul>
<p align="center">
<img src="{{site.gdrive_url_prefix}}1is0IzX2yeObF2T0ZdsIK5_Tun2OBR12b" title="lec07-p11 image" style="float: center; width:100%"></p>
</div>

## 10. Empirical Mean and Covariance

- Given a finite dataset of size N, we can obtain an estimate of the mean.
- The empirical mean / sample mean vector is the arithmetic average observations for each variable, defined as $\overline{x}:=\frac{1}{N}\overset{N}{\underset{n=1}{\large{\Sigma}}}x_n$. where $x_1,\ldots,x_N$ are observations.
- The empirical covariance matrix is $\Sigma:=\frac{1}{N}\overset{N}{\underset{n=1}{\large{\Sigma}}}(x_n-\overline{x})(x_n-\overline{x})^T$.

## 11. Sums and Transformations of Random Variables

- For two random variables $X,\ Y$ with states $x, y\in\mathbb{R}^D$
  - $\mathbb{E}[x+y]=\mathbb{E}[x]+\mathbb{E}[y]$
  - $\mathbb{E}[x-y]=\mathbb{E}[x]-\mathbb{E}[y]$
  - $\mathbb{V}[x+y]=\mathbb{V}[x]+\mathbb{V}[y]+Cov[x,y]+Cov[y,x]$
  - $\mathbb{V}[x+y]=\mathbb{V}[x]+\mathbb{V}[y]-Cov[x,y]-Cov[y,x]$
- $y=Ax+b$
  <div>
  <ul>
  <li>$\mathbb{E}_Y[y]=\mathbb{E}_X[Ax+b]=A\mathbb{E}_X[x]+b=A\mu+b$</li>
  <li>$\mathbb{V}_Y[y]=\mathbb{V}_X[Ax+b]=\mathbb{V}_X[Ax]=A\mathbb{V}_X[x]A^T$</li>
  <ul>
  <p>
  $\begin{align*}
  Cov[x,y]&=\mathbb{E}[x(Ax+b)^T]-\mathbb{E}[x]\mathbb{E}[Ax+b]^T\\
  &=\mathbb{E}[x]b^T+\mathbb{E}[xx^T]A^T-\mu b^T-\mu\mu^TA^T\\
  &=\mu b^T-\mu b^T+(\mathbb{E}[xx^T]-\mu\mu^T)A^T\\
  &=\Sigma A^T \\
  \end{align*}
  $
  </p>
  </ul>
  </ul>
  </div>
 
## 12. Independence

- Two random variables, $X,\ Y$ are independent if and only if $p(x,y)=p(x)p(y)$ and below properties are hold:
  - $p(y\vert x)=p(y)$
  - $p(x\vert y)=p(x)$
  - $\mathbb{V}_{X,Y}[x+y]=\mathbb{V}_X[x]+\mathbb{V}_Y[y]$
  - $Cov_{X,Y}[x,y]=0$
- Two random variables $X,\ Y$ are conditionally independent given $Z$ if and onyl if:
  $p(x,y\vert z)=p(x\vert y,z)p(y\vert z)=p(x|z)p(y\vert z)\ \text{for all}\ z\in\mathcal{Z}$
- $p(x\vert y,z)=p(x\vert z)$ implies "Markov Process"
  - The current state only depends on the previous state