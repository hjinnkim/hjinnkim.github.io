---
title: "[Text-to-Video] Text-to-Video: The Task, Challenges and the Current State"
date: 2024-01-26 + 0130
categories: [Video, Text-to-Video]
tags: [t2v, blog]		# TAG는 반드시 소문자로 이루어져야함!
pin: false
math: true
---

# Text-to-Video: The Task, Challenges and the Current State

오늘은 논문이 아닌 블로그 포스트를 보았다. "Text-to-Video: The Task, Challenges and the Current State"[(https://huggingface.co/blog/text-to-video?utm_source=tldrai)](https://huggingface.co/blog/text-to-video?utm_source=tldrai) 

최근 Video관련 연구의 시작 단계에 있는데, pre-trained large model을 활용할 수 있지 않을까하여 관련 내용을 찾아보고 있다. Text-to-Video에 대한 기초 지식이 없는 지금, 이러한 블로그 포스트를 읽어보면 기초를 비교적 쉽게 쌓을 수 있다.

## Introduction

Text-to-Video는 text로부터 공간적-시간적으로 일관된 일련의 이미지를 생성해야 하지만, 굉장히 어려운 문제 중 하나이다. 이는 Text-to-Image 문제와 굉장히 비슷해보이지만, 많은 차이가 존재한다.

## Text-to-Video vs. Text-to-Image

초창기 Text-to-Image 모델들은 GAN-based 모델들인 VQGAN-CLIP, XMC-GAN, GauGAN2 등이다. 이후 OpenAI에서 Tranformer-based 모델인 DALL-E, DALL-2를 선보였고, 이후 Stable Diffusion과 Imagen으로 대표되는 Diffusion Model들이 등장하였다.

뛰어난 Text-to-Image 성능에도 불구하고, (Diffusion-based든 Non Diffusion-based든) Text-to-Video 모델은 생성 성능이 더욱 떨어졌다. Text-to-Video 모델은 주로 매우 짧은 클립들로 학습되어, 긴 이미지를 생성하기 위해선 큰 계산 비용과 느린 sliding window 방식을 취해야 한다는 것을 의미한다. 결과적으로 Text-to-Video들은 사용 / 확장이 어렵고 문맥이나 길이에 있어 여전히 제한적인 상황으로 악명이 높다.

Text-to-Video 문제는 다음과 같은 큰 난관에 직면하고 있다.

- 계산 코스트 문제: 프레임간 공간적, 시간적 일관성을 유지하는 것은 높은 계산 코스트를 야기하고, 그러한 모델을 학습하는 것은 대부분의 연구자들에게 너무 큰 비용을 요구한다.

- 고품질의 데이터셋 부족: Text-to-Video 모델 생성을 위한 Multi-modal 데이터셋은 부족하고, 거의 드물게 annotate되어 있어 복잡한 움직힌 의미를 배우기 힘들게 한다.

- 비디오 캡션의 모호함: 모델이 쉽게 학습할 수 있도록 비디오를 묘사하는 방법은 여전히 열린 문제이다. 비디오를 완전히 묘사하기 위해서는 한 줄 이상의 text prompt가 필요하다. 생성된 비디오는 시간에 걸쳐 여러 줄의 prompt나 이야기로 제약되어야 한다.

다음 단락에서는 Text-to-Video의 발전 과정과 여러 난관들을 해결하기 위한 여러 방법들에 대해 논의한다. 더 큰 시야에서, Text-to-Video는 다음 중 하나를 제시한다.

- 모델의 학습을 쉽게하는 고품질의 새로운 데이터셋

- Text-Video 데이터쌍없이 모델을 학습시킬 수 있는 방법

- 긴, 고해상도의 비디오를 생성할 수 있는 더 효율적인 계산 방법

## How to Generate Videos from Text?

Text-to-Video 문제와 비슷하게, Text-to-Video 초창기 연구들은 주로 GAN / VAE-based 방법을 취하여 auto-regressively 캡션으로부터 프레임을 생성하였다 (see [Text2Filter](https://huggingface.co/papers/1710.00421)  and [TGANs-C](https://huggingface.co/papers/1804.08264)). 이 방법들은 Text-to-Video의 기반을 제안하였지만, 저해상도, 짧은 길이의 제한된 움직임만을 생성할 수 있었다.

언어 분야의 대규모, 사전학습 트랜스포머 모델 (large-scale pretrained transformer models)인 GPT3의 성공에서 영감받아, 이후의 Text-to-Video 모델은 Transformer 구조를 차용하였다. [Phenaki](https://huggingface.co/papers/2210.02399), [Make-A-Video](https://huggingface.co/papers/2111.12417), [NÜWA](https://huggingface.co/papers/1710.00421), [VideoGPT](https://huggingface.co/papers/2104.10157), [CogVideo](https://huggingface.co/papers/2205.15868)는 모두 트랜스포머 구조 기반 프레임워크를 제안하였고, [TATS](https://huggingface.co/blog/text-to-video?utm_source=tldrai)는 이미지 생성을 위해서는 VQGAN, 이미지의 연속적인 생성을 위해서는 시간축에 민감한 트랜스포머 구조를 차용한 하이브리스 방법을 제시했다. 이 중에서도 Phenaki는 일련의 prompts을 조건부로 긴 비디오의 생성을 가능하게 하여 주목을 받았으며, [NUWA-Infinity](https://huggingface.co/papers/2207.09814)는 autoregressive를 통해 text input으로부터 긴, 고품질의 비디오를 생성할 수 있다. 하지만, Phenaki와 NUWA 모두 일반에 공개되지 않았다.

다음, 그리고 현재의 Text-to-Video 모델의 연구 흐름은 Diffusion-based 구조이다. 이미지 생성 분야에서의 Diffusion model의 엄청난 성공은 audio, 3D, Video 등의 다양한 분야에서 주목을 끌기 시작했다. 비디오 생성 분야에서는 Diffusion model을 Video 생성에 확장한 [VDM](https://huggingface.co/papers/2204.03458)와 Latent space에서 짧은 비디오 클립을 생성하여 VDM보다 효율적인 [MagicVideo](https://huggingface.co/papers/2211.11018)으로부터 시작된다. Text-to-Image Pretrained 모델을 하나의 text-to-video 데이터쌍으로 fine-tune시키고, 모션을 유지한 채 비디오를 생성할 수 있는 [Tune-a-Video](https://huggingface.co/papers/2212.11565)도 중요하다. 이 모델들의 이어 [Video LDM](https://huggingface.co/papers/2304.08818), [Text2Video-Zero](https://huggingface.co/papers/2303.13439), [](https://huggingface.co/papers/2212.11565), [Runway Gen1 and Gen2](https://huggingface.co/papers/2302.03011), [NUWA-XL](https://huggingface.co/papers/2303.12346)가 등장하였다. 

Text2Video-Zero는 ControlNet의 형태로 Text로 Video 생성을 유도하고, 조작하는 프레임워크이다. 이는 Text-input, text-pose, text-edge 데이터 입력을 기반으로 비디오를 직접적으로 생성 (혹은 편집)할 수 있다. 이름에서 알 수 있듯이, Text2-Video-Zero는 학습 가능한 motion dynamics 모듈과 pre-trained Text-to-Image Stable Diffusion을 Text-to-Video 데이터쌍없이 결합시킬 수 있는 zero-shot 모델이다. Runway의 GEN-1/2 모델은 text나 image를 통해 묘사된 내용을 통해 비디오를 생성할 수 있다. 위의 대부분의 연구들은 짧은 비디오 클립으로 학습되어 긴 비디오를 생성하기 위해 sliding window를 통한 autoregressive 방식의 생성에 의존해야 하였고, 결괒거으로 context gap이 존재한다. NUWA-XL은 3376 프레임에서 학습하기 위해 "diffusion over diffusion" 방식을 제안하여 위 문제를 해결하였다. 마지막으로, Alibaba / DAMO Vision Intelligence Lab의 **ModelScope**와 Tencel의 **VideoCrafter**과 같은 학회나 저널에서 발표되지 않은 오픈소스 Text-to-Video 모델들이 존재한다.

## Datasets

Vision-Launguage 모델들처럼, Text-to-Video 모델들은 주로 비디오와 텍스트의 대규모 Paired 데이터셋으로 학습된다. 이러한 데이터셋의 비디오들은 주로 짧고, 고정된 길이로 쪼개져있으며, 적은 물체의 독립된 움직임으로 이루어져 있다. 이는 부분적으로는 계산적 제약과 비디오 내용을 의미있는 방식으로 묘사하기 위함이며, Multi-modal Video-Text 데이터셋과 text-to-video 모델이 서로 연관되는 것을 볼 수 있다. 몇몇 연구들은 모델이 학습하기 더 쉬운, 좋으면서도 잘 일반화되는 데이터셋을 연구하지만, [Phenaki](https://phenaki.video/?mc_cid=9fee7eeb9d#) text-to-video 연구를 위해 text-image쌍과 text-video 쌍을 합치는 대안 연구를 탐색했다. Make-a-Video에서는 text-image 쌍만 사용하여 세상이 어떻게 생겼는지를 배우고, 비디오 데이터만을 사용하여 비지도 방식으로 spatio-temporal dependencies를 학습한다.

이러한 대규모 데이터셋들은 text-to-image 데이터셋과 비슷한 무제를 겪는다. 가장 흔히 쓰이는 text-video 데이터셋인 [WebVid](https://maxbain.com/webvid-dataset/)는 10.7 million text-video쌍으로 이루어져 있으며, 꽤 다수의 연관없는 video description의 noisy sample들을 포함하고 있다. 다른 데이터셋들은 특정 작업이나 분야에 집중하여 이 문제를 극복하려 했다. [HowTo100M](https://www.di.ens.fr/willow/research/howto100m/)는 136 million의 캡션 있는 비디오 클립 데이터셋으로 요리, 수공예와 같은 복잡한 작업을 어떻게 단계별로 수행하는지를 묘사하는 캡션이 포함되어 있디. [QuerYD](https://www.robots.ox.ac.uk/~vgg/data/queryd/)는 물체의 상대적 위치나 동작을 자세히 묘사하는 비디오 캡션을 통해 event localization 문제에 집중한다. [CelebV-Text](https://celebv-text.github.io/)는 대규모의 얼굴 text-video 데이터셋으로 현실적인 얼굴, 감정, 동작이 잇는 비디오 생성을 위한 70K 비디오로 이루어져 있다.

## Text-to-Video at Hugging Face

Hugging face 데모: [Demo](https://huggingface.co/blog/text-to-video?utm_source=tldrai#text-to-video-at-hugging-face)

## 마무리

Text-to-Video 연구를 위해 어떤 연구들을 훑어봐야 하는지 훑어볼 수 있는 좋은 글이었다. 