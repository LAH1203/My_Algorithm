### 목차
> 1. [트리(Tree)](#트리tree)
>> 1) [트리의 정의](#트리의-정의)
>> 2) [트리의 개념](#트리의-개념)
>> 3) [트리의 용어](#트리의-용어)
>> 4) [트리의 자료구조](#트리의-자료구조)
>> 5) [트리의 구조](#트리의-구조)
> 2. [이진 트리(Binary tree)](#이진-트리binary-tree)
>> 1) [이진 트리의 정의](#이진-트리의-정의)
>> 2) [이진 트리의 성질](#이진-트리의-성질)
>> 3) [특별한 이진 트리](#특별한-이진-트리)
> 3. [트리 탐색(Tree Search)](#트리-탐색tree-search)
>> 1) [탐색이란?](#탐색이란)
>> 2) [중위 우선 탐색(Inorder traversal)](#중위-우선-탐색inorder-traversal)
>> 3) [전위 우선 탐색(Preorder traversal)](#전위-우선-탐색preorder-traversal)
>> 4) [후위 우선 탐색(Postorder traversal)](#후위-우선-탐색postorder-traversal)

## 트리(Tree)
### 트리의 정의
- 루트(root)라고 불리우는 특별한 노드가 있음
- 연결된 모든 노드의 쌍은 재귀적으로 부모 자식 관계(parent-child relation)을 맺고 있음
- 순환하는 경로(cycle path)가 없음

### 트리의 개념
- 계층적인 구조를 표현
  - 디렉토리와 서브디렉토리 구조
  - (ex) 조직도, 가계도
- 트리는 노드(node)와 노드를 연결하는 링크(link)로 구성됨

### 트리의 용어
![트리의 용어 예시](https://user-images.githubusercontent.com/57928612/106420046-264aa000-649d-11eb-8d6b-f4aed392d977.png)
- node(or vertex)
  - 트리에서 자료를 보관하는 곳
  - 정점(vertex) 또는 노드(node)라고 부름
  - 언제나 트리의 정점은 간선 개수보다 1개 많음
  - 위 트리의 N(V) = 13
- edge(or link)
  - 정점에서 다른 정점으로 가는 경로
  - 간선(edge) 또는 링크(link, 다른 노드의 위치 정보), branch라고 부름
  - 위 트리의 N(E) = 12
  #### N(V) = N(E) + 1 ( N(V)=정점의 개수, N(E)=간선의 개수 )
- size(크기)
  - 보통 자기 자신을 포함한 모든 자손 노드의 개수를 노드의 크기라 함
  - 위 트리에서 B 노드의 크기 = 5
- degree(차수)
  - degree of a node
    - 한 노드가 갖는 자식 노드의 개수이며, 노드의 차수라고도 부름
    - 위 트리에서 A 노드의 degree = 3, B 노드의 degree = 2
  - degree of a tree
    - 한 트리에 속한 노드들의 최대 degree
    - 위 트리의 degree = 3 (because of A, D nodes)
    - Binary tree -> degree = 2
    - Ternary tree -> degree = 3
    - k-ary tree -> degree = k
  - 부(하위) 트리 개수 / 간선수(degree) = 각 노드가 가진 edge 수
  - 차수가 0인(자식이 없는) 노드를 leaf node라고 부름
- depth(깊이)
  - depth of a node(노드의 깊이)
    - depth of a root = 0 or 1
      - 각 사람이나 책에 따라 0 또는 1이라고 함
      - 문제를 풀 경우에는 루트 노드의 깊이가 무엇으로 주어지는지를 잘 보고 이에 따라 코드를 달리 써야 함!
  - depth(height) of a tree(트리의 높이)
    - 가장 깊이가 깊은 leaf node의 깊이가 곧 트리의 높이가 된다
    - 위 트리에서 루트 노드의 깊이를 1로 둔다면, 트리의 높이는 4가 됨
  - 루트에서 어떤 노드에 도달하기 위해 거쳐야 하는 간선의 수
- root node
  - 부모가 없는 노드
  - 다른 모든 노드의 조상 노드
  - 모든 트리는 하나의 루트 노드를 가짐
  - 위 트리에서의 root node = A
- leaf node
  - 자식이 없는 노드
  - 모든 트리의 가장 끝에 위치함
  - 위 트리에서의 leaf node = K, L, F, G, M, I, J
- internal node(내부 노드)
  - 내부 노드의 정의는 두 가지로 나뉜다.
    1) **leaf node가 아닌 모든 노드**
    2) leaf + root가 아닌 모든 노드
    ##### 보통 첫 번째 정의를 internal node의 정의로 생각하는 경우가 많으므로 첫 번째 정의를 internal node라 정의한다. 땅땅!
- parent node(부모 노드)
  - 자기 자신의 바로 위에 연결되어 있는 노드
- child node(자식 노드)
  - 부모 노드와 반대로, 자기 자신에게서 바로 아래에 연결된 노드
- sibling node(형제 노드)
  - 같은 부모를 가지는 노드
- ancestor node(조상 노드)
  - 부모뿐만 아니라 위로 자신과 연결된 노드
  - 루트에서부터 자기 자신에게 오기 위한 경로에 있는 모든 노드
- descendent node(후손 노드)
  - 조상 노드와 반대로, 자식뿐만 아니라 아래로 자신과 연결된 노드
  - 자기 자신으로부터 아래로 갈 수 있는 모든 노드
- subtree(부분 트리)
  - 루트가 아닌 다른 노드를 루트로 하는 트리(트리 안의 작은 트리랄까?)

### 트리의 자료구조
- 노드
  - 저장할 데이터
  - 자식 노드의 수
  - 자식 노드에 대한 포인터
```cpp
// 포인터 기반의 자료구조
typedef class node* nptr;
class node {
  datatype data;
  int n_childs;
  nptr* child;
};
```
![트리의 자료구조](https://user-images.githubusercontent.com/57928612/106420774-acb3b180-649e-11eb-9545-2b4a7de01fea.png)

### 트리의 구조
![트리의 구조](https://user-images.githubusercontent.com/57928612/106420984-28adf980-649f-11eb-8ffe-c840405e1819.png)


## 이진 트리(Binary tree)
### 이진 트리의 정의
- degree가 2인 트리
- 최대 노드의 수는 2개 (O)
- 모든 노드의 수가 2개 (X)
```cpp
typedef class node* nptr;
class node {
  datatype data;
  nptr lchild, rchild;
};
```

### 이진 트리의 성질
- i번째 레벨의 최대 노드의 수는 2^(i-1)개
  ![이진 트리의 성질-1](https://user-images.githubusercontent.com/57928612/106423247-6ca2fd80-64a3-11eb-8f72-668f688d35b3.png)
  ![이진 트리의 성질-2](https://user-images.githubusercontent.com/57928612/106423395-b7247a00-64a3-11eb-8566-60f99ffdcefc.png)
- Depth가 k인 이진 트리의 최대 노드의 수는 2^k - 1개
  ![이진 트리의 성질-3](https://user-images.githubusercontent.com/57928612/106423539-f652cb00-64a3-11eb-8b24-9e8d5205cae5.png)
  ![이진 트리의 성질-4](https://user-images.githubusercontent.com/57928612/106423577-0c608b80-64a4-11eb-88da-b237f8abc1ec.png)
- 원소가 있는 이진 트리에 대해서 n0를 leaf node의 수, n2를 degree가 2인 노드의 수라고 하면 n0 = n2 + 1이 성립
  ![이진 트리의 성질-5](https://user-images.githubusercontent.com/57928612/106423710-521d5400-64a4-11eb-80e4-fabe5692ef66.png)
  ![이진 트리의 성질-6](https://user-images.githubusercontent.com/57928612/106423743-66615100-64a4-11eb-8d16-964cd8d74704.png)

### 특별한 이진 트리
#### 포화 이진 트리(Full binary tree)
- Depth가 k일 때 2^k - 1개의 노드를 갖는 이진 트리
- ![포화 이진 트리](https://user-images.githubusercontent.com/57928612/106423923-afb1a080-64a4-11eb-8068-f1fca9e76b9e.png)
#### 완전 이진 트리(Complete binary tree)
- 같은 Depth를 갖는 포화 이진 트리와 노드들 사이에 1:1 대응이 성립하는 이진 트리
- 포화 이진 트리로 가는 과정(?)이라고 생각해도 좋을 것 같음 -> 중간이 비지 않음
![완전 이진 트리](https://user-images.githubusercontent.com/57928612/106425329-2354ad00-64a7-11eb-86e8-1be4f4e45426.png)

## 트리 탐색(Tree search)

### 탐색이란?
- 루트 노드로부터 시작해서 트리의 모든 노드를 방문하는 연산
- 모든 트리에서 적용되는 일반적인 탐색
  - 깊이 우선 탐색(Depth-first search, DFS)
  - 넓이 우선 탐색(Breadth-first search, BFS)
- 이진 트리에만 적용되는 특별한 탐색
  - 노드와 그 자식 노드를 방문하는 순서에 따라서 다음의 3가지 탐색이 가능
  - [중위 우선 탐색(Inorder traversal)](#중위-우선-탐색inorder-traversal)
    - 왼쪽 자식 노드 -> 부모 노드 -> 오른쪽 자식 노드
  - [전위 우선 탐색(Preorder traversal)](#전위-우선-탐색preorder-traversal)
    - 부모 노드 -> 왼쪽 자식 노드 -> 오른쪽 자식 노드
  - [후위 우선 탐색(Postorder traversal)](#후위-우선-탐색postorder-traversal)
    - 왼쪽 자식 노드 -> 오른쪽 자식 노드 -> 부모 노드
- 탐색에 걸리는 연산의 시간 복잡도
  - **O(n)**

![트리 탐색 예시](https://user-images.githubusercontent.com/57928612/106427005-f1911580-64a9-11eb-9492-9c61c441fbc0.png)
### 중위 우선 탐색(Inorder traversal)
- 왼쪽 -> 부모 -> 오른쪽
```cpp
void inorder(nptr bt) {
  if (bt) {
    inorder(bt -> lchild);
    print(bt -> data);
    inorder(bt -> rchild);
  }
}
```
  #### H -> D -> I -> B -> E -> A -> F -> C -> G

### 전위 우선 탐색(Preorder traversal)
- 부모 -> 왼쪽 -> 오른쪽
```cpp
void preorder(nptr bt) {
  if (bt) {
    print(bt -> data);
    preorder(bt -> lchild);
    preorder(bt -> rchild);
  }
}
```
  #### A -> B -> D -> H -> I -> E -> C -> F -> G

### 후위 우선 탐색(Postorder traversal)
- 왼쪽 -> 오른쪽 -> 부모
```cpp
void postorder(nptr bt) {
  if (bt) {
    postorder(bt -> lchild);
    postorder(bt -> rchild);
    print(bt -> data);
  }
}
```
  #### H -> I -> D -> E -> B -> F -> G -> C -> A
