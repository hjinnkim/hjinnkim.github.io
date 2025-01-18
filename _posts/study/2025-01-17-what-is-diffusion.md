---
title: "[Intro to Diffusion Models] (1) What is Diffusion?"
categories:
  - Study
tags:
  - Theory
  - Diffusion Models
  
excerpt: "[Intro to Diffusion Models] (1) What is Diffusion?"

last_modified_at: 2025-01-17T22:00:00
---

# What is Diffusion Model?

Diffusion Model을 처음 공부하고나서 연구실에서 Diffusion Model을 설명하기 위해 정리했던 내용을 포스팅하고자 합니다. 많이 부족하고 세부적으로 틀릴 수 있는 내용이 있기 때문에, 언제나 지적은 환영합니다.

## Diffusion Process?

Diffusion Model은 확산 과정 (Diffusion process)를 상정하고 generative modelling을 수행한다. 확산 과정이란, 어떤 기체 분자가 공기 속에서 퍼져가는 과정, 물 위에 모여있는 꽃가루들이 퍼져가는 과정을 생각하면 편하다. 

<div style="display: flex; justify-content: center; align-items: center; gap: 10px; width: 100%; flex-wrap: wrap;">
  <img src="https://upload.wikimedia.org/wikipedia/commons/9/97/2d_random_walk_ag_adatom_ag111.gif" alt="Brownian Motion GIF 1" style="width: 35%; height: auto;">
  <img src="https://upload.wikimedia.org/wikipedia/commons/c/c2/Brownian_motion_large.gif" alt="Brownian Motion GIF 2" style="width: 25%; height: auto;">
</div>
<p style="text-align: center; margin-top: 10px; font-size: 14px;">
  Source: <a href="https://en.wikipedia.org/wiki/Brownian_motion" target="_blank">Wikipedia - Brownian Motion</a>.
</p>

각 기체 분자, 꽃가루들은 우리가 말하는 *샘플 (sample)*에 해당하고, 그것들이 특정 위치에 존재할 확률을 묘사하는 것을 우리가 흔히 말하는 *확률 분포 (probability distribution)*에 해당한다. 

## Stochastic Differention Equation (SDE)

이 확산 과정은 주로 Stochastic Differention Equation (SDE)에 의해 묘사되는데, 다음과 같은 형태를 띈다.

$$
dx=\underbrace{f(x,t)dt}_{\text{drift term}}+\underbrace{L(x,t)w(t)dt}_{\text{diffusion term}},\\
w(t):\ \text{zero mean, whith Gaussian process}
$$

<p style="text-align: center; margin-top: 10px; font-size: 14px;">
  Source: <a href="https://users.aalto.fi/~asolin/sde-book/sde-book.pdf" target="_blank">Applied Stochastic Differential Equations, (Eq 4.1)</a>
</p>

Gaussian process는 제자리 주변에서의 랜덤한 움직임이라고 생각하면 편한다. 위 식은 다음과 같이 $x$를 움직인다.

$$
x(t)-x(t_0)=\int^t_{t_0}f(x(t),t)dt+\int^t_{t_0}L(x(t),t)w(t)dt\\
\text{usually, }t_0=0,\ 0\leq t\leq 1
$$

<p style="text-align: center; margin-top: 10px; font-size: 14px;">
  Source: <a href="https://users.aalto.fi/~asolin/sde-book/sde-book.pdf" target="_blank">Applied Stochastic Differential Equations, (Eq 4.2)</a>
</p>

SDE에 대한 이야기는 후에 더 포스팅하는 것이 목표이고, 단순히 위 식만 놓고 보면, $x(t)$는 시간이 진행됨에 따라 전체적인 움직임이 $f(x(t),t)$에 의해서 결정되며, 그 움직임의 궤적 주변에서 $L(x(t),t)$에 의해 조금씩 랜덤하게 위치가 바뀐다고 생각하면 편하다.

따라서 $f(x(t),t)$와 $L(x(t),t)$에 따라 각 시간 $t$에서의 data distribution $p(x(t))$의 형태가 결정된다.

## Complex to Simple Distribution

확산에 대해 조금 더 상상해보자. 처음 시점에 꽃가루들을 아무리 복잡한 형태로 물 위에 띄워놔도, 시간이 지나면 물 위에 고르게 퍼져나가는 것을 알 수 있다. 

이 움직임 역시 위 SDE 형태로 묘사되고, 그에 맞는 $f(x(t),t)$와 $L(x(t),t)$가 존재할 것이다. 따라서 $f(x(t),t)$와 $L(x(t),t)$를 적절히 선택하면, $p(x(0))$가 아무리 복잡하더라도 $p(x(1))$를 고르게 퍼져있는, 간단한 상태로 만들 수 있다. 

이 성질은 생성 모델에서 중요하게 작동한다. 확률 분포의 세계에서 그런 간단한 상태 중 대표적인 것이 바로 $\mathcal N(0, I)$에 해당한다. 간단하고, 샘플링도 편하기 때문에 대부분의 생성 모델은 $z\sim \mathcal N(0,I)$로부터 복잡한 샘플 $x\sim p_{data}(x)$를 생성하려고 한다 $(z\rightarrow x, x=f_{\theta}(z))$. Diffusion Model의 경우, 위 SDE 식을 통해 

$$ 
p(x(0)):=p_{data}(x)\rightarrow p(x(1))\approx \mathcal N(0,I)
$$

로 바꾸고, 이 SDE를 되돌리는 방법을 학습하여 

$$
x(1):=z\sim \mathcal N(0,I) \rightarrow x(0)\approx x\sim p_{data}(x)
$$

를 샘플링한다.

## Markov Process

