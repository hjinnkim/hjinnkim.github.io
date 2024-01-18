---
title: "[Super Resolution] Improving the Stability of Diffusion Models for Content Consistent Super-Resolution"
date: 2024-01-18 + 1030
categories: [Diffusion Model, SR]
tags: [super resolution, diffusion model, ]		# TAG는 반드시 소문자로 이루어져야함!
pin: false
math: true
---

# Improving the Stability of Diffusion Models for Content Consistent Super-Resolution

오늘 본 논문은 arXiv "Improving the Stability of Diffusion Models for Content Consistent Super-Resolution"이다. [(https://csslc.github.io/project-CCSR/)](https://csslc.github.io/project-CCSR/) 

Diffusion prior-based Super Resolution task들이 매 iteration마다 달라지는 noise에 따라 생성되는 이미지의 결과가 달라지는 문제를 해결하기 위해 여러 method를 제시하는 논문이다.

## Abstract

- 기존 Diffusion prior-based Super Resolution들이 동일한 LR Image에 대해 서로 다른 noise에 따라 매번 다른 HR Image를 생성한다는 문제가 발생한다.
- 저자는 이 문제를 해결하기 위해 1) Diffusion model을 Image structure refinement에만 활용하고, 2) Generative Adversarial training을 활용하였다. 
- 또한, *None-uniform timestep learning strategy*를 제시하여 compact한 diffusion network를 학습시키면서도, Image main structure를 안정적이면서도 효율적으로 생성한다.
- Detail enhancement를 위해서는 VQ-VAE decoder로 Image fine detail enhancement를 수행하였다.
- 저자는 이 method를 *Content Consistent Super-Resolution (CCSR)* 이라고 명명하였다.

## Introduction

- 기존의 Diffusion prior-based Super Resolution 모델들은 Noise sample이 달라지면 같은 LR Image에서 다른 HR Image가 생성된다.
- 이는 T2I task에서는 도움이 되는 Generative property가 Content Consistency가 중요한 SR task에서는 GT와 다른 texture를 생성하게 하기 때문이다.
- 저자는 이 문제들을 해결하기 위해 Diffusion prior-based Super Resolution task의 stability를 향상시키기 위해 *Content Consistency Super-Resolution (CCSR)* 를 제시한다.
- Diffusion prior-based method는 GAN-based method에 비해 훨씬 강력하고 유연해서, realistic하고 visually pleasing한 image content를 만든다. 이 특성은 heavy degradation과 information loss에서 오는 LR Image에서 GAN에 비해 좋다.
- Diffusion Model로 structural content를 잘 회복시키면, GAN netowrk가 low stochasticity를 통해 잘 enhance할 수 있다.
- 저자는 Diffusuion prior는 LR Image로부터 coherent structure를 생성하고, Generative Adversarial Training을 통해 detail과 texture를 enhancement한다.
- 이를 위해 Diffusion stage에서 Non-uniform timestep sampling strategy를 제시하여 효율적으로 information extraction를 강화하면서도 image structure generation의 안정성을 향상시켰다.
- Adversarial training stage에서는 pre-trained VAE decoder를 finetune시켜 latent feature decoding 성능과 detail enhancement 성능을 추가적인 computational burden없이 부여하였다.

## Backgrounds

- *ControlNET*
- *DiffBIR*

## Motivation

Diffusion model은 reverse diffusion time-step에 따라 reconstruction하는 정보가 다르다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=5666c0c0-2177-4221-a52d-b45b56fa9463" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="3-1.png"></iframe>

StableSR의 timestep별 PSNR / LPIPS를 보면 초반에는 PSNR이 증가하는 structural refinement를 보이며, 후반에는 PSNR이 떨어지는 structural change가 보인다.

SwinIR은 PSNR은 높지만 LR이 highly corrupted되어 있으면 quality가 안좋은 것을 볼 수 있다. SwinGAN은 fine detail reconstruction에 약한 것을 볼 수 있다. 반면 StableSR은 strong diffusion prior로 훨씬 realistic한 것을 보이며, LPIPS도 가장 좋은 지점까지 떨어진다.

하지만 LR에 충분한 structural information이 있다면 SwinGAN이 StableSR에 근접한 quality를 보여주는 것을 알 수 있다.

이 관찰로부터 저자는 *CCSR* 를 착안하게 되었으며, 이는 Structure refinement (top left)와 detail enhancement (top right) 두 stage로 이루어져있다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=8dbb1fae-af5b-4851-8a53-28f820f8631e" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="3-2.png"></iframe>

## Framework

