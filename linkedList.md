### 목차
> 1. [연결 리스트(Linked List)](#연결-리스트linked-list)
>>  1) [연결 리스트의 정의](#연결-리스트의-정의)
>>  2) [구조체와 클래스를 이용한 정의](#구조체와-클래스를-이용한-연결-리스트의-정의)
>>  3) [배열과 연결 리스트의 비교](#배열과-연결-리스트의-비교)
>>  4) [연결 리스트의 종류](#연결-리스트의-종류)
>>  5) [연결 리스트의 연산](#연결-리스트의-연산)
> 2. [단일 연결 리스트(Singly linked list)](#단일-연결-리스트singly-linked-list)
>>  1) [단일 연결 리스트의 노드 정의](#단일-연결-리스트의-노드-정의)
>>  2) [단일 연결 리스트의 연산](#단일-연결-리스트의-연산)
> 3. [이중 연결 리스트(Doubly linked list)](#이중-연결-리스트doubly-linked-list)
>>  1) [이중 연결 리스트의 정의](#이중-연결-리스트의-정의)
>>  2) [이중 연결 리스트의 연산](#이중-연결-리스트의-연산)
> 4. [시간 복잡도](#시간-복잡도)
> 5. [연산 비교](#연산-비교)
>>  1) [검색 연산의 비교](#검색-연산의-비교)
>>  2) [추가 연산의 비교](#추가-연산의-비교)
>>  3) [제거 연산의 비교](#제거-연산의-비교)


## 연결 리스트(Linked List)

### 연결 리스트의 정의
- node는 메모리에서 임의의 위치(at arbitrary position)에 배치되어 있음
- 각 node는 다음 node를 가리키는 주소(link, pointer)를 가지고 있음(포인터 기반의 자료구조)

  #### data + link -> node

### 구조체와 클래스를 이용한 연결 리스트의 정의
#### Case 1
```cpp
typedef struct _node node;
struct _node {
  char ats[3];
  node* link;
};
```

#### Case 2
```cpp
class node {
  char ats[3];
  node* link;
}
```

### 배열과 연결 리스트의 비교
|| 배열 | 연결 리스트 |
|:---:|:---:|:---:|
| 메모리 공간 | 연속된 메모리 주소 | 이산된 메모리 주소 |
| 공간 할당 | 정적 할당/동적 할당 | 동적 할당 |
| 접근 경로 | 첫 번째 원소의 주소 | 첫 번째 원소의 주소 |
| 접근 방법 | 인덱스 | 포인터 |
| 접근 방식 | Random access | Sequential access |
| 공간 복잡도 | 데이터(O(n)) | 데이터 + 포인터(O(n)) |

### 연결 리스트의 종류
#### 단일 연결 리스트
- 각 node는 하나의 link를 가짐
  - next 또는 link
- Chain

#### 이중 연결 리스트
- 각 node는 두 개의 link를 가짐
  - next 또는 rlink
  - prev 또는 llink

![연결 리스트 종류 예시](https://user-images.githubusercontent.com/57928612/105126252-ada12680-5b21-11eb-923d-5d55248c0fc3.PNG)

### 연결 리스트의 연산
| 연산 | 정렬된 연결 리스트(2->5->7->9->10) | 정렬되지 않은 연결 리스트(5->2->7->10->9) |
|:---:|:---:|:---:|
| 검색 | Linear_search(A,x) | Linear_search(A,x) |
| 추가 | Insert_by_value(A,x) | Insert(A,x) |
|| (A,8): 2->5->7->8->9->10 | (A,8): 8->5->2->7->10->9 |
||| Insert_by_index(A,i,x) |
||| (A,3,8): 5->2->7->8->10->9 |
| 제거 | Delete_by_value(A,x) | Delete_by_value(A,x) |
|| (A,5): 2->7->9->10 | (A,5): 2->7->10->9 |
|| Delete_by_index(A,i) | Delete_by_index(A,i) |
|| (A,3):2->5->7->10 | (A,3): 5->2->7->9 |


## 단일 연결 리스트(Singly linked list)

### 단일 연결 리스트의 노드 정의
```cpp
class node {
  data_type item;
  node* link;
};
class hnode {
  // head node
  node* link;
}
```
![단일 연결 리스트 노드 예시](https://user-images.githubusercontent.com/57928612/105127458-49339680-5b24-11eb-81c1-4653706bc863.png)

### 단일 연결 리스트의 연산
```
1. 생성(create)
2. 검색(search)
3. 추가(insert)
4. 제거(delete)
5. 갱신(modify)
6. 길이(length)
7. 다음(next)
8. 이전(previous)
9. ...
```

#### 생성(create)
- 원소가 없는 연결 리스트를 생성
```cpp
void main() {
  hnode first;
}
```
![단일 연결 리스트 생성](https://user-images.githubusercontent.com/57928612/105128888-48e8ca80-5b27-11eb-88d2-0af1fd74ab81.png)

#### 검색(search)
- 찾는 원소(key element)를 저장하고 있는 노드의 포인터를 리턴
- 선형 탐색만 가능
```cpp
void main() {
  node* temp = first.search("HAT");
}
```
![단일 연결 리스트 검색](https://user-images.githubusercontent.com/57928612/105129119-bb59aa80-5b27-11eb-910b-6ff864024b93.png)

##### Step 1. hnode에서 검색 시작
```cpp
node* hnode::search(data_type item) {
  return this->link->search(item);
}
```
##### Step 2. node에서의 검색
```cpp
node* node::search(data_type item) {
  // 1. Find a proper position
  node* curr = this;
  while (curr != NULL) {
    if (curr->item == item)
      return curr;  // Found
    curr = curr->link;
  }
  return NULL;  // Not found
}
```
##### 배열에서의 선형 검색과 비교
```cpp
// 배열에서의 선형 검색
for (int i = 0; i < n; i++)
  if (arr[i] == item)
    return arr[i];
```
```cpp
// 바꾸기 전 단일 연결 리스트의 검색
node* curr = this;
while (curr != NULL) {
  if (curr->item == item)
    return curr;  // Found
  curr = curr->link;
}
```
```cpp
// 바꾼 후 단일 연결 리스트의 검색
for (node* curr = this; curr != NULL; curr = curr->next)
  if (curr->item == item)
    return curr;
```

#### 추가(insert)
- 추가할 데이터를 저장하는 새로운 노드를 연결 리스트에 삽입하는 연산
```cpp
void main() {
  first.insert("GAT");
}
```
![단일 연결 리스트 추가 전](https://user-images.githubusercontent.com/57928612/105130624-b9ddb180-5b2a-11eb-8d7f-9bfa94fc1d5c.png)
![단일 연결 리스트 추가 후](https://user-images.githubusercontent.com/57928612/105130662-cbbf5480-5b2a-11eb-82bb-e862e34fbfc0.png)

##### 과정
```
1. 데이터를 추가할 적절한 위치를 결정
2. 데이터를 저장할 새로운 노드를 생성
3. 새로운 노드가 연결 리스트에 포함되도록 링크를 갱신
```

##### Step 1. hnode에서 추가 시작
```cpp
void hnode::insert(data_type item) {
  this->link->insert(item);
}
```

##### Step 2. node에서의 추가
```cpp
void node::insert(data_type item) {
  // 1. 적절한 위치 결정
  node* curr = this;
  while (curr->link != NULL) {
    if (curr->link->item > item)
      break;
    curr = curr->link;
  }
  // 2. 새로운 노드 생성
  node* nnode = new node;
  nnode->item = item;
  // 3. 링크 갱신
  nnode->link = curr->link;
  curr->link = nnode;
}
```

##### 예외적인 경우
- 첫 번째 노드 앞에 추가하는 경우
- 텅 빈 리스트에 처음으로 추가하는 경우
- 마지막 노드 뒤에 추가하는 경우
<br><br>**위와 같은 예외 경우를 고려하여 적절하게 코드를 더하는 것이 필요하다.**

#### 제거(delete)
- 리스트로부터 원하는 원소의 연결을 해제하여 리스트에서 그 원소를 삭제하는 연산
```cpp
void main() {
  first.delete("FAT");
}
```
![단일 연결 리스트 제거 전](https://user-images.githubusercontent.com/57928612/105132106-a6801580-5b2d-11eb-9f3e-fc774d6fd9b8.png)
![단일 연결 리스트 제거 후](https://user-images.githubusercontent.com/57928612/105132136-b8fa4f00-5b2d-11eb-93fa-6c064b0dff81.png)

##### 과정
```
1. 제거할 원소를 찾음
2. 링크를 갱신하여 제거할 원소를 리스트에서 분리
3. 제거된 노드의 메모리를 free함
```

##### Step 1. hnode에서 제거 시작
```cpp
void hnode::delete(data_type item) {
  this->link->delete(item);
}
```

##### Step 2. node에서의 제거
```cpp
void node::delete(data_type item) {
  // 1. 제거할 원소를 찾음
  node* curr = this;
  while (curr->link != NULL) {
    if (curr->link->item == item)
      break;
    curr = curr->link;
  }
  // 2. 링크 갱신
  node* dnode = curr->link;
  curr->link = dnode->link;
  // 3. 제거된 node를 free
  free(dnode);
}
```

##### 예외적인 경우
- 첫 번째 노드를 제거하는 경우
- 텅 빈 리스트에서 제거하는 경우
- 마지막 노드를 제거하는 경우
- 제거할 값이 없는 경우
<br><br>**위와 같은 예외 경우를 고려하여 적절하게 코드를 짜는 것이 필요하다.**

## 이중 연결 리스트(Doubly linked list)

### 이중 연결 리스트의 정의
- 각 node가 다음(next, rlink) node를 가리키는 link와 이전(prev, llink) node를 가리키는 link를 동시에 가지고 있는 연결 리스트
```cpp
class node {
  data_type item;
  node *llink, *rlink;
}
class hnode {
  // head node
  node* link;
}
```
![이중 연결 리스트 노드 예시](https://user-images.githubusercontent.com/57928612/105133277-cc0e1e80-5b2f-11eb-9ec3-3ee4457acf16.png)

### 이중 연결 리스트의 연산
#### 추가(insert)
- 추가할 데이터를 저장하는 새로운 노드를 연결 리스트에 삽입하는 연산
```cpp
void main() {
  first.insert("GAT");
}
```
![이중 연결 리스트 추가 전](https://user-images.githubusercontent.com/57928612/105134492-e21cde80-5b31-11eb-992b-ae96f2a19c5c.png)
![이중 연결 리스트 추가 후](https://user-images.githubusercontent.com/57928612/105134562-f9f46280-5b31-11eb-9ca2-b5402746eee9.png)

##### 과정
```
1. 데이터를 추가할 적절한 위치를 결정
2. 데이터를 저장할 새로운 노드를 생성
3. 새로운 노드가 연결 리스트에 포함되도록 링크를 갱신
```

##### 코드
```cpp
void node::insert(data_type item) {
  // 1. 적절한 위치 결정
  node* curr = this;
  while (curr->rlink != NULL) {
    if (curr->rlink->item > item)
      break;
    curr = curr->rlink;
  }
  // 2. 새로운 노드 생성
  node* nnode = new node;
  nnode->item = item;
  // 3. 링크 갱신
  // 3.1. rlink 방향(->)
  nnode->rlink = curr->rlink;
  curr->rlink = nnode;
  // 3.2. llink 방향(<-)
  nnode->llink = curr;
  nnode->rlink->llink = nnode;
}
```

##### 예외적인 경우
- 첫 번째 노드 앞에 추가하는 경우
- 텅 빈 리스트에 처음으로 추가하는 경우
- 마지막 노드 뒤에 추가하는 경우
<br><br>**위와 같은 예외 경우를 고려하여 적절하게 코드를 짜는 것이 필요하다.**

#### 제거(delete)
- 리스트로부터 원하는 원소의 연결을 해제하여 리스트에서 그 원소를 삭제하는 연산
```cpp
void main() {
  first.delete("FAT");
}
```
![이중 연결 리스트 제거 전](https://user-images.githubusercontent.com/57928612/105136225-a46d8500-5b34-11eb-8f38-4f130c67b1d7.png)
![이중 연결 리스트 제거 후](https://user-images.githubusercontent.com/57928612/105136248-ae8f8380-5b34-11eb-8115-63b03987c231.png)

##### 과정
```
1. 제거할 원소를 찾음
2. 링크를 갱신하여 제거할 원소를 리스트에서 분
3. 제거된 노드의 메모리를 free함
```

##### 코드
```cpp
void node::delete(data_type item) {
  // 1. 제거할 원소를 찾음
  node* curr = first;
  while (curr->rlink != NULL) {
    if (curr->rlink->item == item)
      break;
    curr = curr->rlink;
  }
  // 2. 링크 갱신
  // 2.1. rlink 갱신(->)
  node* dnode = curr->rlink;
  curr->rlink = dnode->rlink;
  // 2.2. llink 갱신(<-)
  curr->rlink->llink = dnode->llink;
  // 3. 제거된 node를 free
  free(dnode);
}
```

##### 예외적인 경우
- 첫 번째 노드를 제거하는 경우
- 텅 빈 리스트에서 제거하는 경우
- 마지막 노드를 제거하는 경우
<br><br>**위와 같은 예외 경우를 고려하여 적절하게 코드를 짜는 것이 필요하다.**


## 시간 복잡도
| 연산 | 단일 연결 리스트 | 이중 연결 리스트 |
|:---:|:---:|:---:|
| 검색(search) | O(n) | O(n) |
| 추가(insert) | O(n) | O(n) |
| 제거(delete) | O(n) | O(n) |

## 연산 비교

### 검색 연산의 비교
![검색 연산의 비교-1](https://user-images.githubusercontent.com/57928612/105138214-ee0b9f00-5b37-11eb-983a-bc1ece37fd69.png)
![검색 연산의 비교-2](https://user-images.githubusercontent.com/57928612/105138313-0b406d80-5b38-11eb-859d-a2eeecba8b49.png)

### 추가 연산의 비교
![추가 연산의 비교-1](https://user-images.githubusercontent.com/57928612/105138681-9457a480-5b38-11eb-8ebf-74eea5b8d441.png)
![추가 연산의 비교-2](https://user-images.githubusercontent.com/57928612/105138758-b0f3dc80-5b38-11eb-91b4-3e6af41aa197.png)

### 제거 연산의 비교
![제거 연산의 비교-1](https://user-images.githubusercontent.com/57928612/105138813-c49f4300-5b38-11eb-8654-a0d72966c7c2.png)
![제거 연산의 비교-2](https://user-images.githubusercontent.com/57928612/105138864-d7197c80-5b38-11eb-8b07-6e59608ee812.png)
