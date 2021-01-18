### 목차
> 1. [리스트(List)](#리스트list)
> 2. [배열(Array)](#배열array)
> 3. [자료의 관리(1) - 추가/삽입(insert)](#insert)
> 4. [자료의 관리(2) - 제거/삭제(delete, remove)](#delete-remove)
> 5. [자료의 관리(3) - 검색/탐색(search)](#search)

## 리스트(List)
### 리스트의 개념
+ 원소들을 한 줄로 나열한 구조(특별한 순서를 따름)
+ 가장 오랫동안 널리 사용된 구조

### 리스트의 정의
+ 유한한 원소들의 나열(a finite sequence of elements)
  + (ex) A = (2, 3, 5, 7, 11, 13, 17, 19)
+ **각 원소들은 인덱스에 대응됨(index -> element)**
  + 리스트의 가장 중요한 성질
  + | a0 | a1 | a2 | a3 | a4 | a5 | a6 | a7 |
    |:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
    | 2 | 3 | 5 | 7 | 11 | 13 | 17 | 19 |

### 리스트의 구현 방법
  + 배열(Array)
    + 인덱스에 기반한 구현
    + 연속된 기억 공간에 원소를 배치
    + 메모리에 배열의 크기보다 더 큰 공간이 허용될 때 사용
    + | 위치 | 메모리 |
      | :-----: | :-----: |
      | 1 | *NULL* |
      | 2 | BAT |
      | 3 | CAT |
      | 4 | EAT |
      | 5 | FAT |
      | 6 | HAT |
      | 7 | *NULL* |
  + 연결 리스트(Linked list)
    + 포인터에 기반한 구현
    + 서로 떨어진 기억 공간에 원소를 배치하고, 다음 원소의 주소를 가지고 있음
    + | 위치 | 메모리 | 다음 원소의 주소 |
      | :-----: | :-----: | :-----: |
      | 1 | *NULL* | *NULL* |
      | 2 | EAT | 9 |
      | 3 | *NULL* | *NULL* |
      | 4 | BAT | 7 |
      | 5 | HAT | *NULL* |
      | 6 | *NULL* | *NULL* |
      | 7 | CAT | 2 |
      | 8 | *NULL* | *NULL* |
      | 9 | FAT | 5 |

## 배열(Array)
### 배열의 특징
+ 리스트를 index를 이용해서 구현한 구조
+ 연속적으로 할당된 기억 공간
+ 모든 프로그래밍 언어에서 기본적으로 제공
+ 배열의 모든 원소들은 index에 대응됨
+ n개의 자료를 하나의 주소로 접근할 수 있음
  + 배열의 장점
  + 배열의 이름과 index로 해당 배열의 모든 원소에 접근할 수 있다는 말과 동일

### 배열의 구현
+ 요소
  + arr
    + 배열
  + size
    + 배열의 크기(할당받은 메모리의 크기)
  + count
    + 현재 배열에 저장된 원소의 수
      + count <= size
      + 반드시 0으로 초기화해서 사용할 것
+ 정적 배열(static array)
  ```cpp
  #define SIZE 100
  {
    int count = 0;
    int arr[SIZE];
  }
  ```
+ 동적 배열(dynamic array)
  ```cpp
  #define SIZE 100
  {
    int count = 0;
    int* arr = calloc(SIZE, sizeof(int));
  }
  ```

### 기본 연산
+ 생성(create)
  + n개의 원소를 저장할 수 있는 배열 공간을 할당
+ 인출(retrieve)
  + 배열의 i번째 원소를 읽어옴
+ 저장(store)
  + 원소 x를 배열의 i번째 위치에 저장
  + (ex) ```cpp
         int L[10];
         int x = L[5];
         L[5] = x;
         ```

### 추가 연산들
+ 검색(search)
+ 추가(insert)
+ 제거(delete)
+ 크기(size)
+ 풀(isFull)
+ 엠티(isEmpty)
+ store, retrieve 등...

## 자료의 관리
| 연산 | 정렬된 배열(A=[2,5,7,9,10]) | 정렬되지 않은 배열(A=[5,2,7,10,9]) |
|:-----:|:-----:|:-----:|
| 검색(search) | linear_search(A,x) | linear_search(A,x) |
|| binary_search(A,x) ||
| 추가(insert) | insert_by_value(A,x) : (A,8)->[2,5,7,8,9,10] | insert(A,x) : (A,8)->[5,2,7,10,9,8] |
||| insert_by_index(A,i,x) : (A,3,8)->[5,2,7,8,10,9] |
||| store(A,i,x) : (A,3,8)->[5,2,7,8,9] |
| 제거(delete) | delete_by_value(A,x) : (A,5)->[2,~~5~~,7,9,10] | delete_by_value(A,x) : (A,5)->[2,7,10,9] |
|| delete_by_index(A,i) : (A,3)->[2,5,7,~~9~~,10] | delete_by_index(A,i) : (A,3)->[5,2,7,9] |

### Insert
+ 추가(insert)의 정의
  + 배열의 적절한 위치에 새로운 원소를 삽입하는 연산
  + 새로운 원소가 삽입될 위치를 명확하게 지정하지 않음
+ 추가 알고리즘
  1. 위치 결정
      + x보다 큰 값들 중에서 가장 작은 값의 위치
  2. 공간 확보
      + 끝의 원소부터 추가해야할 위치의 원소까지를 한 칸씩 뒤로 옮겨서(복사해서) 공간을 확보
  3. 원소 추가
      + 추가할 원소를 1에서 결정한 위치에 추가
  4. 배열의 크기(count) 증가
+ 추가 알고리즘의 구현
```cpp
Array insert_by_value(Array arr, elt x) {
  // 0. 예외?
  // 배열에 원소가 없는 경우
  if (count == 0) {
    arr[0] = x;
    count++;
    return arr;
  }
  // 배열이 가득 찬 
  if (count == SIZE) {
    println("원소를 더 추가할 수 없습니다.");
    return arr;
  }
  
  // 1. x를 추가할 위치를 결정
  int i;
  for (i = 0; i < count; i++) {
    if (arr[i] > x)
      break;
  }
  
  // 2. 추가할 위치의 원소를 옮겨서 공간을 확보(->)
  for (int j = count - 1; j >= i; j--)
    arr[j + 1] = arr[j];
  
  // 3. 원소를 추가
  arr[i] = x;
  
  // 4. 배열의 크기(count)를 증가
  count++;
  return arr;
}
```
+ 추가 알고리즘의 시간 복잡도
1) 위치 결정 -> O(k)
2) 공간 확보 -> O(n-k)
3) 원소 추가 -> O(1)
4) 배열 크기 증가 -> O(1)
    #### **결론 : O(n)**

### Delete, Remove
+ 제거의 정의
  + 배열에서 원소를 삭제하는 연산
  + 배열에서 빈 공간이 남지 않도록 할 것
+ 제거 알고리즘
  1. 제거할 원소(x)를 배열(arr)에서 찾음
  2. 제거할 원소 뒤에 있는 원소들을 앞으로 이동
  3. 배열의 크기(count)를 감소
+ 제거 알고리즘 구현
```cpp
Array delete(Array arr, elt x) {
  // 0. 예외? : 배열에 원소가 없는 경우
  if (count == 0) {
    println("배열에 원소가 하나도 없습니다.");
    return arr;
  }
  
  // 1. 제거할 원소를 배열에서 찾음
  bool check = false;
  for (int i = 0; i < count; i++) {
    if (arr[i] == x) {
      check = true;
      break;
    }
  }
  // 0. 예외? : 해당 원소가 배열에 없는 경우
  if (!check) {
    println("해당 원소가 배열 내에 존재하지 않습니다.")
    return arr;
  }
  
  // 2. 제거할 원소 뒤에 있는 원소들을 앞으로 이동(<-)
  for (int j = i; j < count - 1; j++)
    arr[j] = arr[j + 1];
  
  // 3. 배열의 크기를 감소
  count--;
  return arr;
}
```
+ 제거 알고리즘의 시간 복잡도
1) 제거할 원소를 배열에서 찾음 -> O(k)
2) 제거할 원소 뒤에 있는 원소들을 앞으로 이동 -> O(n-k)
3) 배열의 크기 감소 -> O(1)
    #### **결론 : O(n)**

