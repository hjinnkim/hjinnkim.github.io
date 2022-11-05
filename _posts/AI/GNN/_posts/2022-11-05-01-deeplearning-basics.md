---
title: "[GNN] 01. Deep Learning Basics"
excerpt: "Deep Learning Basics"

categories:
  - GNN
tags:
  - [Deep Learning, GNN]

permalink: /courses/gnn/001/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-11-05
last_modified_at: 2022-11-05
order: 1
published: false
use_math: true

---
# Part 1 딥러닝의 기초

## 0. Table of Contents

1. [Neural Networks](#1-neural-networks-신경망)
2. [Convolutional Neural Networks](#2-convolution-neural-networks-cnns)
3. [Recurrent Neural Networks](#3-recurrent-neural-networks-rnns)
4. [Autoencoders](#4-autoencoders)

## 1. Neural Networks (신경망)

- 생물학적 뉴런을 인공적으로 모방한 모델. (1943년 W. McCulloch에 의해 소개되었음)
- 퍼셉트론은 활성화 함수가 계단 함수(Step Function)인 모델. (1957년 F. Rosenblatt에 의해 소개되었음)
  - 퍼셉트론은 간단한 XOR 문제를 풀지 못하는 한계를 가짐.
  - 퍼셉트론을 여러 층으로 쌓으면 이 문제를 풀 수 있는 것을 발견함 (Fully Connected Layers)
  - 이렇게 퍼셉트론을 쌓는 것을 Multilayer Perceptron(MLP)
- 인공신경망 (Artificial Neural Network)가 많은 수의 은닉층 (Hidden layers)를 가지고 있을 때, 심층신경망 (Deep Neural Network, DNNs))이라고 부른다. 
  - 하지만 은닉충의 개수가 많다 (deep)의 기준이 명확하지 않기 때문에, 신경망 구조를 학습시키는 것을 Deep Learning이라고 총칭한다.
- 1986년 이전에도 DNNs의 개념은 존재했지만, 어떻게 MLP를 훈련할지 정해지지 않았음.
  - 1986년 D. Rumelhart, G. Hinton 그리고 R. Williams가 *Backpropagation training algorithm* 논문을 공개함.
  - 경사하강법을 사용하는 알고리즘 (Gradient Descent)
    1. 계단 함수는 경사하강법을 적용할 수 없음 (기울기가 없음)을 저자는 깨달음.
      - 활성화 함수를 변경 $\rightarrow$ Sigmoid 함수 (기울기가 생김)
    2. 연쇄 법칙을 적용.
- 오늘날에는 다양한 활성화 함수가 소개됨.
  - Sigmoid, Tanh (Hyperbolic tangent), ReLU, Leaky ReLU, GeLU, etc.
    - ReLU의 변형들은 음수에서 함수의 값을 0으로 하지 않게 한다.
- Neural Network는 몇 가지 장단점이 있음.
  - 복잡한 문제를 풀 수 있는 유연성을 제공
  - 그런 유연성은 많은 하이퍼패러미터로 인해 trade-off가 있다.
    - 주어진 하이퍼패러미터 조정을 효율적으로 도와주는 여러 라이브러리가 존재한다.
  - 하지만 모델의 아키텍처가 잘못되었다면 하이퍼패러미터를 아무리 잘 조정해도 성능이 개선되지 않는다.
- 신경망을 다음을 주로 고려한다.
  - 은닉층이 얼마나 많은가?
  - 은닉층당 얼마나 많은 뉴런이 존재하는가?
    - 모두 과적합의 위험을 내재함.

## 2. Convolution Neural Networks (CNNs)

- CNN은 신경망의 종류 중 하나.
  - 인간의 시각 피질이 어떻게 작동하는지를 연구한 결과
  - 이미지 기반 작업에 효과적
    - Image recognition, Image detection, etc...
  - 심지어는 NLP나 Voice recognition 에서도 많이 쓰임.
- 실제 시각 피질에서는 제한된 영역의 자극에만 반응하는 뉴런들을 찾을 수 있음.
  - 각 뉴런의 관심 영역은 겹칠 수 있음.
- 뉴런들은 각각의 특징을 가지고 있음.
  - 그들 중 몇몇은 수평선에만 반응하고, 몇몇은 수직선에만 반응한다.
  - 또 몇몇은 더 복잡한 패턴에 반응한다.
- FCNs (Fully Connected Networks)는 왜 이미지 처리에 사용되지 않을까?
  - 저 해상도의 200x200 pixel 이미지를 생각해보자. (총 40000 pixel)
  - 첫 layer의 Neuron의 수를 4000으로 잡아도 (1:10 축소, 정보 손실), 중간 연결의 수는 4000만이 된다. (**어째서 4000만? 4000x40000=1억 6천 아닌가?**)
  - CNNs의 각 Neuron은 모든 입력 pixel에 연결되지 않고, 각 visual region / receptive field에 연결된다. $\rightarrow$ 즉, 연결 수가 훨씬 적음.
  <p align="center">
  <img src="{{site.gdrive_url_prefix}}1EVxUFDSBAsRsaj-UGTaWSB-ivw8FcJKl" title="1_CNN" style="float: center; width:50%">
  </p>
- CNNs의 중요한 개념 / 패러미터
  - Filters
    - 특정 패턴에 집중할 수 있게 해줌. (수직선, 수평선 등)
    - 같은 필터를 쓰는 layer는 feature map을 결과로 출력함.
  - Zero padding
    - 인풋의 경계에 0 픽셀들을 추가하는 것. 아웃풋 사이즈를 조정함. (인풋 사이즈를 보존할 수 있음)
  - Stride
    - receptive field의 겹치는 정도를 조정할 수 있음.
    - 필터가 몇 간씩 뛰어넘을지 결정.
  - Depth
    - 필터의 개수를 지정.
    - output feature map의 개수를 결정
- Pooling layers
  - feature map의 크기를 조정 (패러미터 감소).
  - feature map의 중요한 정보를 추출하는 역할.
  - Max pooling / Average pooling
- 몇몇 중요한 CNNs
  - AlexNet(2012)
  - GoogleNet(2014) $\rightarrow$ Inception Module. 참고: [Inception Module](https://kangbk0120.github.io/articles/2018-01/inception-googlenet-review)
  - VGG16(2014)
  - ResNet(2015) $\rightarrow$ Residual Learning
  - Inception-v4 (GoogleNet+ResNet)
  - Xception(2016) $\rightarrow$ Inception Module를 depthwise separable convolution layers로 변경
  - SENet(2017)
  - YOLOv3(2018)$\rightarrow$ 몹시 빠른 객체 검출

  $\Rightarrow$ 핵심은 네트워크는 깊어지지만 패러미터는 적어진다는 것.

## 3. Recurrent Neural Networks (RNNs)

- 지금까지는 인공 신경망이 Feedforward Network라고 가정했다. (입력층에서 출력층으로 이동)
- RNNs에서는 자신의 출력이 자신의 입력으로 들어온다.
  - t-1시점의 출력을 t시점의 입력과 함께 다시 자신에게 전달한다.
- 각 RNN는 두 가지 형태의 가중치를 가진다.
  - input X, output for time t-1
- t 시점의 뉴런의 출력은 이전 시간의 입력들에 대한 함수가 되기 때문에, 뉴런의 출력이 기억(memory)를 가지고 있다고 말할 수 있다.
  - RNNs가 과거를 기억하고 미래를 예측한다고 생각할 수 있음.
- RNNs의 목표에 따라 다른 형태를 가짐.
  - Sequence-to-Sequence Network: 주가 예측과 같은 시계열 예측에 유리. 지난 X days동안의 입력으로부터 X+1~X+Y 예측(Y날동안의 미래를 예측)
  - Sequence-to-Vector Network: 신경망의 결과에만 관심이 있음. ex) 문장이 주어지면 의미를 예측 (+1 positive ~ -1 negative)
  - Vector-to-Sequence: 이미지를 받으면 이미지의 캡션을 생성.
  - Encoder/Decoder : Sequence-to-Vector 뒤에 Vector-to-Sequence가 연결됨. 언어 번역에 사용됨.
- 어떻게 훈련시키나?
  - BackPropagation Through Time(BPTT): 네트워크를 시간에 따라 풀었다가(unroll) 전형적인 역전파를 적용. 참고: [BPTT](https://velog.io/@jody1188/BPTT)

## 4. Autoencoders

- Autoencoders는 입력값의 dense representation을 배우는 ANNs이다.
  - 비지도학습.
  - Dense representation은 Latent representation 혹은 Latent coding 이라고 부르기도 함.
- 차원 축소, 특징 선택에 효과적이며, unsupervised pretraining networks와 generative tasks(GANs)에 쓰임.
- 즉, Autoencoder는 입력에서 출력으로 복사하는 법을 배움.
  - 주로 MLP와 같은 구조를 가짐.
    - 입력과 출력이 같은 사이즈를 가짐.
    - 출력은 입력과 매우 비슷하지만 달라야 한다.
      - 잠재 정보를 사용하기 때문.
      - 중간 은닉층의 출력 결과가 잠재 정보.
  - Encoder의 출력은 Latent representation이라고 불리며, 입력의 더 낮은 차원의 표현(Lower dimensional representation)이라고 볼 수 있다.
    - 그래서 차원 축소 / 특징 선택이라고 볼 수 있다.
