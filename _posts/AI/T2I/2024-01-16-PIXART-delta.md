---
title: "[Text-to-Image] PIXART-δ:FASTANDCONTROLLABLEIMAGE GENERATIONWITHLATENTCONSISTENCYMODELS"
date: 2024-01-16 + 0030
categories: [Diffusion model, T2I]
tags: [text-to-image, diffusion model, ]		# TAG는 반드시 소문자로 이루어져야함!
pin: false
math: true
---

# PIXART-δ: Fast and Controllable Image Generation with Latent Consistency Models

오늘 본 논문은 arXiv "PIXART-δ: Fast and Controllable Image Generation with Latent Consistency Models"이다. [(https://arxiv.org/abs/2401.05252?utm_source=tldrai)](https://arxiv.org/abs/2401.05252?utm_source=tldrai) 

기존의 저자들의 연구인 "PIXART-$\alpha$"를 발전시키는 Technical report이다. 

## Abstract

- Photo-realistic text-to-image synthesis diffusion transformer 모델인 "PIXART-$\alpha$"에 *Latent Consistency Model* 를 적용하여 Inference 효율성 & 학습 효율성 & 성능을 향상시켰다.
- Text-to-Image Diffusion 모델에 다양한 condition을 주는 연구인 *ControlNet* 을 Diffusion Transformer 구조에 맞게 개량한 *ControlNet-Transformer* 구조를 제안하였다.

## Introduction

- *PIXART-$\alpha$* 는 1024px 이미지를 생성할 수 있는 Text-to-Image diffusion transformer 모델이다.
- *LCM (Latent Consistency Model)* 을 적용하여 추론 (inference)를 가속화하였다. (~4 steps)
- 합친 모델의 이름은 *PIXART-δ* 이다. 1024x1024 이미지 생성에 0.5초 (A100)가 걸리며, *PIXART-$\alpha$* 에 비해 7배 빨라졌다.
- *LCM-LoRA* 를 지원하여 더욱 효율적인 사용이 가능하다.

## Backgrounds

- *Consistency Model / Latent Consistency Model / LCM-LoRA*
- *ControlNet*

## LCM In PIXART-δ

*PIXART-$\alpha$* 에 *LCM* 을 적용한 알고리즘을 설명한다.

<p align="center">
<img src="{{site.gdrive_url_prefix}}13r_NhmTLDNZwln9vWRNOUK8O_HoV3OGD"
     title="LCM algorithm" 
     style="float: center; width:80%">
</p>