이상적으로 확산과정은 연속 시간 (continuous time)에서 이루어지므로, $t$가 $0\rightarrow 1$로 가는 과정을 $0\leq t\leq 1$를 굉장히 많은 수의 time step, $t_{step}=\{1,\dots,T\}\ (\text{usually, $T=1000$})$으로 쪼개어 나타낸다.

$$
x_0:=x(0)\rightarrow x_1\rightarrow \cdots \rightarrow x_{T-1}\rightarrow x_T:=x(1),\ x(1)\approx z\sim \mathcal N(0,I)
$$

따라서, 각 state $x_t$는 이전 state $x_{0:t-1}$에 의존한다. 

$$
p(x_1\vert x_0) \\
p(x_2\vert x_1,x_0) \\
\vdots \\
p(x_T\vert x_{0:T-1})
$$

확산 과정의 중요한 성질 중 하나는 Markov Process란 것이다. 이는 즉, $ 0<s<t<u<1 $에 해당하는 time step $s, t, u$에 대해 우리가 $x(s),x(t)$를 알고 있더라도, $x(u)$의 상태는 오로지 이전 상태인 $x(t)$에만 의존한다는 것이다. 즉, 비록 샘플이 $x(s)\rightarrow x(t)\rightarrow x(u)$의 과정을 거쳐왔지만, $x(u)$는 오로지 $x(t)$에만 의존하며, 따라서 $p(x(u)\vert x(s),x(t))=p(x(u)\vert x(t))$로 나타난다는 것이다. 

이를 통해 Diffusion Process에서 각 state는 다음처럼 묘사된다.

$$
p(x_1\vert x_0) \\
p(x_2\vert x_1) \\
\vdots \\
p(x_T\vert x_{T-1})
$$

Diffusion Process에서 이전 state로부터 다른 state로의 움직임을 묘사하는 $p(x_t\vert x_{t-1})$을 $\textit{transition kernel}$ 은 주로 gaussian distribution으로 묘사된다. 즉, $p(x_t\vert x_{t-1}):=\mathcal N(x_t\vert \mu(x_{t-1}),\Sigma(x_{t-1}))$의 형태로 묘사된다. ($\mu(x_{t-1}),\Sigma(x_{t-1})$는 gaussian distribution의 평균과 분산이 $x_{t-1}$에 의해 결정된다는 뜻이다.) 이 과정이 실제로는 $x_{t-1}$에 gaussian noise를 입히는 형태로 수행되기 때문에 $corrupt$ 시킨다고 표현한다.

<div style="display: flex; justify-content: center; align-items: center; gap: 10px; width: 100%; flex-wrap: wrap;">
  <img src="https://upload.wikimedia.org/wikipedia/commons/9/99/X-Y_plot_of_algorithmically-generated_AI_art_of_European-style_castle_in_Japan_demonstrating_DDIM_diffusion_steps.png" alt="X-Y plot of algorithmically-generated AI art of European-style castle in Japan demonstrating DDIM diffusion steps.png"  style="width: 70%; height: auto;">
</div>
<p style="text-align: center; margin-top: 10px; font-size: 14px;">
  Source: <a href="https://en.wikipedia.org/wiki/Diffusion_model" target="_blank">Wikipedia - Diffusion Model</a><br>여기서는 step 1이 x(1)에 해당하고, step이 진행될 수록 x(0)에 가까워지는 것.
</p>

## Diffusion Models

이전 연구 ([Feller et al., 1949](https://digitalassets.lib.berkeley.edu/math/ucb/text/math_s1_article-21.pdf))에서 Diffusion Process의 대부분의 transition kernel을 되돌리는 transition kernel들이 같은 형태의 functional form을 띈다는 것을 보였다.

즉, $p(x_t\vert x_{t-1})$이 gaussian distribution이라면, $p(x_{t-1}\vert x_t)$도 gaussian distribution으로 묘사될 수 있다는 것이다.  

Diffusion Model은 이런 확산 과정의 성질을 이용하여, $p(x_t\vert x_{t-1})$을 되돌릴 수 있는 $p(x_{t-1}\vert x_t)=\mathcal N(x_{t-1}\vert \mu_\theta(x_t),\Sigma_\theta(x_t))$를 배우도록 학습한다. ([Sohl-Dickstein et al., 2015](https://arxiv.org/abs/1503.03585)). 

이를 통해 Diffusion Model은 다음과 같은 형태로 묘사된다.

$$
x_0:=x(0)\sim p_{data}(x)\underset{p_\theta(x_0 \vert x_1)}{\overset{p(x_1 \vert x_0)}{\rightleftarrows}} x_1 \rightleftarrows \cdots \rightarrow x_{T-1} \underset{p_\theta(x_{T-1} \vert x_{T})}{\overset{p(x_{T} \vert x_{T-1})}{\rightleftarrows}} x_T:=x(1),\ x(1)\approx z\sim \mathcal N(0,I)
$$




여기서, 복잡한 data distribution $p_{data}(x)$를 간단한 latent distribution $p(z):=\mathcal N(0,I)$로 바꾸는 과정, 이것을 되돌리는 과정 모두 Diffusion Process이기 때문에, data distribution을 corrupt하는 과정을 $\textit{Forward Diffusion Process}$, corrupted sample을 clean data로 되돌리는 과정을 $\textit{Reverse Diffusion Process}$라고 부른다.

## 마치며...

여기서는 Diffusion Model, 특히 거의 첫 논문인 [DDPM](https://arxiv.org/abs/2006.11239)을 이해하기 위한 배경지식 위주로 정리하였다. 다음 포스팅에서는 DDPM에 들어있는 수학적인 내용을 이해하기 위한 내용을 포스팅할 예정이다. 