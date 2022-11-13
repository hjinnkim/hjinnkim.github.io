---
title: "[GNN] 04. Graph Node Embedding Methods"
excerpt: "Graph Node Embedding"

categories:
  - GNN
tags:
  - [Graph, GNN]

permalink: /courses/gnn/004/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-11-12
last_modified_at: 2022-11-12
order: 4
published: false
use_math: true

---
# Part 4 그래프 임베딩

## 0. Table of Contents

1. [Graph Node Embedding Methods](#1-graph-node-embedding-methods)
2. [Graph Node Embedding Process](#2-graph-node-embedding-process)
3. [Graph Node Embedding Example](#3-graph-node-embedding-example)
4. [General process of Graph Node Embedding](#4-general-process-of-graph-node-embedding)
5. [General Framework of Graph Node Embedding](#5-general-framework-of-graph-node-embedding)
6. [Transductive vs. Inductive](#6-transductive-vs-inductive)
7. [DeepWalk Algorithm](#7-deepwalk-algorithm)
8. [Node2Vec Algorithm](#8-node2vec-algorithm)
9. [Node2Vec/DeepWalk Pros and Cons](#9-node2vecdeepwalk-pros-and-cons)
10. [GraphSAGE Algorithm](#10-graphsage-algorithm)

## 1. Graph Node Embedding Methods

- 딥러닝 혹은 머신러닝에서 학습할 수 있는 임베딩의 집합으로 그래프의 노드를 변환하는 방법
- 우리는 그래프를 이런 임베딩들로 변환하는 방법을 배워야 한다
- 주요한 세가지 방법:
  1. DeepWalk
  2. Node2Vec
  3. GraphSAGE

## 2. Graph Node Embedding Process

- General Idea: 그래프의 모든 단일 노드들을 저차원의 벡터로 나타내는 것
- 이러한 노드 임베딩은 기존 그래프 속 노드들의 중요 정보(Key information)을 보존해주어야 한다.
  - 만약 그래프 속에서 노드들의 spatial information이 비슷하다면 임베딩 공간에서도 해당 노드들은 비슷해야 한다.
- 두 가지 중요한 질문:
  1. 어떤 정보를 보존해야 하는가?
  2. 어떻게 그 정보를 보존할 것인가?
- 그래프 노드 임베딩 알고리즘들은 이 질문에 여러 방식으로 답을 제시한다.

## 3. Graph Node Embedding Example

  <p align="center">
    <img src="{{site.gdrive_url_prefix}}1jCzplBb14pvvFqoVhKEzGZmYURDGT1XP" title="4_CNN" style="float: center; width:80%">
    <font size="2">Image reference: <a href="https://arxiv.org/abs/1709.05584">Image reference: William L. Hamilton, Rex Ying, Jure Leskovec: Representation Learning on Graphs: Methods and Applications//</a></font>
  </p>

- Left: 가라테 클럽 소셜 네트워크 그래프 표현(선으로 이어진 것은 친구, 서로 다른 색상은 서로 다른 커뮤니티를 나타냄)
- Right: 노드들의 2차원 임베딩 표현(유사성 포착)
-  두 노드의 동시 발생을 포착하는 DeepWalk 사용 $\rightarrow$ 그래프의 특수 표현에서 노드의 저차원 임베딩으로 변환할 때 찾고 있는 정보를 보존
- 문제 : 매핑은 어떻게 하는가? 임베딩을 얻는 가장 좋은 방법은 어떻게 학습하는가?

## 4. General process of Graph Node Embedding

- 임베딩 차원의 노드 정보를 사용하여 보존하고 싶은 그래프 차원의 정보를 재구성
- 따라서 우리는 보존하고 싶은 그래프로부터의 정보를 재구성하게 해주는 "node embedding representation" 방법을 원한다
- General Idea: 재구성 과정에서 오류를 최소화하는 매핑 함수를 찾는 것
  - (+그래프의 크기가 임베딩하는 속도에 주는 영향이 작아야 한다.)

## 5. General Framework of Graph Node Embedding

- 4가지 주요 구성 요소
  1. The mapping function: 실제로 배우고자 하는 함수이며, 그래프의 노드를 임베딩 공간에 매핑한다.
    - "encoding function"이라고 불린다
  2. The information extractor: 그래프에서 보존하고자 하는 key information을 추출한다
    - "similarity graph function"이라고 불린다
  3. The information reconstructor: mapping function에서 얻어진 노드 임베딩들로부터 key information을 재구성한다
    - "similarity embedding funcion"이라고 불린다
  4. The objective: 추출한 key information과 재구성한 key information을 기반으로 objective를 정의한다. mapping/reconstruction에 관여하는 패러미터들은 objective를 최적화하여 학습한다.

## 6. Transductive vs. Inductive

- Transductive methods: 보이지 않는 데이터(현재 존재하지 않는 데이터)에서 잘 작동하지 않는다.
  - 만약 그래프에 새로운 노드가 추가된다면, 새로운 노드로부터 노드 임베딩을 얻기 위해 모델을 새로 훈련시켜야 한다.
  - DeepWalk, Node2Vec 이 있음.
- Inductive methods: 보이지 않는 데이터에도 쉽게 사용될 수 있음.
  - 일반적으로 이웃 노드의 정보를 이용하여 새로운 노드의 임베딩을 얻기 때문
  - GraphSAGE

## 7. DeepWalk Algorithm

- 2014년 Perozzi et al.이 제시함.
- 그래프 속 비슷한 노드를 보존하는 것에 초점을 맞춤.
- 그래프의 Walk의 개념을 사용.
- "random walk"에서 동시에 발생하는 노드들을 비슷하다고 고려함.
- mapping function은 학습된 노드 임베딩들이 walks에서 추출한 유사도를 재구성할 수 있도록 최적화 됨.
- 그래프 노드 임베딩에서 보통 가장 먼저 배우는 일반적인 알고리즘.
- 과정
  - 입력: 노드나 엣지의 정보가 없는 그래프를 받는다.
  - Mapping function: 노드의 임베딩은 $v_i$일 때, index i에서 얻어지는 [look up table](https://wikidocs.net/64779)
    - $f(v_i)=u_i=e^T_iW$, where $e[i]=1$ and rest of positions are 0 and $W$ are the embedding parameters we want to learn
  - Information Extraction
    1. Random walks
    2. Extracting co-occurrence
- Random walk
  - 그래프의 특정 노드로부터 랜덤워크를 시행했을 때, walk가
    1. 5-3-8-7
    2. 4-6-8-7
  - 이때, 8과 7이 동시 발생
  - 이 동시 발생에 집중하는 것이 DeepWalk
- DeepWalk Algorithm
  1. Generating Random walks
      <p>
        $
        \text{Input: G={V, E}, T, alpha}\\\quad {\scriptsize \text{(T : length of random walk, alpha : # of random walks we want to generate)}} \\
        \text{Output: R}\\
        \text{Initialization: R}\leftarrow\emptyset\\
        \text{for v in V do}\ {\scriptsize\text{# for all nodes in graph}} \\
        \quad\text{for I in range(1, alpha) do}\ {\scriptsize {\scriptsize\text{# generate alpha random walks}}}\\
        \qquad\text{W}\leftarrow\text{RW(G,v,T)}\ {\scriptsize \text{# generate a random walk W starting at node v of length T}} \\
        \qquad\text{R}\leftarrow R\cup \text{W}\ {\scriptsize \text{# store the random walk just generated}}\\
        \quad \text{end}\\
        \text{end}
        $
      </p>
  2. Extracting co-occurrence
      <p>
        $
        \begin{array}[l]
          \text{Input: R, d}\hfill\\
          \text{Output: I}\hfill\\
          \text{Initialization: I}\leftarrow \text{[ ]}\hfill\\
          \text{for W in R do}\hfill\\
          \quad\text{for v[i] in W do}\hfill\\
          \qquad\text{for j in range(1, d) do}\hfill\\
          \quad\qquad\text{I.append({v[i-j], v[i]})}{\scriptsize \text{# add the co-occurrence of node i and its next i-j node}}\hfill\\
          \quad\qquad\text{I.append({v[i+j], v[i]})}{\scriptsize \text{# add the co-occurrence of node i and its next i+j node}}\hfill\\
          \quad\qquad {\small\text{(v[i] is center node, v[i-j], v[i+j] are context nodes)}}\hfill\\
          \qquad\text{end}\hfill\\
          \quad\text{end}\hfill\\
          \text{end}\hfill
        \end{array}
        $
      </p>
  - 지금까지 mapping function(the lookup table)과 information extractor(from random walks)를 가지고 있다.
  - 노드 임베딩에서 co-occurrence를 재구성하기 위해, $\text{I}$ 속 tuples들을 관측할 확률을 다룬다.
  3. Reconstructor function:
      <p>
      <ul>
        <li>$\text{z_i, z_j: embeddings of v_i and v_j nodes}$</li>
        <li>각 tuples $(v_j,\ v_i)$에서 각 노드들은 두 가지 역할을 가질 수 있다: context, center(depending on the tuple</li>
        <li>$v_i$의 context로서 $v_j$를 관측할 확률은 softmax 함수로 모델링할 수 있다</li>
        <ul>$$rec(v_i,v_j)=p(v_j\vert v_i)=\frac{\exp{z^T_iz_j}}{\Sigma_{v\in V}\exp{z^T_iz_v}}\\\text{where }z_i=e^T_iW,\ \text{and } z_j=e^T_jW$$</ul>
        <li>만약 reconstruction function이 잘 작동한다면, $\text{I(co-occurrence list)}$에 있는 tuple들은 높은 확률을 반환할 것이고, 랜덤하게 생성된 튜플들($\text{I}$에 있지 않은 튜플들)은 낮은 확률을 반환할 것이다.</li>
        <li>original co-occurrence information을 재구성할 확률을 다음과 같이 계산된다.</li>
        <ul>$$I'=rec(I)=\underset{v_i,v_j\in set(I)}{\Pi}p(v_j\vert v_i)^{\text{# of }(v_j,v_i)}$$</ul>
        <li>따라서, 우린 다음의 objective function을 최소화한다.</li>
        <ul>$$L(W)=-\underset{(v_i,v_j)\in set(I)}{\huge{\Sigma}}{\small \text{# of }(v_j, v_i)*\log p(v_j\vert v_i)}$$</ul>
      </ul>
      </p> 
## 8. Node2Vec Algorithm

- Node2Vec은 random walk 생성 방식을 변형하여 노드의 neighborhood를 탐색하기 위한 더 유연한 방식이다.
- 나머지는 DeepWalk와 동일하다.
- Node2Vec Random Walk 과정
  - DeepWalk에서는 walk에서 다으 ㅁ노드를 방문할 확률이 uniform distribution을 따른다.
  - Node2Vec에서는 새로운 두 패러미터 p, q가 도입된다.
  - p는 현재 노드로부터 이전에 방문했던 노드를 재탐색할 확률을 제어한다. 작은 p는 이전에 방문했던 노드를 방문할 확률을 높게 한다.
  - q는 현재 노드로부터 가까운 노드를 방문할 확률을 제어한다. q가 크면 (q>1) 가까운 노드를 탐색할 확률이, q가 작으면 (q<1) 멀리 있는 노드를 탐색할 확률이 높다.
    - q<1: DFS, q>1: BFS
  - p와 q를 잘 선택하여 local structure와 global structure 모두를 잘 포착하는 것이 관건이다.
  - 다음 노드를 선택하기 위한 Node2Vec의 비정규화 확률
      <p>
        $$
          p_{pq}(v^{t+1}\vert v^{t-1},v^t)=\begin{cases}
          \frac{1}{p} & \text{if dis}(v^{t-1},v^{t+1})=0 \\
          1 & \text{if dis}(v^{t-1},v^{t+1})=1 \\
          \frac{1}{q} & \text{if dis}(v^{t-1},v^{t+1})=2 \\
          \end{cases}\\
          {\scriptsize \text{where dis}(v^{t-1},v^{t+1})\ \text{measures the shortest path between these two nodes}}
        $$
        
      </p>

## 9. Node2Vec/DeepWalk Pros and Cons

- 장점
  - random walk의 생성을 병렬화하여 확장 가능하다.
  - Node2Vec의 경우 local structure와 global structure 모두를 포착할 수 있다.
- 단점
  - 노드나 엣지의 정보를 사용할 수 없다.
  - 전제 그래프에 대해 알고리즘을 돌리는 것이 아니면 새로 추가된 노드에게는 임베딩을 부여할 수 없다.

## 10. GraphSAGE Algorithm

- GraphSAGE: Inductive Representation Learning on Large Graphs
- DeepWalk와 Node2Vec의 한계를 극복하기 위한 시도
  - 귀납적. 고려되지 않은 새로운 노드의 임베딩도 생성 가능.
  - 새로 추가된 노드의 표현을 생성하기 위해 노드의 feature를 사용함.
- GraphSAGE의 핵심 아이디어는 타겟 노드의 임베딩을 생성하기 위해 그래프 전체가 아닌 이웃 노드들의 샘플 집합을 사용한다는 것에 있음.
- GraphSAGE는 3단계로 이루어져 있음.
  - Sampling
  - Aggregation(집계)
  - Activation function
- GraphSAGE는 Graph Filter로 이해될 수 있음.
- GraphSAGE Algorithm
    <p>
      $
        h^0_v=x_v\\
        h^k_v=\sigma(w_k*f_{agg}(h^{k-1}_u,u\in N(v)),B_kh^{k-1}_v),\quad \text{k layer}\\
        z_v=h^k_v\quad \text{final layer}
      $  
    </p>
- GraphSAGE의 원 논문에서는 3가지의 집계함수를 제안했다.
  - Mean aggregator:
    - 노드 벡터의 평균
    - GCN filter와 매우 흡사
  - LSTM aggregator:
    - 고비용의 용량(capacity)를 요구
    - aggregation은 순서에 따른 영향이 없어야 하기 때문에, 이웃 노드들은 순열변환되어야 한다. (permuted)
  - Pooling aggregator:
    - 학습 가능한 fully-connected NN
    - 결과를 max-pool
- GraphSAGE 특징:
  - 그래프의 특정 부분만을 사용하여 목표 노드의 임베딩을 생성하기 때문에 확장이 용이하다.
  - aggregation method가 parameter를 공유하기 때문에 그래프에 없던 노드의 임베딩이 생성 가능하다.
  - 목적에 따라 다른 aggregation 기능을 사용할 수 있다.