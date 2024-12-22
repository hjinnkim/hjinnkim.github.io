---
title: "[Super Resolution] Transforming Image Super-Resolution: A ConvFormer-based Efficient Approach"
date: 2024-01-17 + 0030
categories: [Super Resolution, Light Weight]
tags: [super resolution]		# TAG는 반드시 소문자로 이루어져야함!
pin: false
math: true
---

# Transforming Image Super-Resolution: A ConvFormer-based Efficient Approach

오늘 본 논문은 arXiv "Transforming Image Super-Resolution: A ConvFormer-based Efficient Approach"이다. [(https://arxiv.org/abs/2401.05633v1?utm_source=tldrai)](https://arxiv.org/abs/2401.05633v1?utm_source=tldrai) 

Single Image Super Resolution 문제를 Efficient한 환경에서 수행하기 위해 연산량을 줄이면서 성능을 끌어올린 연구이다.

## Abstract

- Single Image Super Resolution (SISR) 분야는 뛰어난 성능을 보여주지만, computational cost로 인해 resource-restricted 환경에의 차용이 힘든 상황이다. 특히, 좋은 성능을 보여주는 Transformer-based 모델들은 Self-attention에서 큰 computational cost가 발생한다.
- 이에 저자들은 Large Kernel Convolution을 feature mixer로 사용하여, self-attention을 대체하면서 효율적으로 long range dependency를 modeling하는 *Convolutional Transformer layer (ConvFormer)*를 제시하였다.
- 또한, local feature를 aggregation하면서 high-frequency information을 보존하는 *Edge-preserving Feed-forward Network (EFN)*를 제시한다.
- 이 두 모듈을 합쳐 *ConvFormer-based Super-Resolution (CFSR)*를 소개한다.

## Introduction

- Computational cost와 performance를 적절히 가져갈 수 있는 새로운 module인 *Convolutional Transformer Layer (ConvFormer)*를 제시한다. 이를 이용하여 Super Resolution task를 수행하는 *ConvFormer-based Super-Resolution network (CFSR)*를 소개한다.
- *ConvFormer*는 Large Kernel Convolution을 차용하여 Self-attention을 대체한다. Self-attention을 배제함으로써, 연산량 및 메모리 사용량을 대폭 줄일 수 있었다.
- 또한, *Edge-preserving Feed-forward Network (EFN)*을 제시하여 edge extraction 성능을 향상시켰다. *EFN*은 기존 vision task에서 사용되는 3x3 depth-wise convolution에 image gradient prior를 합쳐 high-frequency 정보를 보존하면서도, re-parameterization을 통해 연산량이나 parameter의 증가없이 light-weight model을 위한 성능 향상을 이끌어냈다.

## Backgrounds

- SISR은 parameter를 늘림으로써 성공적으로 성능을 향상시켜왔지만, 동시에 resource-limited device에서 모델을 차용하기 위한 lightweight SISR 역시 많이 연구되어 왔다.
- 최근엔 Large Kernel Convnolution 이용한 SISR 연구가 좋은 성능을 보였다. (ShuffleMixer)
- Transformer-based SISR은 좋은 성능을 보여 왔지만 (SwinIR 등), self-attention으로 인해 CNN-based method에 비해 높은 computational resource가 요구된다.

## Pipeline

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=61f820d6-0c23-4e6f-b145-4f5f14e6dbcf" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="3-1.png"></iframe>

- 첫 3x3 convolution은 shallow feature extractor로 HxWx3 -> HxWxC 로 LR 이미지를 same resolution에서 latent space mapping 용이다.
- 이후 두개의 RCFB block은 deep feature extractor로 여러 *ConvFormer layer*가 포함되어 있으며, local feature aggregation을 위해 3x3 convolution이 RCFB를 뒤따른다.
- 마지막 reconstruction은 shallow feature와 deep feature의 합을 받아 HR image를 만드는데, 3x3 convolution과 pixel-shuffle operation이 포함되어 있다.
- Loss function은 L1 pixel loss를 사용한다.

## ConvFormer Layer

ConvFormer는 Self-attention을 대체하는 중요한 모듈이다. 

크게 두 개의 모듈로 이루어져 있다. 

### Large Kernel Mixer

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=dd644767-76f4-44a0-9f80-5240e9a2c820" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="4-1.png"></iframe>

이때, DWConv는 Depth-Wise Convolution이다. Kernel size K는 9를 사용했다고 한다. 보통 K << C 이기 때문에 Multi-Head Self-Attention 보다 효율적이라고 한다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=827b6917-cc61-4918-83a1-667bf6456e23" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="4-2.png"></iframe>

### Edge-preserving Feed-forward Network (EFN)

*EFN*의 operation은 다음과 같다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=3ddf9b14-46ab-40f9-9357-33662e286d01" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="4-4.png"></iframe>

Edge 정보를 중간 feature에 고려하게 되면 성능이 향상된다는 이전 논문들이 있다. Edge-preserving property는 중간 EDC에서 오는데, EDC의 묘사는 다음과 같다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=fa39ae51-4b52-4f6e-b7e6-090f50addc0f" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="4-3.png"></iframe>

결국 Depth-wise Convolution과 1 / 2차 미분 (gradient)을 탐지하는 sobel filter와 laplacian filter를 Depth-wise convolution의 filter로 사용하는 convolution을 (learnable) alpha의 조합으로 이루어진다.

식은 다음과 같다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=f8504991-8b03-4a87-9edd-e24372a89513" width="640" height="160" frameborder="0" scrolling="no" allowfullscreen title="4-5.png"></iframe>

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=f99134d0-9625-4e82-9ed2-699df5cf7027" width="640" height="160" frameborder="0" scrolling="no" allowfullscreen title="4-6.png"></iframe>

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=c0f5cc6e-7573-467b-ae07-fd8c32057daf" width="640" height="160" frameborder="0" scrolling="no" allowfullscreen title="4-7.png"></iframe>

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=d8f863d0-0087-4096-be71-c2157f7d2aaa" width="640" height="160" frameborder="0" scrolling="no" allowfullscreen title="4-8.png"></iframe>

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=599a43a5-5121-4c3b-ada3-6ff46ded6552" width="640" height="160" frameborder="0" scrolling="no" allowfullscreen title="4-9.png"></iframe>

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=93ad8c9d-2b20-4605-9e3c-2dd777f50fb2" width="640" height="160" frameborder="0" scrolling="no" allowfullscreen title="4-10.png"></iframe>

이때 Depth-wise convolution을 모두 따로 실행하면 computation이 많아지기 때문에 re-parametrization을 통해 한 번에 계산한다. (Merged EDC by re-parametrization in inference)

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=84bbbcb7-31a1-45c0-b036-297fc0388e28" width="640" height="160" frameborder="0" scrolling="no" allowfullscreen title="4-11.png"></iframe>

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=cac483ed-6ebb-45dd-b77a-92a5eccd6013" width="640" height="160" frameborder="0" scrolling="no" allowfullscreen title="4-12.png"></iframe>

## Performance

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=fca6de36-7de6-4152-ae1d-35e1d87c23a9" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="5-1.png"></iframe>

더 적은 parameter와 FLOPs로 Transforme-based 모델에 근접한 성능을 보여준다.

## 마무리

SISR 분야에서 Depth-wise Convolution이 주력으로 쓰인다는 걸 알았다. Large Kernel Convolution이 Long-range dependency를 모델링하는데 효과적이라는 점은 새로 알게 되었다. 다만, Self-attention처럼 모든 spatial을 고려하려면 *ConvFormer* block을 여러 번 통과해야함으로, Diffusion model architecture 차용하기에는 힘들 것으로 보인다. 

그럼에도, Lightweight Transformer-based method들에 근접한 성능을 only CNN-based method로 보이는 것은 고무적인 결과로 보인다.