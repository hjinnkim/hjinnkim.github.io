---
title: "[AI toolkit] 09. Optimization (1)"
excerpt: "Convex Optimization introduction"

categories:
  - AI toolkit
tags:
  - [Convex, Optimization, Lagrange]

permalink: /courses/ai-toolkit/009/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-23
last_modified_at: 2022-10-23
order: 10
published: true
use_math: true

---
# Lecture 09

## 0. Table of Contents

1. [Mathematical Optimization Problems](#1-mathematical-optimization-problems)
2. [Convex Optimization Problem](#2-convex-optimization-problem)
3. [Key Properties of Convex Functions](#3-key-properties-of-convex-functions)
4. [Jensen's Inequality](#4-jensens-inequality)
5. [Basic Terminology](#5-basic-terminology)
6. [Convex Solution Sets](#6-convex-solution-sets)
7. [First-Order Optimality Condition](#7-first-order-optimality-condition)
8. [Special Cases of Convex Problems](#8-special-cases-of-convex-problems)
9. [Lagrangian](#9-lagrangian)

## 1. Mathematical Optimization Problems

<div>
<p>
$\begin{matrix}
\underset{x\in D}{min}\hfill & f(x)\hfill &  \\
\text{subject to}\ & g_i(x)\leq 0,\ & i=1,\ldots,m \\
& h_j(x)=0,\ & j=1,\ldots, r \\
\end{matrix}$</p>
<p>
$\begin{matrix}
x\in\mathbb{R}^n\hfill & : \text{optimization variable}\hfill \\
f:\mathbb{R}^n\to\mathbb{R}\hfill & : \text{objective or cost function}\hfill \\
g_i:\mathbb{R}^n\to\mathbb{R}, & i=1,\ldots,m:\text{ineqn. constraints}\hfill \\
h_i:\mathbb{R}^n\to\mathbb{R}, & i=1,\ldots,r:\text{eqn. constraints}\hfill \\
\end{matrix}$
</p>
<ul>
<li>"Optimal solution" $x^*$ minimizes the objective function in a feasible domain $D$.</li>
<li>$D=dom(f)\cap\overset{m}{\underset{i=1}{\LARGE{\cap}}}dom(g_i)\cap\overset{r}{\underset{j=1}{\LARGE{\cap}}}dom(h_j)$<br>
where $dom(f)$ is an impolicit constraint, and the remain is an explicit constraints</li>
<li>
<p></p>
Example)<br>
$\begin{matrix}
\text{minimize} &\log(x) & \Rightarrow & \text{minimize} & \log(x) \\
& & & & \text{subject to} & x \gt 0 \\
\end{matrix}$
</li>
</ul>
</div>

## 2. Convex Optimization Problem

<div>
<p>
$\begin{matrix}
\underset{x\in D}{min}\hfill & f(x)\hfill &  \\
\text{subject to}\ & g_i(x)\leq 0,\ & i=1,\ldots,m \\
& h_j(x)=0,\ & j=1,\ldots, r \\
\end{matrix}$</p>
<ul>
<li>$f$ and $g$ are convex,</li>
<li>$h_j$ is affine $(Ax+b)$</li>
<ul><a href="https://mathworld.wolfram.com/AffineFunction.html"><span style="color:blue">affine function</span></a></ul>
</ul>
<p align="center">
<img src="{{site.gdrive_url_prefix}}1qNiVzjy7MadwsRVfq5qHMX1JXa2_s_8p" title="lec09-p4 image" style="float: center; width:80%">
</p>
$\Rightarrow$ def. of Convex
<ul><ul>
<li><b>Convex set, $C$</b></li>
<ul>
<li>When line segment connecting $x_1$ and $x_2$ are defined as $x=\theta x_1+(1-\theta)x_2$</li>
<li>A set $C$ is convex if $x_1,\ x_2\in C,\ 0\leq \theta \leq 1 \Rightarrow \theta x_1+(1-\theta)x_2\in C$ </li>
</ul>
<li><b>Convex Function</b></li>
<ul>
<li>$f:\mathbb{R}^n\to\mathbb{R}$ is convex if $dom(f)$ is a convex set and,<br>$f(\theta x+(1-\theta)y)\leq\theta f(x)+(1-\theta)f(y)$ for all $x,\ y\in dom(f),\ 0\leq\theta\leq 1$.</li>
<li>$f$ is a convex function if and only if its epigraph is a convex set.</li>
<li>epigraph of $f:\mathbb{R}^n\to\mathbb{R}$<br>
epi $f=\{(x,t)\in\mathbb{R}^{n+1}\lvert x\in dom(f),\ f(x)\leq t\}$</li>
</ul>
<li><b>Property of convex optimization problems</b></li>
<ul>
<li>If $f$ is a convex and $x$ is a locally optimal point of $f(x)$,<br>
actually $x$ is a globally optimal point.</li>
<ul><li><a href="https://ai.stanford.edu/~gwthomas/notes/convexity.pdf"><span style="color:blue">proof</span></a></li></ul>
</ul>
</ul>
</ul>
</div>

## 3. Key Properties of Convex Functions

<div>
<ul>
<li><b>First-order Cahracterization</b></li>
<ul><li>$f$ is convex iff $dom(f)$ is a convex, and $f(y)\geq f(x)+\nabla f(x)^T(y-x)$ for all $x,y\in dom(f)$.</li>
<p align="center">
<img src="{{site.gdrive_url_prefix}}1xXuziKmznWpuNGOpdceiy81oug_jlB2-" title="lec09-p5 image" style="float: center; width:80%">
</p>
</ul>
<li><b>Second-order Characterization</b></li>
<ul>
<li>$f$ is convex $\Leftrightarrow\ \nabla^2f(x)\succcurlyeq 0$ for all $x\in dom(f)$ and where $dom(f)$ is a convex.</li>
<p>
$\begin{align*}
\succcurlyeq &: \text{when used with matrix, it denotes positive semidefiniteness.} \\
&: \text{when used with vector, it denotes all elements of the vectors are non-negative.}
\end{align*}$
</p>
</ul>
</ul>
</div>

## 4. Jensen's Inequality

- For convex $f$, and for $w_1, \ldots, w_n$ where $w_i\gt 0$ and $\overset{n}{\underset{i=1}{\Large{\Sigma}}}w_i=1,$

    $\Rightarrow\overset{n}{\underset{i=1}{\Large{\Sigma}}}w_if(x_i)\geq f\bigg(\overset{n}{\underset{i=1}{\Large{\Sigma}}}w_ix_i\bigg)\rightarrow\mathbb{E}(f(x))\geq f(\mathbb{E}(x))$
    $\Rightarrow f(tx_1+(1-t)x_2)\leq tf(x_1)+(1-t)f(x_2)$ for $0\leq t\leq 1$

## 5. Basic Terminology

<div>
<p>
$\begin{matrix}
\underset{x\in D}{min}\hfill & f(x)\hfill &  \\
\text{subject to}\ & g_i(x)\leq 0,\ & i=1,\ldots,m \\
& h_j(x)=0,\ & j=1,\ldots, r \\
\end{matrix}$</p>
<p>
$\begin{matrix}
x\in\mathbb{R}^n\hfill & : \text{optimization variable}\hfill \\
f:\mathbb{R}^n\to\mathbb{R}\hfill & : \text{objective or cost function}\hfill \\
g_i:\mathbb{R}^n\to\mathbb{R}, & i=1,\ldots,m:\text{ineqn. constraints}\hfill \\
h_i:\mathbb{R}^n\to\mathbb{R}, & i=1,\ldots,r:\text{eqn. constraints}\hfill \\
\end{matrix}$
</p>
<ul>
<li>If $x\in D$ satisfies explicit constraints, it is called "feaible point"</li>
<li>For feasible point $x$, the minimum of $f(x)$ is called "optimal value", and denoted as $f^*$.</li>
<li>For feasible point $x$, if $f(x)=f^*,\ x$ is called "optimal", "solution", or "minimizer".</li>
<li>For feasible point $x$, if $f(x)\leq f^*+\epsilon,\ x$ is called "$\epsilon$-suboptimal".</li>
<li>For feasible point, $x$, if $g_i(x)=0,\ g_i$ is "active" on $x$.</li>
</ul>
</div>

## 6. Convex Solution Sets

<div>
<ul>
<li>Let us denote $X_{opt}$ is a set of solution for a certain convex problem.</li>
<ul><p>$\begin{matrix}
\hfill X_{opt}= & arg\underset{x}{min}\hfill & f(x)\hfill &  \\
& \text{subject to}\ & g_i(x)\leq 0,\ & i=1,\ldots,m \\
& & h_j(x)=0,\ & j=1,\ldots, r \\
\end{matrix}$</p></ul>
<li>$X_{opt}$ is a convex set.</li>
<ul>
<li>How do we check this for $tx+(1-t)y$ where $x,\ y$ are solutions?</li>
<ul>
<li>Is $tx+(1-t)y$ in $D$?</li>
<li>Does $tx+(1-t)y$ satisfy explicit conditions?</li>
<li>Is $tx+(1-t)y$ also a solution?</li>
</ul>
</ul>
<li>About being "strictly convex"</li>
<ul><li>$f(tx+(1-t)y)\lt tf(x)+(1-t)f(y)$<br>
where $0\lt t\lt 1$, $x\ne y$, and $xy\in dom(f)$.
</li></ul>
</ul>
</div>

## 7. First-Order Optimality Condition

<div>
<ul>
<p>$\begin{matrix}
\underset{x}{min}\hfill & f(x)\hfill \\
\text{subject to}\ & x\in C \\
\end{matrix}$</p>
<li>For this convex problem, if $f$ is differentiable,</li>
<ul>
$x$ is optimal $\Leftrightarrow \nabla f(x)^T(y-x)\geq 0$ for all $y\in C$<br>
Set $C$ is in half-space which is opposite to the direction $-\nabla f(x)$
</ul>
<p align="center">
<img src="{{site.gdrive_url_prefix}}1Y87DW1EOlkPzQdFJj-ymvGbbCmNNyelK" title="lec09-p10 image" style="float: center; width:100%">
</p>
</ul>
</div>

## 8. Special Cases of Convex Problems

<div>
<ul>
<li><b>Linear Programming</b></li>
<ul>: A convex optimization problems is a linear probram (LP) if both the objective function and inequality constraints are affine functions.<br>
<p>$\begin{matrix}
\text{minimize}\hfill & c^Tx+d\hfill \\
\text{subject to}\ & Gx\preccurlyeq h \hfill\\
& Ax=b \hfill\\
\end{matrix}$</p>
<p align="center">
<img src="{{site.gdrive_url_prefix}}1Fd-0rEdbw0e2YcQgmXqW1uRRXpCNr5Yq" title="lec09-p11 image" style="float: center; width:80%">
</p>
</ul>
<li><b>Quadratic Programming</b></li>
<ul>: A convex optimization problem is a quadratic program (QP) if the inequality constraints are still all affine, but the objective function is a convex function.<br>
<p>$\begin{matrix}
\text{minimize}\hfill & \frac{1}{2}x^TPx+c^Tx+d\hfill \\
\text{subject to}\ & Gx\preccurlyeq h\hfill \\
& Ax=b \hfill\\
\end{matrix}$</p>
<p align="center">
<img src="{{site.gdrive_url_prefix}}1GwnCpc2lAcUNMSZ0mWo5frbd0rfQ9omO" title="lec09-p12 image" style="float: center; width:100%">
</p>
To make objective function as convex, $P$ is a symmetric positive semidefinite matrix.
</ul>
<li><b>Quadratically Constrained Quadratic Programming</b></li>
<ul>: A convex optimization problem is a quadratically constrained quadratic program (QCQP) if both the objective and inequality constraints are convex quadratic functions<br>
<p>$\begin{matrix}
\text{minimize}\hfill & \frac{1}{2}x^TP_0x+q^T_0x+r_0\hfill \\
\text{subject to}\ & \frac{1}{2}x^TP_ix+q^T_ix+r_i\leq0, i=1,\ldots,m\hfill \\
& Ax=b \hfill\\
\text{where}\hfill & P_i\in\mathbb{S}^n_+\ \text{for}\ i=0,\ldots,m,\ \text{and}\ A\in\mathbb{R}^{p\times n}\hfill
\end{matrix}$</p>
</ul>
<li><b>Semidefinite Programming</b></li>
<ul>: If inequality constraint in LP changes to linear matrix inequality, it is Semidefinite Program.<br>
<p>$\begin{matrix}
\underset{x}{\text{minimize}}\hfill &c^Tx+d\hfill \\
\text{subject to}\ & xF_1+\cdots+x_nF_n+G\preccurlyeq 0\hfill \\
& Ax=b \hfill\\
\text{where}\hfill & G,F_1,\cdots,F_n\in\mathbb{S}^k\ \text{and}\ A\in\mathbb{R}^{p\times n}\hfill \\
\end{matrix}$</p>
</ul>
</ul>
</div>

## 9. Lagrangian

<div>
<ul>
<p>$\begin{matrix}
\underset{x}{\text{min}}\hfill & f(x)\hfill \\
\text{s.t.}\ & h_i(x)\leq 0,\ i=1,\ldots,m \hfill\\
& l_j(x)=0,\ j=1,\ldots,r \hfill\\
\end{matrix}$</p>
<li>For above optimization problem, which doesn't have to be a convex optimization,</li>
<li>We define Lagrangian as, with new variables $u\in\mathbb{R}^m,v\in\mathbb{R}^r,\ \text{where}\ u\geq0$,</li>
<p align="center">$L(x,u,v)=f(x)+\overset{m}{\underset{i=1}{\Large{\Sigma}}}u_ih_i(x)+\overset{r}{\underset{j=1}{\Large{\Sigma}}}v_jl_j(x)$</p>
<ul>
<li>Implicitly, we define $L(x,u,v)=-\infty\ \text{for}\ u\gt 0$</li>
<li>For feasible $x$,<br>
<p align="center">
$L(x,u,v)=f(x)+\overset{m}{\underset{i=1}{\Large{\Sigma}}}u_ih_i(x)+\overset{r}{\underset{j=1}{\Large{\Sigma}}}v_jl_j(x)\leq f(x)$
</p></li>
</ul>
</ul>
</div>

### 9.1 Lagrange Dual Function

<div>
<ul>
<li>Let $C$ denote a primal feabile set, and $f^*$ be a prinmal optimal value</li>
<li>Then, if we minimze $L(x,u,v)$ for all $x$, it has below lower bound<br>
<p align="center">$f^*\geq \underset{x\in C}{\text{min}}L(x,u,v)\geq\underset{x}{\text{min}}L(x,u,v):=g(u,v)$</p></li>
<li>Here, we call $g(u,v)$ as a Lagrange dual function, which provides a lower bound for any feasible $u\geq 0, v$.</li>
<li>Lagrangian $L(x,u,v)$ is affine in $u\geq0,v$.</li>
<ul><li>Dual function $g(u,v)$ is always concave, becuase it is the pointwise infimum of a set of affine functions.<br>
<p align="center">$g(u,v)=\underset{x}{\text{min}}\{f(x)+\overset{m}{\underset{i=1}{\Large{\Sigma}}}u_ih_i(x)+\overset{r}{\underset{j=1}{\Large{\Sigma}}}v_jl_j(x)\}$</p></li>
</ul>
</ul>
</div>

### 9.2 Lagrange Dual Problem

<div>
<ul>
<p>$\begin{matrix}
\underset{x}{\text{min}}\hfill & f(x)\hfill \\
\text{s.t.}\ & h_i(x)\leq 0,\ i=1,\ldots,m \hfill\\
& l_j(x)=0,\ j=1,\ldots,r \hfill\\
\end{matrix}$</p>
<li>The dual function $g(u,v)$ satisfies $f^*\geq g(u,v)$</li>
<ul>
<li>Maximized $g(u,v)$ is the best lower bound!</li>
<li>Dual Problem: <p>$\begin{matrix}
\underset{u,v}{\text{max}}\hfill & g(u,v)\hfill \\
\text{s.t.}\ & u\geq 0,\hfill\\
\end{matrix}$</p></li>
</ul>
<li>Since $g(u,v)$ is concave, dual problem is a concave maximization problem.</li>
</ul>
</div>