### Search
+ 검색(search, 탐색)의 정의
  + 내가 찾는 원소(key element)가 주어진 배열에 존재하는지 여부를 판단해서 리턴
    + 있으면 true, 없으면 false 리턴
  + 내가 찾는 원소의 index를 리턴
    + 내가 찾는 원소가 없으면 -1 또는 NULL을 리턴
+ 검색의 종류
  + 선형 검색(linear search)
    + 완전 검색(exhaustive search) 또는 순차 검색(sequential search)
    + 배열의 첫 번째 원소부터 차례로 방문하면서 key element와 동일한 원소가 있는지 확인
    + 정렬된 배열, 정렬되지 않은 배열 모두 적용할 수 있음
  + 이진 검색(binary search)
    + 분할 정복(divide & conquer) 알고리즘
    + 배열의 중간 원소(mid element)와 key element를 비교하여 배열을 분할함으로써 검색을 수행
    + 정렬된 배열에서만 적용할 수 있음
+ 선형 검색 알고리즘
  + 배열의 첫 번째 원소부터 차례대로 방문하면서 key element와 동일한 원소가 있는지 확인
  + i번째 원소와 key element를 비교 -> 같으면 i를 리턴
    ```cpp
    if (arr[i] == x)
      return i;
    ```
  + 모든 원소(0~n-1)에 대해서 비교
    ```cpp
    for (int i = 0; i < n; i++)
      if (arr[i] == x)
        return i;
    ```
  + 끝까지 같은 원소를 찾지 못했으면 -1을 리턴
    ```cpp
    return -1;  // NULL
    ```
  + 선형 검색 함수의 구성
    + 입력 : 배열(arr)과 key element(x)
    + 출력 : x의 index
  ```cpp
  index linear_search(Array arr, elt x) {
    for (int i = 0; i < count; i++)
      if (arr[i] == x)
        return i;
    return -1;
  }
  ```
