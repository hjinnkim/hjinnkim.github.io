---
title: "[GNN] 05. GNN application 1"
excerpt: "Graph Recurrent Network, Graph Attention Network"

categories:
  - GNN
tags:
  - [Graph, GNN]

permalink: /courses/gnn/005/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-11-12
last_modified_at: 2022-11-12
order: 5
published: false
use_math: true

---
# Part 5 그래프 신경망 활용

## 0. Table of Contents

1. [Gated Graph Neural Networks](#1-gated-graph-neural-networks)
2. [Tree LSTM](#2-tree-lstm)
3. [Graph LSTM](#3-graph-lstm)
4. [Graph Attention Network](#4-graph-attention-network)



## 1. Gated Graph Neural Networks

- GGNN은 propagation 단계에서 Gated Recurrent Units(GRU)의 사용을 제시했다.
- 노드의 hidden state를 업데이트하기 위해, GRU-like operation에서 이전 시간의 정보 및 이웃 노드들로부터의 정보를 사용한다.
    <p>
      $
       a^t_v=A^T_v[h^{t-1}_1,\ldots,h^{t-1}_N]+b\\
       z^t_v=\sigma(W^za^t_v+U^zh^{t-1}_v)\\
       r^t_v=\sigma(W^ra^t_v+U^rh^{t-1}_v)\\
       \overset{~}{h^t_v}=tanh(Wa^t_h+U(r^t_v\odot h^{t-1}_v))\\
       h^t_v=(1-z^t_v)\odot h^{t-1}_v+z^t_v\odot \overset{~}{h^t_v}
      $
    </p>

## 2. Tree LSTM

- GGNN의 GRU와 같이, LSTM 역시 graph와 tree의 propagation method로 제시되었다.
- Tree LSTM은 tree-structured data를 위한 LSTM 모델의 일반화를 위해 소개되었다. Tree는 loop가 없는 graph의 한 분류일 뿐이다.
- LSTM에서는 시퀀스에서 노드의 히든 스테이트는 현재 노드와 이전 노드의 히든 스테이트로부터 구해진다.
- Tree는 임의의 수의 child nodes(previous nodes)들을 가지고 있으며, 스퀀스 속 노드의 히든 스테이트를 얻기 위해 현재 노드와 child node들의 히든 스테이트를 사용할 수 있다.

## 3. Graph LSTM

- 어떤 그래프에서든 사용할 수 있도록 일반화된 Tree LSTM이다.
- 핵심 문제는 일반적인 그래프는 tree와 같은 순서가 없다는 것이다. child나 parent와 같은 개념이 없는 것이다.
- 따라서, 우린 노드의 순서를 먼저 결정해야 한다.
  - BFS/DFS는 노드의 순서가 될 수 있다. 적절한 순서는 활용에 따라 다를 것이다.
  - 순서가 결정되면 노드의 히든 스테이트는 노드의 child가 아닌 모든 이웃 노드의 히든 스테이트를 활용하여 계산될 것이다.

## 4. Graph Attention Network

- Attention 메커니즘 역시 sequence-based task (machine translation)에 효과적이라는 것이 증명되어 왔다.
- GCN에서는 모든 이웃 노드들이 동등하게 취급되었다.
- Attention 메커니즘에서는 각 노드가 다르게 취급될 수 있으며, 즉, 각 이웃 노드에게 각각 다른 attention score를 적용할 수 있다는 것이다.
- 약어로는 GAT을 사용한다. (Generative Adversarial Network와의 혼동을 막기 위해)
- 2018년 GAT은 Velickovic et al.에 의해 attention 메커니즘을 GNN의 propagation step에 통합시키기 위해 제시되었다.
- self-attention 전략을 따르며, 각 노드의 히든 스테이트는 이웃 노드들에 의해 계산된다.
- GAT은 하나의 graph attention layer를 정의하며, 이 레이어를 쌓아 임의의 graph attention network를 구성한다.
- Graph attention layer는 multi-head attention을 사용하여 학습 과정을 안정화시킨다. 히든 스테이트를 계산하기 위해 K개의 독립적인 attention mechanism을 적용하며, 목표 노드의 최종 히든 스테이트를 얻기 위해 그것들을 연결하거나 평균을 취한다.
- GAT 특징:
  - 노드와 이웃쌍의 GAT의 연산은 병렬화될 수 있으며, 매우 효율적이다.
  - 각 노드에 서로 다른 가중치를 적용할 수 있다.
  - GAT은 귀납적 문제에도 적용될 수 있다.(이전에 보지 못한 문제에도 효율적으로 사용될 수 있다.)