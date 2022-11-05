---
title: "[GNN] 03. Graph Neural Networks"
excerpt: "Graph Neural Networks basics"

categories:
  - GNN
tags:
  - [Graph, GNN]

permalink: /courses/gnn/003/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-11-05
last_modified_at: 2022-11-05
order: 3
published: false
use_math: true

---
# Part 3 그래프 신경망 기초

## 0. Table of Contents

1. [Introduction and Motivation of GNNs](#1-introduction-and-motivation-of-gnns)
2. [GNN Frameworks](#2-gnn-frameworks)
3. [Spectral Graph Convolution](#3-spectral-graph-convolution)
4. [Spatial Graph Convolution](#4-spatial-graph-convolution)
5. [Graph Pooling](#5-graph-pooling)
6. [Framework](#6-framework)
7. [Graph Filters in Detail](#7-graph-filters-in-detail)

## 1. Introduction and Motivation of GNNs

- 실제 데이터는 graph-based인 경우가 많다.
- GNN은 Scarselli et al.(2009)에 의해 제시되었으며, 딥러닝을 그래프 형태의 데이터에 적용하기 위해 나왔다.
- 그래프에서는 노드는 그 자체의 특징과 다른 노드들간의 관계로 정의되어 있다.
- GNN의 주요 목표는 노드와 주변 환경의 정보를 인코딩하는 *state enbedding*을 학습하는 것이다. 
  - Node-focused tasks에서 GNN은 각 노드의 특징을 배우는 것이다.
  - Graph-focused tasks에서 GNN은 전체 그래프의 특징을 배우는 것이며, 노드의 특징을 배우는 것은 이를 위한 중간 단계에 주로 해당한다.
<p align="center">
  <img src="{{site.gdrive_url_prefix}}1WpkvRqMcxAu3yK-PT_l33TiztZeifrXw" title="2_Graph" style="float: center; width:50%">
</p>
- node 1의 state embedding, x1
  - x1은 node 1에 관련된 feature의 vector로 볼 수 있다.
  - x1에 연결된 edge 역시 node 1의 neighbor들을 알아내어 neighbor들을 고려한 x1을 추출하는데 사용될 수 있다.

## 2. GNN Frameworks

- Graph filtering opertaion
  - 노드의 feature와 그래프의 structure를 기반으로 노드의 feature를 학습하는 과정
  - 이는 주로 노드의 feature와 그래프의 structure로부터 새로운 노드의 feature(더 고차원의)를 생성하는 과정이다.
    - 따라서 그래프의 structure는 바뀌지 않지만 노드가 담고있는 feature는 이전과 다르다.
  - 이를 위한 두 가지 방식의 convolutional operation(convolution은 필터링)이 있다.
    - Spectral-based filtering
    - Spatial-based filtering
- 핵심은 노드의 embedding state를 생성하기 위해 노드 그 자체의 feature뿐 아니라 neighbors의 feature 역시 사용하는 것이다.

## 3. Spectral Graph Convolution

- Spectral graph theory를 기반으로 한다.
  - graph-based polynomial, eigenvalue/vector of the matrices associated with the graph의 특징에 대한 연구
- 기본적으로 kernel에 의한 graph signal의 곱으로 정의됨.
  - Fourier domain에서 정의됨.
- 단점
  - 전체 그래프에 대해 동시에 처리한다.(큰 그래프에서 비실용적)
  - 고정된 그래프라고 가정

## 4. Spatial Graph Convolution

- 노드의 neighbor들의 feature를 이용하여 연산한다.
  - 그래프 어디에서든 weight를 공유한다.
  - 그래프 전체보다는 subgraph의 노드들만 계산한다.
  - GraphSAGE 알고리즘이 spatial graph convolution을 위한 가장 대표적인 알고리즘이다.

## 5. Graph Pooling

- Node-focused tasks에서는 graph filtering이 충분할 수 있다.
  - 하지만 전체 그래프에 대한 정보를 추출하기 위해선 다른 방식의 연산이 필요하다.
  - Graph poolinbg opertaion
- Pooling operation은 subsampling이다.
  - CNNs와 마찬가지로 필터링과 풀링이 반복적으로 나타날 수 있다.
  - Graph filtering은 CNN의 일반화이다.(따라서 GNN의 시작은 CNN이다.)
- 기존의 그래프를 입력으로 받고 더 적은 수의 더 좋은 feature를 가진 노드들의 그래프를 얻는다.

## 6. Framework

- Node tasks(i.e. 노드의 label을 예측)를 위한 GNN을 쌓는 것을 위한 전형적인 프레임워크는 하나 이상의 graph filtering과 비선형 활성화함수를 조합하는 것이다.
  - Graph filtering과 비선형 활성화함수는 각 노드의 더 나은 feature를 추출하기 위함.
- Graph tasks를 위한 GNN을 쌓는 것은 하나 이상의 graph filtering과 비선형 활성화 함수, 그리고 grpah pooling layers를 추가한 것들을 조합하는 것이다.
  - 이 경우 graph가 결과로 나온다.
  - pooling layer의 경우 고차원 feature를 생성하기 위함(i.e. 노드의 feature를 요약)

## 7. Graph Filters in Detail

- 두 가지 종류의 graph filters
  - Spectral-based graph filters
  - Spatial-based graph filters
- Spectral-based graph filters
  - graph signal을 조절하는 것이 목표
    - graph signal을 재생성하기 전에 graph signal들을 증폭/소거
    - Graph fourier transform이 사용됨.
  - Background
    - 데이터로부터 어떤 signal을 보존할지 학습
    - 함수에 자유도를 주기 위해 parameter의 수가 그래프의 노드의 수와 같음
      - 연산 소모량이 많음
  - Poly-filter
    - 연산량 문제를 해결하기 위해 Defferrard et al (2016)에 의해 제시됨.
    - 그래프의 특정 노드들만 사용함. (k-hop neighborhoods)
    - polynomial filter의 한계 중 하나는 polynomial의 basis가 orthogonal 하지 않다는 것. 이는 학습 중 하나의 계수가 변하면 다른 계수가 변경될 수 있음을 의미. 학습 과정에서 불안정성을 야기할 수 있음. 
  - Cheby-filter
    - Chebshev polynomial을 사용.
      - orthogonal basis를 사용
      - poly-filter와 같은 장점. 더 안정.
  - GCN-filter
    - Cheby-filter는 k의 값을 1로 한정하면 k-hop neighborhood를 의미함.
    - GCN-filter는 spatial-based filter로도 볼 수 있다. (특정 노드들만 고려할 수 있으므로)
- Spatial-based graph filters
  - Scarselli et al.(2005) 제시됨.
  - 그래프 내의 특정 노드들만으로 연산함.
  - 이때 kernel g()는
    - node i의 label, node i의 associated features, node i와 1-hop neighbor인 노드들의 label을 입력으로 받는다.
    - g()는 FNN으로 모델링 될 수 있으며, 그래프의 모든 노드들의 같은 g()를 공유할 수 있다.
  - GraphSAGE-filter
    1. Sampling: 이웃으로부터 S 노드만 뽑는다.
    2. Aggregation: 이웃들의 정보를 합니다. 다양한 aggregation function이 사용 가능.
    3. Combination: 노드의 이전 feature와 aggregate한 정보를 합쳐서 노드의 새로운 feature를 생성한다.
    - combination에서 feedforward NN이 사용될 수 있다.
  - GAT-filter
    - Graph Attention Networks를 위해 설계된 spatial graph filter를 위해 self-attention mechanism이 소개되었다.
    - GCN-filter와 닮았다.
    - GCN-filter는 그래프 구조만 사용하지만 GAT-filter는 중요도에 따라 이웃에 서로 다른 가중치를 할당한다.
  - EC-filter
    - edge 자체에도 정보가 있을 수 있다.
    - EC-filter(edge-conditioned graph filter)는 edge의 정보를 사용하기 위해 설계되었다.
  - GGNN-filter
    - GRU(Gated Recurrent Units)를 사용한 graph filter를 적용함.
    - edge에 정보가 있는 Directed graph를 위해 설계되었다.