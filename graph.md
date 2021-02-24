### 목차
> 1. [그래프의 기본 개념](#그래프의-기본-개념)
>> 1) [그래프의 소개](#그래프의-소개)
>> 2) [그래프의 정의](#그래프의-정의)
>> 3) [그래프의 표현](#그래프의-표현)
>> 4) [그래프의 종류](#그래프의-종류)
> 2. [그래프의 기본 용어](#그래프의-기본-용어)
> 3. [그래프의 기본 연산](#그래프의-기본-연산)

# 그래프의 기본 개념
## 그래프의 소개
- 쾨니히스베르크(Konigsberg) 다리 문제
  - 오일러(Euler)가 그래프의 개념을 도입해서 해결
  - 한붓그리기 문제
    - 4개의 vertex
    - 각 vertex는 3개씩의 edge에 연결됨
    - 홀수 개의 edge에 연결된 vertex의 수가 4개 이상이면 한붓그리기가 불가능한 도형
    - ∴ 이 문제의 해는 존재하지 않음!

![쾨니히스베르크](https://user-images.githubusercontent.com/57928612/108972154-5fee7f80-76c6-11eb-9e80-16a3540a9bb3.png)
---
## 그래프의 정의
- 그래프
  - 개체들 사이의 일대일 관계를 시각적으로 표현하는 수학적 모델
    - A mathematical(abstracted) model that represents the one-to-one(binary) relationship between objects visually
  - 그래프는 vertex(정점)와 edge(간선)의 집합
  - G = (V, E)
    - V : vertex. 개체를 나타냄
    - E : edge. 일대일 관계를 나타냄. edge는 vertex의 쌍으로 표현.
---
## 그래프의 표현
### 그래프의 수학적 표현
- V = { 0, 1, 2, 3 }
- E = { (0, 2), (0, 3), (1, 2), (1, 3) }
### 그래프의 시각적 표현
![그래프의 시각적 표현](https://user-images.githubusercontent.com/57928612/108972973-3550f680-76c7-11eb-81b3-59d0ef136c12.png)
### Edge 리스트를 이용한 표현
```
4, 4
0, 2
0, 3
1, 2
1, 3
```
---
## 그래프의 종류
### 방향과 가중치에 따른 그래프의 종류
1. 무방향 그래프(graph)
2. 방향 그래프(directed graph)
3. 가중치 그래프(weighted graph)
4. 방향 가중치 그래프(weighted directed graph)

![방향과 가중치에 따른 그래프의 종류](https://user-images.githubusercontent.com/57928612/108973430-abedf400-76c7-11eb-8bfd-aa05a0bc1f6f.png)

### 특별한 그래프
#### Self edge를 가진 그래프
- self edge
  - 나와 나를 연결한 edge
  - (ex) (A, A)
#### Multi edge를 가진 그래프
- multi edge
  - 두 vertex 사이에 edge가 2개 이상 있는 경우
  - (ex) (A, B), (A, B)

### 복잡도에 따른 그래프의 종류
![복잡도에 따른 그래프의 종류](https://user-images.githubusercontent.com/57928612/108974462-d55b4f80-76c8-11eb-883c-fbbac4761396.png)
#### 완전 그래프(complete graph)
- n개의 vertex들이 서로 연결된 그래프(모든 vertex 사이에는 edge가 존재)
- 하나의 vertex가 n-1개의 vertex와 연결됨
- edge의 수 : n(n-1)/2
- 방향 그래프의 경우는 n(n-1)개의 edge를 가짐

![완전그래프](https://user-images.githubusercontent.com/57928612/108975115-7ba75500-76c9-11eb-91fa-ac3e2c394026.png)
#### 밀집 그래프(dense graph)
- n개의 vertex들의 대부분이 서로 연결된 그래프
- edge의 수 : O(n<sup>2</sup>)
- 방향 그래프의 경우에도 O(n<sup>2</sup>)개의 edge를 가짐

![밀집그래프](https://user-images.githubusercontent.com/57928612/108975545-f2dce900-76c9-11eb-8356-d5449238b8f2.png)
#### 희소 그래프(sparse graph)
- 각 vertex들이 상수 개의 vertex와 연결된 그래프
- 한 vertex에서 연결된 vertex의 수 : O(1)
- edge의 수 : O(n)

![희소그래프](https://user-images.githubusercontent.com/57928612/108975786-33d4fd80-76ca-11eb-9559-0d1d4112e037.png)

# 그래프의 기본 용어
## vertex
- 하나의 개체를 표현
- 원 내부에 숫자나 문자로 정체성을 나타냄
## edge
- vertex 사이의 1:1 관계를 나타냄
- vertex의 쌍으로 표현
  - (ex) (0, 1)
- 무방향 그래프의 경우에는 (0, 1)과 (1, 0)이 같으나, 방향 그래프에서는 방향성을 띠기 때문에 다르다.
## adjacent(인접) & incident
- vertex u와 v가 edge로 연결되어 있다면,
  - (u, v) ∈ E
  - u와 v는 인접(adjacent)해 있다.
- 두 edge가 같은 vertex를 공유하고 있다면,
  - 두 edge는 incident
  - 헷갈리지 말아야 하는게 multi edge라는게 아니다. 그저 같은 vertex를 공유하고 있는 것이다. 즉, multi edge ∈ incident edge이다.
## 부분 그래프(subgraph)
- G = (V, E)이고 G' = (V', E')일 때, V'⊆V 이고 E'⊆E이면, G'는 G의 부분 그래프(subgraph)이다.

![부분그래프](https://user-images.githubusercontent.com/57928612/108977569-04bf8b80-76cc-11eb-8057-b45e7f8ef851.png)
## 경로(path)
- vertex u에서 v로 가는 경로는 다음을 만족하는 vertex들의 연속체이다.
- (u, i<sub>1</sub>), (i<sub>1</sub>, i<sub>2</sub>), ..., (i<sub>k</sub>, v) ∈ E
  - path (u, v) : (u, i<sub>1</sub>, i<sub>2</sub>, ..., i<sub>k</sub>, v)

![경로1](https://user-images.githubusercontent.com/57928612/108978255-c37bab80-76cc-11eb-937b-a5b2382a5875.png)

- 여러 개의 경로가 존재할 수 있음

![경로2](https://user-images.githubusercontent.com/57928612/108978123-a0e99280-76cc-11eb-9017-672bf9f23ad3.png)

- vertex u에서 v로 가는 경로는 다음을 만족하는 edge들의 연속체로 정의할 수 있음
- (u, i<sub>1</sub>), (i<sub>1</sub>, i<sub>2</sub>), ..., (i<sub>k</sub>, v) ∈ E
  - path (u, v) : ((u, i<sub>1</sub>), (i<sub>1</sub>, i<sub>2</sub>), ..., (i<sub>k</sub>, v))

![경로3](https://user-images.githubusercontent.com/57928612/108978619-1ead9e00-76cd-11eb-9948-34ca4644cbd5.png)

### 경로의 길이(length of a path)
  - 경로에 있는 vertex 또는 edge의 수
  - (ex) Path (1, 7)의 길이 : 5(vertex의 수) 또는 4(edge의 수)
  
  ![경로4](https://user-images.githubusercontent.com/57928612/108978885-6cc2a180-76cd-11eb-84a8-83a78963a100.png)
  
  - 가중치 그래프(weighted graph)의 경우
    - 경로에 있는 edge의 weight의 합
    - (ex) Path (1, 7) : ((1, 0), (0, 2), (2, 6), (6, 7)) -> Path (1, 7)의 길이 : 5 + 3 + 4 + 3 = 15
    
    ![경로5](https://user-images.githubusercontent.com/57928612/108979248-cdea7500-76cd-11eb-84c8-745f4d25c66b.png)

### 최단 경로(shortest path)
- 여러 경로 중에서 길이가 최소인 경로

![경로6](https://user-images.githubusercontent.com/57928612/108979532-19048800-76ce-11eb-9151-b2131702c5dd.png)

### 단순 경로(simple path)
- 시작 vertex와 끝 vertex를 제외한 모든 vertex가 서로 다른 경로

![경로7](https://user-images.githubusercontent.com/57928612/108979715-47826300-76ce-11eb-9bd4-7feb498a8730.png)

## 순환(cycle)
- 첫 번째 vertex와 마지막 vertex가 일치하는 단순 경로

![순환](https://user-images.githubusercontent.com/57928612/108979911-7a2c5b80-76ce-11eb-8466-21901e2aefdb.png)

### 비순환 그래프(acyclic graph)
- 순환이 존재하지 않는 그래프

![비순환그래프](https://user-images.githubusercontent.com/57928612/108980062-a34cec00-76ce-11eb-88f1-0e472e3ad670.png)

## 연결(connected)
- 두 vertex u와 v 사이에 경로가 존재할 때 이 두 vertex는 연결(connected)되었다고 함

![연결1](https://user-images.githubusercontent.com/57928612/108980292-db542f00-76ce-11eb-90f0-165620c37278.png)

- 어떤 그래프에 속한 모든 쌍의 vertex들이 연결되어 있을 때, 이 그래프는 연결(connected)되었다고 함

![연결2](https://user-images.githubusercontent.com/57928612/108980462-08a0dd00-76cf-11eb-9278-866c00e66945.png)

- 트리는 연결된 비순환 그래프(connected acyclic graph)

![연결4](https://user-images.githubusercontent.com/57928612/108980842-777e3600-76cf-11eb-9a36-f1e02bbd60a6.png)

### 연결 성분(connected component)
- 연결된 부분 그래프의 최대치(A maximal connected subgraph)

![연결3](https://user-images.githubusercontent.com/57928612/108980669-43a31080-76cf-11eb-85c1-fadcd86a86a4.png)

### 강한 연결(strongly connected)
- 방향 그래프에서는 경로에 방향성이 추가되기 때문에 강한 연결(strongly connected)이라고 함

![연결5](https://user-images.githubusercontent.com/57928612/108981011-a5fc1100-76cf-11eb-97f6-596817c142af.png)

### 강한 연결 성분(strongly connected component)
- 방향 그래프에서 강하게 연결된 부분 그래프의 최대치

![연결6](https://user-images.githubusercontent.com/57928612/108981319-f70c0500-76cf-11eb-9296-dc4d9cbb388e.png)

## vertex의 차수(degree of vertex)
- vertex에 연결된 edge의 수
- (ex) vertex 7의 degree는 4

![차수](https://user-images.githubusercontent.com/57928612/108981515-26227680-76d0-11eb-957c-f9ce219b54a8.png)

## 신장 트리(spanning tree)
- 그래프 G = (V, E)의 부분 그래프 T = (V', E')가 다음의 조건을 만족시킬 때, T를 G의 신장 트리라고 함
  - V' = V and E' ⊆ E and |E'| = |V| - 1
  - E'는 순환이 없음
  - 여러 개의 신장 트리가 존재함

![신장트리](https://user-images.githubusercontent.com/57928612/108982096-c2e51400-76d0-11eb-9912-82f8d8168f4b.png)

# 그래프의 구현