+ 선형 검색의 시간 복잡도
  + 최악의 경우 : O(n)
  + 평균의 경우 : O(n/2) -> O(n)
  + 최선의 경우 : O(1)
      #### **결론 : O(n)**
+ 이진 검색 알고리즘
  + 배열의 중간 원소(middle)을 찾아서 배열을 절반으로 분할하여 탐색을 재귀적으로 수행
  + 배열에서 검색할 범위를 지정할 것
    + 시작 위치 : s
    + 끝 위치 : e
  + arr[mid]와 key element를 비교
    + arr[mid] == key element -> mid를 리턴
    + arr[mid] > key element -> mid보다 작은 부분에서 검색(arr[s] ~ arr[mid-1])
    + arr[mid] < key element -> mid보다 큰 부분에서 검색(arr[mid+1] ~ arr[e])
    ```cpp
    mid = (s + e) / 2;
    if (arr[mid] == x)
      return mid;
    if (arr[mid] > x)
      return binary_search(s, mid - 1);
    if (arr[mid] < x)
      return binary_search(mid + 1, e);
    ```
  + case 1
  ```cpp
  index binary_search(Array arr, index s, index e, elt x) {
    // 예외처리
    if (s == e)
      return (arr[s] == x) ? s : -1;
    
    int mid = (s + e) / 2;
    if (x == arr[mid])
      return mid;
    else if (arr[mid] > x)
      return binary_search(arr, s, mid - 1, x);
    else
      return binary_search(arr, mid + 1, e, x);
  }
  ```
  + case 2
  ```cpp
  index binary_search(Array arr, elt x) {
    int s = 0;
    int e = count - 1;
    int mid;
    
    while (s <= e) {
      mid = (s + e) / 2;
      if (x == arr[mid])
        return mid;
      else if (arr[mid] > x)
        e = mid - 1;
      else
        s = mid + 1;
    }
    return -1;
  }
  ```
+ 이진 검색의 시간 복잡도
  + 최선의 경우 : O(1)
  + 최악의 경우 : O(log n)
    #### **결론 : O(log n)**
  
