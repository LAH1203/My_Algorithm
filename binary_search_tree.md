### 목차
> 1. [이진 탐색 트리(Binary Search Tree) 정의](#이진-탐색-트리binary-search-tree-정의)
> 2. [이진 탐색 트리(Binary Search Tree) 연산](#이진-탐색-트리binary-search-tree-연산)
>> 1) [생성(초기화)](#생성초기화)
>> 2) [검색(search)](#검색)
>> 3) [추가(insert)](#추가)
>> 4) [제거(delete)](#제거)
>> 5) [성능 비교](#성능-비교)
> 3. [균형 이진 탐색 트리(Balanced Binary Search Tree)](#균형-이진-탐색-트리balanced-binary-search-tree)
>> 1) [AVL tree](#avl-tree)
>> 2) [Red-black tree](#red-black-tree)
>> 3) [2-3 tree](#2-3-tree)
>> 4) [B+ tree](#b-tree)

## 이진 탐색 트리(Binary search tree) 정의
### 이진 탐색(Binary search)
- 배열의 중앙값을 찾아서 배열을 반으로 분할하는 과정을 재귀적으로 반복하면서 key 값을 검색
### 이진 트리와의 유사성
- 배열이 2개의 부분 배열로 재귀적으로 분할
- 분할 구조
  - 배열
    - (왼쪽 부분 배열) + 중앙 + (오른쪽 부분 배열)
    - 각 부분 배열이 또 분할됨(부분 배열의 원소 수가 1개가 될 때까지)
  - 이진 트리
    - (왼쪽 subtree) + 부모 노드 + (오른쪽 subtree)
    - 각 subtree가 또 분할됨(subtree의 원소 수가 1개가 될 때까지)
### 이진 탐색 트리(Binary search tree)
- 이진 탐색 과정에서 분할되는 배열을 이진 트리에 대응시킨 구조
- 이진 탐색 트리는 다음의 조건을 만족시킴
  - 모든 노드는 서로 다른 하나의 값(key)을 저장함
  - **왼쪽** subtree의 값들은 root node의 값보다 **더 작음**
  - **오른쪽** subtree의 값들은 root node의 값보다 **더 큼**
  - 왼쪽 subtree와 오른쪽 subtree는 **이진 탐색 트리**임
- 좋은 이진 탐색 트리 vs 나쁜 이진 탐색 트리
  - 이진 탐색 트리의 시간복잡도는 항상 최선의 경우와 최악의 경우를 따로 고려해야 함
  - 균형(balanced): |depth(left)-depth(right)|<=1 **vs** 편향(skewed)
  - 최선의 경우
    - depth = O(log n)
  - 최악의 경우
    - depth = O(n)
### 이진 탐색 트리(Binary search tree) 자료구조
```cpp
typedef class node *nptr;
class node {
  dataType key;
  nptr lchild, rchild;
};
```

## 이진 탐색 트리(Binary search tree) 연산
1. [생성(초기화)](#생성초기화)
2. [검색](#검색)
3. [추가](#추가)
4. [제거](#제거)
### 생성(초기화)
- 초기화
  - root node를 선언
  - 원소가 없는 이진 탐색 트리는 root node의 key를 원소로 사용되지 않을 값(ex: -1)로 초기화
### 검색
- 이진 탐색 트리에서 원하는 원소(x)를 찾는 연산
- **root node**부터 검색 시작
- 3가지 경우
  - x == root->key
    - Bingo!
  - x < root->key
    - 왼쪽 서브 트리로 이동
  - x > root->key
    - 오른쪽 서브 트리로 이동
```cpp
// 재귀 호출을 이용한 구현
void node::search(int x) {
  if (x == this -> key)
    printf("Bingo!!\n");
  else if (x < this -> key) {
    if (this -> lchild != NULL)
      this -> lchild -> search(x);
    else
      printf("Not found\n");
  else {
    // x > this -> key
    if (this -> rchild != NULL)
      this -> rchild -> search(x);
    else
      printf("Not found\n");
  }
}
```
- 시간 복잡도(n개의 노드)
  - 최선의 경우(balanced)
    - 최선의 경우일 때 이진 탐색 트리의 depth는 log n
    - 최악의 경우 검색은 leaf node에서 끝남
    - 따라서 시간 복잡도는 **O(log n)**!
  - 최악의 경우(skewed)
    - 최악의 경우일 때 이진 탐색 트리의 depth는 n
    - 최악의 경우 검색은 leaf node에서 끝남
    - 따라서 시간 복잡도는 **O(n)**!
### 추가
- 이진 탐색 트리에 새로운 원소를 더하는 연산
- 새로운 원소에 저장하는 **노드를 생성**하여 추가
- 새로 추가되는 노드는 반드시 **leaf node**여야 함!
- 3가지 경우(검색과 유사한 접근 방법)
  - x == root->key
    - Nooop!(정의에 위배)
  - x < root->key
    - 왼쪽 서브 트리로 이동
  - x > root->key
    - 오른쪽 서브 트리로 이동
```cpp
void node::insert(int x) {
  // degenerate case: root node에 삽입
  if (this -> key == -1) {
    this -> key = x;
    return;
  }
  // x와 key가 같으면
  if (this -> key == x)
    printf("No duplicate data\n");
  // x가 key보다 작으면
  else if (x < this -> key) {
    // lchild에 추가
    if (this -> lchild != NULL)
      this -> lchild -> insert(x);
    else {
      this -> lchild = (nptr)malloc(sizeof(node));
      this -> lchild -> key = x;
      this -> lchild -> lchild = this -> lchild -> rchild = NULL;
    }
  }
  // x가 key보다 크면
  else {
    // rchild에 추가
    if (this -> rchild != NULL)
      this -> rchild -> insert(x);
    else {
      this -> rchild = (nptr)malloc(sizeof(node));
      this -> rchild -> key = x;
      this -> rchild -> lchild = this -> rchild -> rchild = NULL;
    }
  }
}
```
- 시간 복잡도(n개의 노드)
  - 최선의 경우(balanced)
    - 최선의 경우 이진 탐색 트리의 depth는 log n
    - 추가는 항상 leaf node에서 끝남
    - 따라서 시간 복잡도는 **O(log n)**!
  - 최악의 경우(skewed)
    - 최악의 경우 이진 탐색 트리의 depth는 n
    - 추가는 항상 leaf node에서 끝남
    - 따라서 시간 복잡도는 **O(n)**!
### 제거
- 이진 탐색 트리의 노드를 지우는 연산
- 제거할 노드를 찾아가는 과정은 검색/추가와 비슷한 재귀적 구조로 구현됨
- 어떤 노드를 제거하느냐에 따라서 다른 연산 방법이 필요함
  - leaf node
    - 바로 제거
    ![leaf node 제거](https://user-images.githubusercontent.com/57928612/106232225-cce13780-6236-11eb-8a5c-a50fe7c0b67e.png)
  - 한 개의 child node를 갖는 노드
    - 노드를 제거하고 child node로 대체
    ![한 개의 child node를 갖는 노드 제거](https://user-images.githubusercontent.com/57928612/106232267-eb473300-6236-11eb-9bbf-c722e06b3cc9.png)
  - 두 개의 child node를 갖는 노드
    - 노드의 key를 left_max(또는 right_min)으로 대체하고 left_max(또는 right_min) 노드를 제거
    - left_max
      - 왼쪽 부분 트리의 원소 중 최댓값
    - right_min
      - 오른쪽 부분 트리의 원소 중 최값
      ![두 개의 child node를 갖는 노드 제거](https://user-images.githubusercontent.com/57928612/106232394-45e08f00-6237-11eb-9e45-1b79409eeab2.png)
- 재귀적인 구현
  - 제거 연산이 검색/추가 연산과 다른 점
    - 함수가 정수형의 리턴값을 가짐
  - 노드는 자기 자신을 파괴할 수 없기 때문에 부모 노드에게 자신을 파괴해달라고 요청함
  ```cpp
  int node::remove(int x) {
    // x와 key가 같으면
    if (x == this -> key) {
      printf("Removing %d\n", x);
      // (1) lchild, rchild의 값이 모두 NULL -> leaf node
      if (this -> lchild == NULL && this -> rchild == NULL)
        return 1;
      // (2-1) lchild의 값만 NULL
      if (this -> lchild == NULL && this -> rchild != NULL) {
        this -> key = this -> rchild -> key;
        this -> lchild = this -> rchild -> lchild;
        this -> rchild = this -> rchild -> rchild;
        return 0;
      }
      // (2-2) rchild의 값만 NULL
      if (this -> lchild != NULL && this -> rchild == NULL) {
        this -> key = this -> lchild -> key;
        this -> lchild = this -> lchild -> lchild;
        this -> rchild = this -> lchild -> rchild;
        return 0;
      }
      // (3) lchild, rchild의 값이 모두 NULL이 아닌 경우
      if (this -> lchild != NULL && this -> rchild != NULL) {
        this -> key = this -> lchild -> get_max();
        if (this -> lchild -> remove(this -> key) == 1)
          this -> lchild = NULL;
        return 0;
      }
    }
    // x가 key보다 작으면
    else if (x < this -> key) {
      // this -> lchild -> remove(x);
      if (this -> lchild != NULL) {
        if (this -> lchild -> remove(x) == 1)
          this -> lchild = NULL;
      }
      else
        printf("Not found %d in removing\n", x);
      return 0;
    }
    // x가 key보다 크면
    else {
      // this -> rchild -> remove(x);
      if (this -> rchild != NULL) {
        if (this -> rchild -> remove(x) == 1)
          this -> rchild = NULL;
      }
      else
        printf("Not found %d in removing\n", x);
      return 0;
    }
  }
  ```
- 시간 복잡도(n개의 노드)
  - 최선의 경우(balanced)
    - 최선의 경우 이진 탐색 트리의 depth는 log n
    - 최악의 경우 제거는 leaf node에서 이루어짐
    - 따라서 시간 복잡도는 **O(log n)**!
  - 최악의 경우(skewed)
    - 최악의 경우 이진 탐색 트리의 depth는 n
    - 최악의 경우 제거는 leaf node에서 이루어짐
    - 따라서 시간 복잡도는 **O(n)**!
### 성능 비교
| 자료구조 | 추가(insert) | 제거(delete) | 검색(search) | 최댓값 찾기(top) | 최댓값 제거(pop) |
| --- | :---: | :---: | :---: | :---: | :---: |
| 정렬된 배열 | O(n) | O(n) | O(log n) | O(1) | O(n) |
| 정렬된 연결 리스트 | O(n) | O(n) | O(n) | O(1) | O(1) |
| 이진 탐색 트리(최선) | O(log n) | O(log n) | O(log n) | - | - |
| 이진 탐색 트리(최악) | O(n) | O(n) | O(n) | - | - |

#### 이진 탐색 트리의 단점
  - 최악의 경우, O(n)의 시간 복잡도가 발생
  - 이를 피하기 위해 균형 이진 탐색 트리 등장!

## 균형 이진 탐색 트리(Balanced binary search tree)
- Self-balancing search tree
- Height-balanced search tree
- Height를 최소로 유지하도록 설계된 이진 탐색 트리
1. [AVL tree](#avl-tree)
2. [Red-black tree](#red-black-tree)
3. [2-3 tree](#2-3-tree)
4. [B+ tree](#b-tree)
### AVL tree
- G.M.Adelson-Velskii & E.M.Landis(1962)
- 모든 노드에서 왼쪽 서브트리와 오른쪽 서브트리의 height의 차를 1 이하로 유지
- Balance factor = Height of right subtree - Height of left subtree <= 1
![AVL tree](https://user-images.githubusercontent.com/57928612/106235683-8263b900-623e-11eb-861a-78e6cb28f6e5.png)
- 추가/제거 연산으로 인해 balance가 깨질 수 있음
  - 항상 balance가 유지되도록 **balancing 연산**을 수행!
- 자료구조
```cpp
class AVL {
  node *parent;
  node *left;
  node *right;
  int key;
}
```
#### balancing 연산
- Single rotate
  - Single left rotate
  - Single right rotate
- Double rotate
  - Double left rotate
  - Double right rotate

코드 참고 : https://box0830.tistory.com/146
```cpp
void AVL::rotate(node *now) {
  node *tmp = now;
  while (tmp != nullptr) {
    // balance check
    int factor = needRotate(tmp);
    if (factor > 1) {
      if (needRotate(tmp -> left) > 0)
        LL(tmp);
      else
        LR(tmp);
    }
    else if (factor < -1) {
      if (needRotate(tmp -> right) > 0)
        RL(tmp);
      else
        RR(tmp);
    }
    tmp = tmp -> parent;
  }
}

int AVL::needRotate(node *position) {
  int left, right;
  node *leftP = position -> left;
  node *rightP = position -> right;
  left = right = 0;
  if (leftP != nullptr && leftP -> key != NULL)
    left = height(leftP);
  if (rightP != nullptr && rightP -> key != NULL)
    right = height(rightP);
  int dif = left - right;
  return dif;
}

// rotate from left -> a, b, c nodes
node *AVL::LL(node *rot) {
  node *tmp = rot -> left;
  // b -> top
  tmp -> parent = rot -> parent;
  if (isRoot(tmp))
    root = tmp;
  else {
    if (rot = rot -> parent -> left)
      rot -> parent -> left = tmp;
    else
      rot -> parent -> right = tmp;
  }
  // T_2 to c
  rot -> left = tmp -> right;
  tmp -> right -> parent = rot;
  // b and c link
  tmp -> right = rot;
  rot -> parent = tmp;
  
  return tmp;
}

node *AVL::RR(node *rot) {
	node *tmp = rot -> right;

	// b -> top
	tmp -> parent = rot -> parent;
	if (isRoot(tmp))
    root = tmp;
	else {
		if (rot == rot -> parent -> left)
      rot -> parent -> left = tmp;
		else
      rot -> parent -> right = tmp;
	}

	// T_1 to a
	rot -> right = tmp -> left;
	tmp -> left -> parent = rot;

	// a and b link
	tmp -> left = rot;
	rot -> parent = tmp;

	return tmp;
}

node *AVL::RL(node *rot) {
	// first step
	node *tmp = rot -> right;
	node *tmp2 = LL(tmp);
	rot -> right = tmp2;
	tmp2 -> parent = rot;

	// last step
	return RR(rot);
}

node *AVL::LR(node *rot) {
	// first step
	node *tmp = rot -> left;
	node *tmp2 = RR(tmp);
	rot -> left = tmp2;
	tmp2 -> parent = rot;

	// last step
	return LL(rot);
}
```
### Red-black tree
- L.J.Guibas and R.Sedgewick(1978)
- 다음의 5가지 성질을 만족하는 이진 탐색 트리
  - 모든 노드는 red 또는 black
  - Root node는 black
  - 모든 leaf node(NIL node)는 black
  - 한 노드가 red라면, 자식 노드는 모두 black
  - 한 노드에서 모든 자식 노드까지의 경로에는 동일한 수의 black 노드가 존재
![Red-black tree](https://user-images.githubusercontent.com/57928612/106240114-0e79de80-6247-11eb-8394-c458156ae8a7.png)
### 2-3 tree
- J.Hpcroft(1970)
- 2개 또는 3개의 자식 노드를 갖는 탐색 트리
  - 2-node
    - 하나의 값이 저장되며 2개의 자식 노드를 가짐
  - 3-node
    - 2개의 값이 저장되며 3개의 자식 노드를 가짐
### B+ tree
- 여러 개의 자식 노드를 갖는 트리(k-ary tree)
- 각 노드는 (k-1)개의 key 값과 k개의 부분 트리를 가짐
