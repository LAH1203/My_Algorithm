### 목차
> 1. [힙(Heap)](#힙heap)
>> 1) [힙의 정의](#힙의-정의)
>> 2) [힙의 구현](#힙의-구현)
>> 3) [힙의 연산](#힙의-연산)
>>> 1. [추가(push)](#추가push)
>>> 2. [제거(pop)](#제거pop)
> 2. [힙 정렬(Heap sort)](#힙-정렬heap-sort)
> 3. [성능 비교](#성능-비교)

## 힙(Heap)
### 힙의 정의
- 우선 순위 큐(Priority Queue)
  - 가장 우선 순위가 높은 원소가 가장 먼저 제거되는 큐
  - 연산
    - 추가(push)
      - 큐에 새로운 원소를 삽입
      - 새로운 원소의 위치는 그 중요도(priority)에 따라서 결정
    - 제거(pop)
      - 큐에서 가장 우선 순위가 높은 원소를 삭제
    - 탑(top)
      - 큐에서 가장 우선 순위가 높은 원소를 찾음(삭제 X)
  - 정렬된 리스트를 이용한 우선 순위 큐
    - 추가(push)
      - 원소를 중요도에 따라서 정렬된 리스트에 추가
      - O(n)
    - 제거(pop)
      - 리스트의 첫 번째 원소를 제거
      - O(n)
    - 탑(top)
      - 리스트의 첫 번째 원소를 리턴
      - O(1)
    - 추가와 제거의 성능을 향상시킬 수 있는 방법은? **이진 트리의 사용(heap)**
- 힙(heap)
  - 트리를 기반으로 구현된 우선 순위 큐
  - 완전 이진 트리(complete binary tree)
  - 종류
    - 최대 힙(max heap)
      - 완전 이진 트리
      - 모든 노드의 값은 자식 노드의 값들보다 큼
      ![최대 힙](https://user-images.githubusercontent.com/57928612/107166793-63adb100-69fa-11eb-8503-08a20a95594b.png)
    - 최소 힙(min heap)
      - 완전 이진 트리
      - 모든 노드의 값은 자식 노드의 값들보다 작음
      ![최소 힙](https://user-images.githubusercontent.com/57928612/107166828-7aec9e80-69fa-11eb-905e-c2580f179344.png)

### 힙의 구현
- 완전 이진 트리를 이용한 구현
  - 포인터를 이용한 구현
  - 배열을 이용한 구현
  - 완전 이진 트리에서 각 노드에 위에서 아래로, 왼쪽에서 오른쪽으로 번호 부여
  - 배열에서 첫 번째 원소부터 번호 부여
  - 번호는 1부터 시작
  - k번째 노드의 부모 노드 : **k/2**
  - k번째 노드의 왼쪽 자식 노드 : **2*k**
  - k번째 노드의 오른쪽 자식 노드 : **2*k + 1**
  ![힙의 구현](https://user-images.githubusercontent.com/57928612/107166951-de76cc00-69fa-11eb-9a86-5e85ff2ece66.png)
#### 자료구조
```cpp
int* heap;
int n;
int cnt;
```
```cpp
cnt = 0;
n = 1000;
heap = (int*)calloc(n, sizeof(int));
```
#### Heapify(k) 함수
- k번째 노드로부터 힙을 재구성할 것
- 하향식 재구성(Topdown heapify)
  - Leaf node까지 힙을 재구성
  ```cpp
  // recursive
  void heapify_topdown(int idx) {
    // leaf node에 도착하면 끝
    if (2 * idx > cnt)
      return;
    if (2 * idx == cnt) {
      // lchild만 있고 lchild(2*idx)가 parent(idx)보다 더 큰 경우
      if (heap[idx] < heap[2 * idx])
        swap(&heap[idx], &heap[2 * idx]);
      return;
    }
    // lchild(2*idx)가 rchild(2*idx+1)보다 더 크고 parent(idx)보다 더 큰 경우
    if (heap[2 * idx] > heap[2 * idx + 1] && heap[2 * idx] > heap[idx]) {
      swap(&heap[idx], &heap[2 * idx]);
      heapify_topdown(2 * idx);
    }
    // rchild(2*idx+1)가 lchild(2*idx)보다 더 크고 parent(idx)보다 더 큰 경우
    else if (heap[2 * idx + 1] > heap[2 * idx] && heap[2 * idx + 1] > heap[idx]) {
      swap(&heap[idx], &heap[2 * idx + 1]);
      heapify_topdown(2 * idx + 1);
    }
  }
  ```
  ```cpp
  // iterative
  void heapify_topdown(int idx) {
    while (2 * idx <= cnt) {
      if (2 * idx == cnt) {
        // lchild만 있고 lchild(2*idx)가 parent(idx)보다 더 큰 경우
        if (heap[idx] < heap[2 * idx]) {
          swap(&(heap[idx]), &(heap[2 * idx]));
          break;
        }
      }
      // lchild(2*idx)가 rchild(2*idx+1)보다 더 크고 parent(idx)보다 더 큰 경우
      if (heap[2 * idx] > heap[2 * idx + 1] && heap[idx] < heap[2 * idx]) {
        swap(&(heap[idx]), &(heap[2 * idx]));
        idx = 2 * idx;
      }
      // rchild(2*idx+1)가 lchild(2*idx)보다 더 크고 parent(idx)보다 더 큰 경우
      else if (heap[2 * idx] < heap[2 * idx + 1] && heap[idx] < heap[2 * idx + 1]) {
        swap(&(heap[idx]), &(heap[2 * idx + 1]));
        idx = 2 * idx + 1;
      }
      else {
        break;
      }
    }
  }
  ```
- 상향식 재구성(Bottomup heapify)
  - Root node까지 힙을 재구성
  ```cpp
  // recursive
  void heapify_bottomup(int idx) {
    // root node에 도착하면 끝
    if (idx == 1)
      return;
    // parent(idx/2)가 child(idx)보다 더 작으면
    if (heap[idx / 2] < heap[idx]) {
      swap(&heap[idx / 2], &heap[idx]);
      heapify_bottomup(idx / 2);
    }
  }
  ```
  ```cpp
  // iterative
  void heapify_bottomup(int idx) {
    // root node에 도착하면 끝
    while (idx > 1) {
      // parent(idx/2)가 child(idx)보다 더 작으면
      if (heap[idx / 2] < heap[idx]) {
        swap(&heap[idx / 2], &heap[idx]);
        idx = (idx / 2);
      } else {
        break;
      }
    }
  }
  ```

### 힙의 연산
#### 추가(push)
- 최대 힙에 새로운 원소를 추가
 1. 원소를 힙의 마지막 위치에 삽입
    - 완전 이진 트리 만족, 최대 힙 만족 실패
    ![추가-1](https://user-images.githubusercontent.com/57928612/107169932-6d3b1700-6a02-11eb-9269-4b303b303ab2.png)
 2. heapify()를 사용하여 새로운 원소가 삽입된 힙을 재구성
    - 최대 힙도 만족
    ![추가-2](https://user-images.githubusercontent.com/57928612/107170000-965ba780-6a02-11eb-9cd1-afe7f3d080ee.png)
    ![추가-3](https://user-images.githubusercontent.com/57928612/107170024-a4112d00-6a02-11eb-93c0-cc65f96d641f.png)
```cpp
void push(int ndata) {
  cnt++;
  heap[cnt] = ndata;
  heapify_bottomup(cnt);
}
```
- 시간 복잡도
  - 히입
    - n개의 원소를 가진 완전 이진 트리
    - height -> log(n)
  - 추가 연산의 시간 복잡도 -> **O(log n)**
    - 원소 추가 -> O(1)
    - 힙의 재구성 -> O(log n)

#### 제거(pop)
- 최대 힙에서 제거
 1. 루트 노드에 저장된 값을 제거
   ![제거-1](https://user-images.githubusercontent.com/57928612/107170356-72e52c80-6a03-11eb-81b1-7331a60e0cf6.png)
 2. 힙의 마지막 노드에 저장된 값을 루트 노드로 옮기고 노드를 제거
    - 완전 이진 트리 만족, 최대 힙 만족 실패
    ![제거-2](https://user-images.githubusercontent.com/57928612/107170423-9f994400-6a03-11eb-85d5-564dfdb4bd1e.png)
 3. 루트 노드로부터 하향식 힙 재구성 수행
    - 최대 힙도 만족
    ![제거-3](https://user-images.githubusercontent.com/57928612/107170486-c788a780-6a03-11eb-9144-6171a13a5ef2.png)
    ![제거-4](https://user-images.githubusercontent.com/57928612/107170510-db340e00-6a03-11eb-9461-c1f5792c0c47.png)
```cpp
int pop() {
  int tmp = heap[1];
  heap[1] = heap[cnt];
  cnt--;
  heapify_topdown(1);
  return tmp;
}
```
- 시간 복잡도
  - 히입
    - n개의 원소를 가진 완전 이진 트리
    - height -> log(n)
  - 제거 연산의 시간 복잡도 -> **O(log n)**
    - 루트 노드의 원소 제거 -> O(1)
    - 마지막 노드 제거 -> O(1)
    - 힙의 재구성 -> O(log n)

## 힙 정렬(Heap sort)
- O(nlogn) 정렬 알고리즘
  - merge sort
  - quick sort
  - heap sort
- Heap sort
  - push n times
  - pop n times
```cpp
void heap_sort(int* arr, int n) {
  for (int i = 0; i < n; i++)
    push(arr[i]);
  for (int i = 0; i < n; i++)
    printf("%d, ", pop());
}
```
- 시간 복잡도
  - push n times
    - n X O(log n) -> **O(n log n)**
  - pop n times
    - n X O(log n) -> **O(n log n)**
  #### O(n log n)

## 성능 비교
![성능 비교](https://user-images.githubusercontent.com/57928612/107171046-2dc1fa00-6a05-11eb-867d-83f872899975.png)
