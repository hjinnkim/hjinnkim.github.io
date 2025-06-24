---
title: "[Paper Review] A Structured Dictionary Perspective on Implicit Neural Representations (CVPR 2022)"
categories:
  - Paper Review
tags:
  - INR
  
excerpt: "[Paper Review] A Structured Dictionary Perspective on Implicit Neural Representations (CVPR 2022)"

last_modified_at: 2025-06-24T10:40:00
---

# A Structured Dictionary Perspective on Implicit Neural Representations (CVPR 2022)

> A Structured Dictionary Perspective on Implicit Neural Representations. [[Paper](https://arxiv.org/abs/2112.01917)] \\
> Gizem Yüce, Guillermo Ortiz-Jiménez, Beril Besbinar, Pascal Frossard \\
> Mar 25th 2022

## Introduction

Implicit Neural Representation (INR)의 표현력(expressive power)과 귀납적 편향(inductive bias)를 이론적으로 탐색한 논문. 

- Implicit Neural Representation (INR): coordinatge $(x,y,z)\rightarrow$ Signal

- 해당 논문의 핵심은 INR(주로 ReLU-MLP 혹은 SIREN)이 표현할 수 있는 Signal의 주파수는 입력층의 signal을 조합하여 만드는 것이라는 것. 
- 즉, 입력층(mapping γ)을 통해 품은 특정 주파수를 정규 사전처럼 사용하고,
- Non-linear layer의 출력값은 입력 주파수들의 정수 배수 주파수(harmonics)의 선형 결합을 형성한다.
- 이는 파라미터가 선형적으로 증가함에도, 이론적으로 INR이 표현할 수 있는 신호의 조합 가능 주파수가 지수적으로 증가한다. (이론적. 실제로는 여러 문제가 생길 수 있음. e.g. aliasing ) 

## Derivations

해당 논문을 모두 이해가기는 어려웠고, 몇 가지 궁금했던 유도들의 캡쳐본을 올린다.
<div style="text-align: center;">
  <img src="/assets/paper-review/inr/2025-06-24-structured-perspective/1.jpg" width="80%"/>
</div>
<div style="text-align: center;">
  <img src="/assets/paper-review/inr/2025-06-24-structured-perspective/2.jpg" width="80%"/>
</div>
<div style="text-align: center;">
  <img src="/assets/paper-review/inr/2025-06-24-structured-perspective/3.jpg" width="80%"/>
</div>
<div style="text-align: center;">
  <img src="/assets/paper-review/inr/2025-06-24-structured-perspective/4.jpg" width="80%"/>
</div>
<div style="text-align: center;">
  <img src="/assets/paper-review/inr/2025-06-24-structured-perspective/5.jpg" width="80%"/>
</div>
<div style="text-align: center;">
  <img src="/assets/paper-review/inr/2025-06-24-structured-perspective/6.jpg" width="80%"/>
</div>

---

모든 수식을 Markdown으로 적을 엄두가 안 나서 그냥 캡쳐본을 첨부해버렸다. 다음엔 첨부하기 좋게 Pad에서 바로 수식을 유도해야겠다.

관련된 신호 주파수 표현 유도가 있는 [BACON](https://arxiv.org/abs/2112.04645)을 읽어보면 좋을 것 같다.