전체 Framework는 ControlNet과 pre-trained SD를 기반으로 작성되었다.

First stage에서는 Non-uniform sampling strategy를 제시하였다. 첫 timestep (From $T \rightarrow t_{max}$)는 Feature Extraction from LR을 수행하여 distribution을 matching하는 용도로 사용되며, 다른 여러 timestep ($t_{max}\rightarrow t_{min}$)은 Image structure generation을 수행한다. $t_min$을 수행한 뒤, 예측된 결과는 second stage로 넘겨지며, second stage에서는 VAE decoder를 adversarial training을 포함하여 finetune시켜 feature decoding 성능과 detail enhancement를 추가적인 computation burden없이 동시에 수행한다.

## First Stage: Non-Uniform Timestep Sampling

기존 DM-based SR method들은 T2I task를 따라 Uniform sampling strategy를 수행한다. 하지만 T2I image generation은 scratch부터 모든 pixel를 생성해야하지만 SR task에서는 desired image를 위한 coarse structure가 존재한다. 기존의 Uniform sampling strategy는 LR input을 충분히 활용하지 못하고 반복적으로 coarse structure를 생성하며 불필요한 computation을 반복한다. 또한, different noise에서 오는 texture difference도 발생한다.

First stage에서는 Non-uniform sampling strategy를 사용하여 structure refinement를 수행한다. Feature extration을 위해서는 LR image에서 coarse structure를 1 step만을 사용하여 Gaussian noise $x_T$를 intermediate noisy $x_{t_{max}}$로 mapping한다. Structure generation을 위해서 $t_{max}\rightarrow t_0$을 모두 수행하는 것이 아닌 $t_{max}\rightarrow t_{min}$까지만 수행하고, $t_{min}$에서 예측한 $\hat x_0$를 second stage로 넘겨 fine detail enhancement를 수행한다. 이 truncated approach는 이미 structure가 충분히 복원되었기 때문이다.

이때, $t=T\rightarrow t=t_{max}$ 로의 mapping은 기존 step보다 훨씬 크기 때문에 단순히 Non-uniform sampling strategy를 적용하는 것은 performance loss를 일으킨다. 저자는 sampled noise를 제약하기보단 $\hat x_{0\leftarrow T}$를 constrain하기로 했다.

Estimaged $\hat x_{0\leftarrow T}=\frac{x_T-\sqrt{1-\bar \alpha}\epsilon_\epsilon(x_T, T)}{\sqrt{\bar \alpha}}$

$l_T=||x_0-\hat x_{0\leftarrow T}||^2$

$\hat x_{t_{max}}=\sqrt{\bar \alpha_{t_{max}}} \cdot \hat x_{0\leftarrow T}+\sqrt{1-\bar\alpha_{t_{max}}}\cdot \epsilon$

$l_{t_{max}}=||x_0-\frac{1}{\sqrt{\bar\alpha_{t_{max}}}}(\hat x_{t_{max}}-\sqrt{1-\bar\alpha_{t_{max}}}\epsilon_{t_{max}})||^2$

CCSR loss at t=T: $l^T_{diff}=l_{diff}+l_T+l_{t_{max}}$

(그런데 $l_{diff}+l_T$는 그냥 scaled $l_{diff}$ 같은데 구현된 코드를 봐야할 것 같다.)

## Detail Enhancement Stage

여러 논문이 VAE decoder가 충분한 power를 가졌다는 이전 연구들을 따라 Decoder는 VQ-VAE2 Generative Adversarial training을 따라 finetune시켰다.

## Remarks

$T=45$ / $T_{max}=2/3 T$ / $T_{min}=1/3 T$

## New Stability Measures

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=2bf6085c-4939-43a1-8f0e-40fccb7b1e53" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="4-1.png"></iframe>

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=be528450-a233-487a-8cfe-e00eeddd3a08" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="4-2.png"></iframe>

p=metric / N번 평가해서 Standard Deviation을 뽑은 것으로, 여러 번 평가했을 때 얼마나 steady한 결과를 생성하냐를 측정. Method에 맞는 새로운 metric을 제시한 것.

## 결과

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=b0c6d57b-b1aa-4560-ac77-398c28d3f38d" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="5-1.png"></iframe>

결과도 좋고, STD도 낮다. Good.

## 마무리

이 논문을 보고 실험해보고 싶은 아이디어가 생각이 났다. 추후 실험하게 되면 포스트해보겠다. 빨리 현재 읽고 있는 GAN paper들을 마무리하고 Diffusion Model로 넘어가서 연구에 진척을 만들고 싶다.
