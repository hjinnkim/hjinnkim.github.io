---
title: "[GNN] 02. Graph Basics"
excerpt: "Graph Basics"

categories:
  - GNN
tags:
  - [Graph, GNN]

permalink: /courses/gnn/002/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-11-05
last_modified_at: 2022-11-05
order: 2
published: false
use_math: true

---
# Part 2 그래프의 기초

## 0. Table of Contents

1. [Why Graph?](#1-why-graph)
2. [Graph Representation](#2-graph-representation)
3. [Properties and Measures of Graph](#3-properties-and-measures-of-graph)
4. [Spectral Graph Theory](#4-spectral-graph-theory)
5. [Graph Signal Processing](#5-graph-signal-processing)

## 1. Why Graph?

- 그래프(Graph)는 실제 데이터를 표현하는데 가장 필수적인 방법.
- 실제로 많이 쓰이기 때문에 여러 분야에서 연구됨.

## 2. Graph Representation

  <p align="center">
    <img src="{{site.gdrive_url_prefix}}1WpkvRqMcxAu3yK-PT_l33TiztZeifrXw" title="2_Graph" style="float: center; width:50%">
  </p>

- 그래프는 다음과 같이 표현된다.
  - $G=\{V, E\}$ where $V=\{v_1,\ldots, v_N\}$ is a set of $N=\vert V\vert$ nodes and $E=\{e_1,\ldots,e_M\}$ is a set of $M=\vert E\vert$ edges.
  - 그래프에서 노드는 일종의 단위이며, 사회과학에서는 사람, 화학에서는 원자가 될 수 있다.
  - 그래프의 크기는 노드의 수로 정의된다.
  - 엣지 1은 노드0과 노드2를 연결하며, $(v_0, v_2)$로 나타낼 수 있다.
  - 엣지가 방향성을 가지는지 여부에 따라 Indirect(ed) graph와 Direct(ed) graph로 나뉜다.
    - Indirect(ed) graph: $(v_0, v_2)=(v_2, v_0)$
    - Direct(ed) graph: $(v_0, v_2)\ne (v_2, v_0)$
- Node $v_{ei}^0$ 와 $v_{ei}^2$는 엣지 $e_i$에 $incident$ 하다고 부른다.
- 또한, 노드 $v_i$와 $v_j$ 사이에 엣지가 있을 경우에만 $v_i$가 $v_j$에 $adjacent$ 하다고 한다.
- 그래프는 $Adjacency\ Matrix$로 나타낼 수 있는데, 이는 그래프의 노드들 간의 연결성을 나타낸다.
  <p align="center">
    <div>
      $$
      \begin{bmatrix}
        0 & 1 & 1 & 1 \\
        1 & 0 & 0 & 1 \\
        1 & 0 & 0 & 0 \\
        1 & 1 & 0 & 0 \\
      \end{bmatrix}
      $$
    </div>
  </p>
  Adjacency Matrix의 $i-th$ row/column은 그래프의 $i-th$ 노드를 나타내고, 두 노드 사이에 엣지가 존재하면 1, 존재하지 않으면 0으로 나타낸다.

## 3. Properties and Measures of Graph

- Properties
  - Cardinality: 그래프의 노드의 수 ($\vert G\vert$)
  - Degree: 노드의 degree는 인접한(adjacent) 노드의개수를 나타낸다.
  - Neighbors: 한 노드의 인접한 모든 노드를 Neighbors라고 한다.
  - Degree of a graph(total degree): 모든 노드의 degree의 합.
- Definitions
  - Walk: 그래프의 walk는 노드와 엣지들이 번갈아 나타나는 일련의 순서이며, 노드로 시작해서 노드로 끝나며, 각 엣지들은 앞 뒤의 노드들에 incident하다.
    - Trail: 엣지들이 distinct한 walk
    - Path: 노드들이 distinct한 walk
  - Subgraph: 그래프 $G=\{V, E\}$의 서브그래프 $G'=\{V', E'\}$는 V의 부분집합 V'와 E의 부분집합 E'로 이루어져 있으며, ($V'\subset V,\ E'\subset E$), V'는 E'의 엣지에 포함되는 모든 노드들을 포함하고 있어야 한다.
  - Connected component: 그래프 $G=\{V, E\}$가 주어졌을 때, 서브그래프 $G'=\{V', E'\}$의 임의의 두 쌍의 노드 사이에 적어도 하나의 path가 존재하고, $V'$의 노드들은 $V \vert V'$에 속하는 노드들과 인접하지 않을 때, $G'$는 connected component 라고 불린다.
  - Connected graph: 그래프 $G=\{V, E\}$가 오직 하나의 connected component를 가질 때, connceted graph라고 불린다.
  - Shortest path: $p_{st}^{sp}=arg \underset{p\in P_{st}}{\min}\vert p\vert $
  - Diameter: $diameter(G)=\underset{vx,vt\in V}{\max}\underset{p\in P_{st}}{\min}\vert p\vert \rightarrow$ shortest path 중 가장 path의 길이.
    (예시 틀린 듯 -> diameter는 3이 맞음)
  - Centrality concept: 노드의 centrality는 그래프에서 그 노드가 얼마나 중요한지를 나타낸다. centrality를 측정하는 방식에는 여러 방식이 있다.
    - Degree centrality: 많은 노드에 연결되어 있으면 그 노드는 중요하다.
    - Eigenvector centrality: neighbor들의 중요도로 따진다.
    - Katz centrality: central node에게도 상수를 추가한다. 상수가 0이면 Eigenvector centraility와 같다.
    - Between centrality: 얼마나 많은 path가 해당 노드를 지난지에 따라 중요도를 나타낸다. (공간적인 위치)

## 4. Spectral Graph Theory

- Spectral Graph Theory는 그래프의 Laplacian matrix의 Eigenvalue/vector를 분석하여 그래프의 특성을 연구하는 것이다.
- Laplacian matrix: 그래프 $G=\{V, E\}$가 주어지고, $A$가 $G$의 Adjacency matrix일 때, Laplacian matrix, $L$은 다음과 같이 정의된다.
  - $L=D-A$
  - $D$는 Diagonal degree matrix로 대각행에는 각 노드의 degree가 나머지에는 0이 있는 matrix이다. 

## 5. Graph Signal Processing

- 그래프에서 구조적 정보를 배제하고, 각 노드에 중요한 정보가 담겨있다고 가정하자. 그래프의 구조와 정보의 조합은 *graph signal* 로 볼 수 있다.
- 우린 graph signal을 spatial domain과 spectral domain으로 나눌 수 있다.
  - 두 도메인 사이의 transformation은 *Graph Fourier Transformation*으로 가능하다. (classical fourier transformation을 기반으로 함)
  - Graph Fourier Transformation은 그래프의 convolution을 가능하게 함. 따라서, 그래프에 CNNs을 적용할 수 있음.
  - 참고: [Graph Fourier Transform](https://harryjo97.github.io/theory/Graph-Fourier-Transform/), [Quadratic form 유도](https://math.stackexchange.com/questions/4525740/question-about-graph-laplacian-quadratic-form)
- Complex Graphs
  - Heterogeneous graphs: 하나의 그래프 안에 여러 종류(type)의 nodes 그리고/혹은 edges가 존재할 경우
  - Bipartite graphs: 하나의 노드로 그래프를 나눌 경우, 두 개의 연결되지 않은 노드들의 부분집합으로 나눌 수 있을 경우
  - Multidimensional graphs: 그래프의 두 노드 사이에 하나 이상의 관계가 존재할 경우
  - Signed graphs: edge가 +/- 정보를 담고 있는 경우.
    - 소셜 미디어에서 follow/unfollow를 나타낼 수 있다.
  - Hypergraphs: 한 노드가 하나 이상의 노드들과 연결되어 있는 경우
  - Dynamic graphs: 시간에 따라 바뀌는 그래프
    - 노드와 엣지는 timestamp를 가지고 있다.
  - 그래프로 할 수 있는 것
    - Node 중심의 작업:
      - Node classification
      - Link prediction
    - Graph 중심의 작업:
      - Graph classification
  