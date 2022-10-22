---
title: "[AI toolkit] 08. Probability Distribution (2)"
excerpt: "Random Variables, Central Limit Theorem"

categories:
  - AI toolkit
tags:
  - [Probability, Central Limit Theorem, Random Variables]

permalink: /courses/ai-toolkit/008/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-23
last_modified_at: 2022-10-23
order: 9
published: true
use_math: true

---
# Lecture 03

## 0. Table of Contents

1. [Bernoulli Distribution](#1-bernoulli-distribution)
2. [Uniform Distribution](#2-uniform-distribution)
3. [Binomial Distribution](#3-binomial-distribution)
4. [Poisson Distribution](#4-poisson-distribution)
5. [Gaussian Distribution](#5-gaussian-distribution)
6. [Properties of Gaussian Distribution](#6-properties-of-gaussian-distribution)
7. [Central Limit Theorem](#7-central-limit-theorem-clt)
8. [Exponential Family](#8-exponential-family)
9. [Random Process](#9-random-process)
10. [Stationary Process](#10-stationary-process)

## 1. Bernoulli Distribution

- For a single binary random variable $X$ with state $x=\{0,\ 1\}$(i.e., coin flip) assume that we observe $X=1$ with probability $\mu$
- We write this as $X\sim Bernoulli(\mu)$, and 
  - $p(x\vert\mu)=\mu^x(1-\mu)^{1-x}, x\in\{0,\ 1\}$
  - $\mathbb{E}[x]=\Sigma xp(x)=0(1-\mu)+1(\mu)=\mu$
  - $\mathbb{V}[x]=\mathbb{E}[x^2]-\mathbb{E}[x]^2=\mu-\mu^2=\mu(1-\mu)$
    - $\mathbb{E}[x^2]=\Sigma x^2p(x)=0(1-\mu)+1(\mu)=\mu$

## 2. Uniform Distribution

- Every possible values is equally likely.
- For discrete random variable $X$ with states $x\in\{a, a+1, \ldots, b-1, b\}$, its p.m.f is:
  - where n=b-a+1
  - $\mathbb{E}[x]=\frac{a+b}{2}, \mathbb{V}=\frac{n^2-1}{12}$
    - $\mathbb{E}[x]=\overset{b}{\underset{x=a}{\Sigma}} xp(x)=\frac{1}{n}\overset{b}{\underset{x=a}{\Sigma}}x=\frac{1}{n}\frac{a+b}{2}n =\frac{a+b}{2}$
    - For uniform distribution, variance is not affected by starting point.
      $\mathbb{E}[x^2]=\overset{b}{\underset{x=a}{\Sigma}}x^2p(x)=\frac{1}{n}\overset{b}{\underset{x=a}{\Sigma}}x^2=\frac{1}{n}\overset{n}{\underset{x=1}{\Sigma}}x^2=\frac{1}{n}\frac{n(n+1)(2n+1)}{6}=\frac{(n+1)(2n+1)}{6}$
      $\mathbb{V}[x]=\mathbb{E}[x^2]-\mathbb{E}[x]^2=\frac{(n+1)(2n+1)}{6}-\frac{n^2+2n+1}{4}=$
      $\frac{2(2n^2+3n+1)-3(n^2+2n+1)}{12}=\frac{n^2-1}{12}$
- For continuous random variable $X$ with states $x\in[a,\ b]$, its pdf is:
  - $p(x)=\begin{cases}
  \frac{1}{b-a} & x\in[a,\ b] \\
  0 & x\notin[a, b] \\
  \end{cases}$
  - $\mathbb{E}[x]=\frac{a+b}{2}, \mathbb{V}[x]=\frac{(b-a)^2}{12}$
    - $\mathbb{E}[x]=\int_a^bp(x)xdx=\int_a^b\frac{1}{b-a}xdx=\frac{1}{2(b-a)}x^2\bigg\rvert_a^b=\frac{b^2-a^2}{2(b-a)}=\frac{a+b}{2}$
    - $\mathbb{E}[x^2]=\int_a^bp(x)x^2dx=\int_a^b\frac{1}{b-a}x^2dx=\frac{1}{3(b-a)}x^3\bigg\rvert_a^b=\frac{b^3-a^3}{3(b-a)}=\frac{b^2+ab+a^2}{3}$
      $\mathbb{V}[x]=\mathbb{E}[x^2]-\mathbb{E}[x]^2=\frac{b^2+ab+a^2}{3}-(\frac{a+b}{2})^2=\frac{b^2+ab+a^2}{3}-(\frac{a^2+2ab+b^2}{4})=$
      $\frac{4(b^2+ab+a^2)}{12}-\frac{3(a^2+2ab+b^2)}{12}=\frac{b^2-2ab+a^2}{12}=\frac{(b-a)^2}{12}$

## 3. Binomial Distribution

- Performing a sequence of $n$ independent experiments, where the probability of each success is $\mu$
- Each experiment is an independent random variable $X_i$, which encode success with 1, failure with 0, such that $X_i\sim Bernoulli(\mu)$.
- If $X=\overset{n}{\underset{i=1}{\large{\Sigma}}}X_i$, we write $X\sim Beinomial(n,\mu)$ and $p(k)=\begin{pmatrix}
n \\\\\\ k
\end{pmatrix}\mu^k(1-\mu)^{n-k}$, where $\begin{pmatrix}
n \\\\\\ k
\end{pmatrix}=\Large{\frac{n!}{k!(n-k)!}}$
- $\mathbb{E}[x]=np, \mathbb{V}[x]=np(1-p)$
  - $\mathbb{E}[x]$ [<span style="color:blue">proof</span>](https://proofwiki.org/wiki/Expectation_of_Binomial_Distribution/Proof_1), $\mathbb{V}[x]$ [<span style="color:blue">proof</span>](https://proofwiki.org/wiki/Variance_of_Binomial_Distribution)
<p align="center">
<img src="{{site.gdrive_url_prefix}}1-YeTN5sj6kW0OI2XZi2oz4IyawsEPCcl" title="lec08-p5 image" style="float: center; width:80%">
</p>

## 4. Poisson Distribution

- The probability of a given number of events that can occur in a fixed interval of time/space.
- Let us assume that "evevnt" is "one bus arrives bus stop", in a fixed "1 minute" time window.
  - Denote this with $X^{(1)}\sim Bernoulli(p)$
  - If you want to model "max. two bus arrive bus stop", in a fixed "1 minute", you can
  <p align="center">
  <img src="{{site.gdrive_url_prefix}}1H5dRKtJY_ftm2QoyP9r9ad5NGUFjAO93" title="lec08-p5 image" style="float: center; width:80%">
  </p>

  - Consider $X^{(n)}\sim Binomial(n, p/n)$, and try to $n\to\infty$. Then, $\mathbb{E}[X^{(\infty)}]=p$ and $\mathbb{V}[X^{(\infty)}]=p$.
  - If it is a random variable which takes the non-negative integer values with probability $p(X=k)=\Large{\frac{\lambda^ke^{-\lambda}}{k!}}$
  - $\lambda$ is a rate/shape denoting the average number of events we expect in one unit of time.
  - [<span style="color:blue">Poisson distribution explanation</span>](https://angeloyeo.github.io/2021/04/26/Poisson_distribution.html)

## 5. Gaussian Distribution

- For univariate random variable, the Gaussian distribution has a density as:
  - $p(x\vert \mu, \sigma^2)=\frac{1}{\sqrt{2\pi\sigma^2}}\text{exp}(-\frac{(x-\mu)^2}{2\sigma^2})$, where $\mu$ is a mean and $\sigma^2$ is a variance.
- For D-dimensional random variable, the Gaussian distribution has a density as:
  - $p(\mathbf{x}\vert\mathbf{\mu},\mathbf{\Sigma})=(2\pi)^{-\frac{D}{2}}\vert\Sigma\vert^{-\frac{1}{2}}\text{exp}(-\frac{1}{2}(\mathbf{x}-\mathbf{\mu})^T\Sigma^{-1}(\mathbf{x}-\mathbf{\mu}))$
  - We write $p(\mathbf{x})=\mathcal{N}(\mathbf{x}\vert\mathbf{\mu},\mathbf{\Sigma})$ or $X\sim \mathcal{N}(\mathbf{\mu},\mathbf{\Sigma})$
  <p align="center">
  <img src="{{site.gdrive_url_prefix}}1ZtO33CKL-8qz5SzcHd6uC2AjK6NchBaz" title="lec08-p7 image" style="float: center; width:100%">
  </p>

## 6. Properties of Gaussian Distribution

- For two multivariate random variables $X$ and $Y$, if joint distribution is defined as:
  - <p>$p(X, Y)=\mathcal{N}(\begin{bmatrix}\mathbf{\mu}_x\\\mathbf{\mu}_y\\ \end{bmatrix}\begin{bmatrix}\Sigma_{xx} & \Sigma_{xy} \\ \Sigma_{yx} & \Sigma_{yy} \end{bmatrix})$, where $\begin{matrix} \Sigma_{xx}=Cov[X,X]\\\Sigma_{yy}=Cov[Y,Y]\\\Sigma_{xy}=Cov[X,Y]\\ \end{matrix}$</p>
- Then, conditional distribution is also Gaussian, such that:
  - <p>$p(x\vert y)=\mathcal{N}(\mathbf{\mu}_{x\vert y}, \mathbf{\Sigma}_{x\vert y})$</p>
  - <p>$\mathbf{\mu}_{x\vert y}=\mathbf{\mu}_{x}+\mathbf{\Sigma}_{xy}\mathbf{\Sigma}_{yy}^{-1}(\mathbf{y}-\mathbf{\mu}_{y})$</p>
  - <p>$\mathbf{\Sigma}_{x\vert y}=\mathbf{\Sigma}_{xx}-\mathbf{\Sigma}_{xy}\mathbf{\Sigma}_{yy}^{-1}\mathbf{\Sigma}_{yx}$</p>
- The marginal distribution is also Gaussian,
  - <p>where $p(x)=\int p(x,y)dy=\mathcal{N}(x\vert\mu_x, \Sigma_{xx})$, same for $Y$.</p>

  <p align="center">
  <img src="{{site.gdrive_url_prefix}}1CN9Uf2_z0vgSxZ0GgKjGDIlYMkzwkbxR" title="lec08-p9 image" style="float: left; width:100%">
  </p>

## 7. Central Limit Theorem (CLT)

- Consider the similar problem as Poisson distribution, but increasing $n$ with fixed $p$.
  - Then, $X_i\sim Bernoulli(p)$ and $X^{(n)}\sim Binomial(n, p)$.
  - Here, consider its "normalized" version (zero mean, one variance)
  - $Y^{(n)}=\Large{\frac{X^{(n)}-\mathbb{E}[x]}{\sqrt{\mathbb{Y}[x]}}}$. Note that it was $\mathbb{E}[x]=np, \mathbb{V}[x]=np(1-p)$
  - Then, as $n\to\infty$, p.d.f of $Y$ becomes Gaussian distribution.

  <p align="center">
  <img src="{{site.gdrive_url_prefix}}1uSxyvynSa6nY85RO-ut8i22M3rK-40l5" title="lec08-p10 image" style="float: left; width:100%">
  </p>

- Let be $X_1, X_2,\ldots$ i.i.d (independent, identically distributed) random variables with finite mean $\mu$ and finite variance $\sigma^2$.
- For $Y_n:=\frac{1}{\sqrt{n}}\overset{n}{\underset{i=1}{\Large{\Sigma}}}\frac{X_i-\mu}{\sigma}$, then its cumulative distribution probability becomes the standard normal c.d.f, which is $\Phi(y)=\int_{-\infty}^ye^{-t^2/2}/\sqrt{2\pi}dt$.
- Proof is a bit compicated. Try the case when $X_i\sim Bernoulli(1/2)$
  - [<span style="color:blue">Source</span>](https://people.bath.ac.uk/pam28/Paul_Milewski,_Professor_of_Mathematics,_University_of_Bath/Past_Teaching_files/stirling.pdf)

## 8. Exponential Family

- Bernoulli, Uniform, Binomial, Poisson, they are all "**exponential family**"
- It is a set of distributions whose density can be expressed as below form:
  - $p(\mathbf{x}\vert \mathbf{\eta})=h(\mathbf{x})\text{exp}(\mathbf{\eta}^TT(\mathbf{x}-A(\mathbf{\eta})))$
  - where $h(\mathbf{x})$ is the underlying measure or the base measure.
  - $\mathbf{\eta}=(\eta_1,\ldots,\ \eta_l)\in\mathbb{R}^l$ is the vector called natural/canonical parameters.
  - $T(\mathbf{x})=(T_1(\mathbf{x},\ldots,\ T_l(\mathbf{x}))$ is called sufficient statistics for $\mathbf{\eta}$, where $\mathbf{x}=(x_1,\ldots,\ x_n)\in\mathbb{R}^n$
  - $A(\mathbf{\eta}=log[\int h(\mathbf{x})\text{exp}(\mathbf{\eta}^TT(\mathbf{x}))d\mathbf{x})]$ as the cumulant function, which ensures the distributino integrates to one.

## 9. Random Process

- A collection of random variables, usually indexed by time.
- Example : stock price of a company over the next few months:
  - At any fixed time $t_1\in[0,\infty), S(t_1)$ is a random variable.
  - Compared to another $S(t_2)$, PDF of two can be different.
- This can be also thought of as a random function of time.
  <p align="center">
  <img src="{{site.gdrive_url_prefix}}17ANx4ZGoGYW_ZQ3YKy5b5D3QHffQ-kZt" title="lec08-p18 image" style="float: center; width:50%">
  </p>

## 10. Stationary Process

- A random process is called stationary if a time shift does not change its statistical propertiees. (ex) white noise)
- There are some properties like weak-sense stationarity.
