### 목차
> 1. [큐(Queue)의 개념](#큐queue의-개념)
> 2. [큐(Queue)의 자료구조](#큐queue의-자료구조)
> 3. [큐(Queue)의 연산](#큐queue의-연산)
>> 1) [생성(CreateQueue)](#생성createqueue)
>> 2) [엠티(IsEmpty)](#엠티isempty)
>> 3) [풀(IsFull)](#풀isfull)
>> 4) [추가(Enqueue)](#추가enqueue)
>> 5) [제거(Dequeue)](#제거dequeue)
>> 6) [비교](#비교)
> 4. [특수한 큐](#특수한-)
>> 1) [원형 큐(Circular Queue)](#원형-큐circular-queue)
>> 2) [DEQ(Doubly-Ended Queue)](#deqdoubly-ended-queue)
>> 3) [우선 순위 큐(Priority Queue)](#우선-순위-큐priority-queue)
> 5. [Josepus Problem](#josepus-problem)

## 큐(Queue)의 개념
- 원소와 그 도착 시간을 저장한 리스트
- First-In First-Out(FIFO) 또는 Last-In Last-Out(LILO)
- 원소의 추가와 제거가 Front(앞)와 Rear(뒤)라 불리우는 양 끝에서 일어나는 리스트
  - REAR(뒤) : 원소가 추가되는 곳
  - FRONT(앞) : 원소가 제거되는 곳
  - 원소를 추가하는 연산 - ENQUEUE(ADDQ, ADD, PUSH)
  - 원소를 제거하는 연산 - DEQUEUE(DELETEQ, REMOVE, POP)

![Queue](https://user-images.githubusercontent.com/57928612/106083957-b36dbc00-6160-11eb-935d-8c4970db5ffc.png)

## 큐(Queue)의 자료구조
- size : 큐에 저장할 수 있는 원소의 수
- List of elements : 원소를 저장하는 리스트
- rear : rear의 위치를 나타내는 값
- front : front의 위치를 나타내는 값
```cpp
class queue {
  int size;
  DataType *Items;
  int rear, front;
}
```

## 큐(Queue)의 연산
- [생성(CreateQueue)](#생성createqueue)
  - 지정된 크기의 원소를 저장하는 큐를 할당
  - O(1)
- [엠티(IsEmpty)](#엠티isempty)
  - 큐에 원소가 없으면 True를 리턴
  - O(1)
- [풀(IsFull)](#풀isfull)
  - 큐에 더 원소를 넣을 수 없으면 True를 리턴
  - O(1)
- [추가(Enqueue)](#추가enqueue)
  - 큐에 새로운 원소를 삽입
  - O(1)
- [제거(Dequeue)](#제거dequeue)
  - 큐에서 원소를 제거
  - O(1)

### 생성(CreateQueue)
- size개의 원소를 저장하는 공간을 할당
- front와 rear를 -1로 초기화
```cpp
void queue::CreateQueue(int _size) {
  size = _size;
  Items = new Datatype[size];
  front = rear = -1;
}
void main() {
  Queue myQueue.CreateQueue(4);
}
```

### 엠티(IsEmpty)
- 큐가 비어 있으면(empty), True를 리턴
- 큐가 비어 있음 <-> front == rear
```cpp
int queue::IsEmpty() {
  return (front == rear);
}
```
![IsEmpty](https://user-images.githubusercontent.com/57928612/106084763-4824e980-6162-11eb-8bb8-7315e94413a8.png)

### 풀(IsFull)
- 큐에 더 이상 원소를 추가할 수 없으면 True를 리턴
- 더 이상 원소를 추가할 수 없음 <-> rear == size(- 1)
```cpp
int queue::IsFull() {
  return (rear == size - 1);
}
```
![IsFull](https://user-images.githubusercontent.com/57928612/106084925-9afea100-6162-11eb-9fdd-ce1098616e95.png)
  #### 위의 사진에서 두 번째 예시처럼 front 앞이 비어있어도 가득 찼다는 말이 나올 수도 있음.. -> 해결책 : [원형 큐](#원형-큐circular-queue)

### 추가(Enqueue)
- 큐에 새로운 원소를 삽입하는 연산
- Enqueue는 오직 rear에서만 수행됨
- rear를 증가시키고 추가할 것!
```cpp
void queue::Enqueue(element item) {
  // 예외
  if (IsFull())
    printf("Cannot add an element to a full queue);
  rear++;
  Items[rear] = item;
}
```
![Enqueue](https://user-images.githubusercontent.com/57928612/106085402-70611800-6163-11eb-8b1e-8e172786ecaf.png)

### 제거(Dequeue)
- 큐에서 원소를 하나 삭제하고 리턴하는 연산
- dequeue는 front에서 제거함
- 제거하기 전에 front를 1 증가시킬 것!
```cpp
element queue::Dequeue() {
  if (IsEmpty())
    printf("You cannot delete from an empty queue);
  front++;
  return Items[front];
}
```
![Dequeue](https://user-images.githubusercontent.com/57928612/106085656-f2e9d780-6163-11eb-9211-9e9910ce37d6.png)

### 비교
![큐의 비교-1](https://user-images.githubusercontent.com/57928612/106085978-90dda200-6164-11eb-86ae-616c2a11defe.png)
![큐의 비교-2](https://user-images.githubusercontent.com/57928612/106086028-abb01680-6164-11eb-928d-193c094642ba.png)

## 특수한 큐

### 원형 큐(Circular Queue)
- 큐의 문제점?
  - 큐에 빈 공간이 있어도 REAR == size이면 Full이라고 판단
- 해결책?
  - 큐의 마지막 원소와 첫 번째 원소를 연결
  ![원형 큐-1](https://user-images.githubusercontent.com/57928612/106086745-1c0b6780-6166-11eb-9420-3433bebb01e0.png)
![원형 큐-2](https://user-images.githubusercontent.com/57928612/106086974-73113c80-6166-11eb-8225-b75dfe08769f.png)
- 장점
  - 스택과 같이 무한한 추가와 제거가 가능
- 단점
  - Full과 Empty의 상태가 동일해짐
    - FRONT == REAR
  - 해결책
    - 큐에 저장된 원소를 나타내는 새로운 변수(count)가 필요
#### 자료구조
```cpp
class circularQueue {
  int size;
  DataType *Items;
  int rear, front;
  int count;
}
```
#### IsFull 연산
```cpp
void circularQueue::IsFull() {
  return (count == size);
}
```
#### IsEmpty 연산
```cpp
void circularQueue::IsEmpty() {
  return (count == 0);
}
```
#### Enqueue 연산
```cpp
void circularQueue::Enqueue() {
  if (IsFull())
    printf("Cannot add an element to a full queue);
  rear = (rear + 1) % size;
  Items[rear] = item;
  count++;
}
```
#### Dequeue 연산
```cpp
element circularQueue::Dequeue() {
  if (IsEmpty())
    printf("You cannot delete from an empty queue);
  count--;
  front = (front + 1) % size;
  return Items[front];
}
```

### DEQ(Doubly-Ended Queue)
- Enqueue와 Dequeue가 queue의 양 끝에서 일어나는 형태의 queue
  - Enqueue_FRONT();
    - FRONT에 새로운 원소 추가
  - Dequeue_FRONT();
    - FRONT에서 원소 제거
  - Enqueue_REAR();
    - REAR에 새로운 원소 추가
  - Dequeue_REAR();
    - REAR에서 원소 제거
- 장점
  - 스택과 큐를 동시 지원
  - 큐로 사용
    - Enqueue_REAR & Dequeue_FRONT
    - Enqueue_FRONT & Dequeue_REAR
  - 스택으로 사용
    - Enqueue_REAR & Dequeue_REAR
    - Enqueue_FRONT & Dequeue_FRONT

### 우선 순위 큐(Priority Queue)
- 우선 순위에 따라 원소를 추가하거나 제거하는 큐
- 큐의 원소는 우선 순위에 따라서 배치됨
- 우선 순위를 유지하도록 원소를 추가
- 항상 큐의 첫 번째 원소는 가장 우선 순위가 높은 원소를 유지함
- 큐의 첫 번째 원소만 제거
- 트리 형태의 구현(heap)
#### 연산
- 추가
  - 원소를 우선 순위에 따라 큐의 적절한 위치에 삽입하는 연산
  - O(n)
- 제거
  - 큐의 첫 번째 위치에 있는 우선 순위가 가장 높은 원소를 삭제하는 연산
  - O(n)
- 탑
  - 큐에서 가장 우선 순위가 높은 원소를 리턴하는 연산(제거 X)
  - O(1)


## Josepus Problem
- Step 1. Building a queue of 41 elements
- Step 2. Remove an item and add it to the queue
- Step 3. Remove every third item and don't add it to the queue
- Step 4. Repeat this process until there are only two items in the queue
```cpp
void Josepus(int n, int k) {
  // n: number of persons in the queue
  // k: kill every k-th person
  int i;
  Queue myQ;
   
  // 1. Add n persons in the queue
  for (i = 0; i < n; i++)
    myQ.AddQ(i + 1);
    
  // 2. Excute <kill every k-th person> algorithm until (k-1) persons remain in the queue
  int Item;
  int turns = 0;
  while (myQ.Count() >= k) {
    turns++;
    Item = myQ.DeleteQ();
    if (turns % k != 0)
      myQ.AddQ(Item);
  }
  
  // 3. Report the (k-1) persons
  myQ.Print();
}
```
