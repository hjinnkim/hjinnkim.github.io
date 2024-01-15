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
- *LCM-LoRA* 를 지원하여 더욱 메모리 효율적인 사용이 가능하다.

## Backgrounds

- *Consistency Model / Latent Consistency Model / LCM-LoRA*
- *ControlNet*

## LCM In PIXART-δ

*PIXART-$\alpha$* 에 *LCM* 을 적용한 알고리즘을 설명한다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=743e7c81-eb1d-4e83-aeac-8f62937d5f75" width="480" height="540" frameborder="0" scrolling="no" allowfullscreen title="3-1.png"></iframe>

Batch size가 크면 FID / CLIP score가 좋아지지만, Batch size가 작아도 5000 iterations 정도에서 수렴한다고 한다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=ee6bae9a-626a-4661-bda8-d9c8b99648a9" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="3-2.png"></iframe>

## Training Efficiency And Inference Speedup

PIXART-δ 모델와 SDXL LCM-LoRA, Stable Diffusion 모델의 학습 및 추론 속도 비교. 더 적은 GPU memory로 학습이 가능해졌다. (데이터 볼륨은 자체 데이터를 활용하기 때문에 차이가 있다.) LCM을 차용하면서 sampling step도 줄어들었다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=4e0dcd5f-5872-4e3c-b057-9862cc5da864" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="3-3.png"></iframe>

## ControlNet In PIXART-δ

ControlNet이 U-Net base Diffusion 모델에서 뛰어난 Conditioning을 보여주었다. 저자들은 Transformer 구조를 가지고 있는 PIXART-δ 에 ControlNet을 적용하기 위해 실험들을 진행했다.

<iframe src="https://unistackr0-my.sharepoint.com/personal/hyeonjin_kim_unist_ac_kr/_layouts/15/embed.aspx?UniqueId=38f784af-5621-4008-bf4d-119d49949558" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="4-1.png"></iframe>

ControlNet은 Diffusion Model의 Encoder 부분을 copy하여 train 한다. PIXART-δ 의 transformer 구조에는 Encoder / Decoder 가 명확히 구분되어 있지 않기 때문에 13번째 transformer 구조까지를 Encoder로 취급하여 copy한다.

또한, ControlNet의 중요한 부분 중 하나인 zero convolution 부분은 zero linear module로 대체되었다. 

마지막으로 ControlNet의 output은 U-Net Decoder에도 계속 더해졌는데, 이를 PIXART-δ 에 적용할 경우 첫 13개 Transformer block에만 더해지도록 변경되었다.

## 마무리

오늘 첫 페이퍼 리뷰를 적어보았다. 최신 논문을 간단하게 매일 정리하는게 목표이기 때문에 최대한 간단하게 정리하였다. 당연히 디테일이 많이 부족한 리뷰가 되었지만, 최신 트렌드를 파악하는 목적이기 때문에 이러한 기조로 진행할 것 같다. Diffusion model을 공부하는 입장에서 봐야할 논문이 많이 눈에 띄었다. 이 Technical report에서 refer된 논문들은 모두 읽어야함을 느꼈다.