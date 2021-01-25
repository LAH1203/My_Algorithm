### 목차
> 1. [스택(Stack)의 정의](#스택stack의-정의)
>> 1) [스택의 개념](#스택의-개념)
>> 2) [스택의 자료구조](#스택의-자료구조)
> 2. [스택(Stack)의 연산](#스택stack의-연산)
>> 1) [생성(CreateStack)](#생성createstack)
>> 2) [엠티(IsEmpty)](#엠티isempty)
>> 3) [풀(IsFull)](#풀isfull)
>> 4) [추가(Push)](#추가push)
>> 5) [제거(Pop)](#제거pop)
>> 6) [탑(Top)](#탑top)
>> 7) [비교](#비교)


## 스택(Stack)의 정의

### 스택의 개념
- 원소와 도착 시간의 반대 순서로 저장한 리스트
- 원소의 추가와 제거가 top이라고 불리는 한쪽 끝에서만 일어남
    #### 즉, Last-In First-Out(LIFO) 또는 First-In Last-Out(FILO)인 것
- 원소를 추가하는 연산 : push
- 원소를 제거하는 연산 : pop

### 스택의 자료구조
![Stack](https://user-images.githubusercontent.com/57928612/105667203-30f5ba00-5f1e-11eb-9c14-32936b1fb241.png)
- size
  - 스택에 저장할 수 있는 원소의 수
- List of elements
  - 원소를 저장하는 리스트
- TOP
  - top의 위치를 나타내는 값
```cpp
class Stack {
  int size;
  DataType *Items;
  int TOP;
};
```

## 스택(Stack)의 연산
1. 생성(CreateStack)
    - 지정된 크기의 원소를 저장하는 스택을 할당
    - **O(1)**
2. 엠티(IsEmpty)
    - 스택에 원소가 없으면 True를 리턴
    - **O(1)**
3. 풀(IsFull)
    - 스택에 더 원소를 넣을 수 없으면 True를 리턴
    - **O(1)**
4. 추가(Push)
    - 스택에 새로운 원소를 삽입
    - **O(1)**
5. 제거(Pop)
    - 스택에서 원소를 삭제
    - **O(1)**
6. 탑(Top)
    - 탑에 저장된 원소를 리턴(제거 X)
    - **O(1)**

### 생성(CreateStack)
- size개의 원소를 저장하는 공간을 할당
- TOP를 0으로 초기화
```cpp
void Stack::CreateStack(int _size) {
  size = _size;
  tItem = new Datatype[size];
  TOP = 0;
}
void main() {
  Stack myStack.CreateStack(4);
}
```

### 엠티(IsEmpty)
- 스택이 비어 있으면(empty) True를 리턴
- 스택이 비어 있음 <-> TOP == 0
```cpp
int Stack::IsEmpty() {
  return (TOP == 0);
}
```

### 풀(IsFull)
- 스택에 더 이상 원소를 추가할 수 없으면 True를 리턴
- 더 이상 원소를 추가할 수 없음 <-> TOP == size
```cpp
int Stack::IsFull() {
  return (TOP == size);
}
```

### 추가(Push)
- 스택에 새로운 원소를 삽입하는 연산
- Push는 오직 TOP에서만 수행됨
- 반드시 추가 후에는 TOP를 증가시킬 것!
- 예외
  - Full인 스택에 원소를 추가하는 경우 -> Overflow
```cpp
void Stack::push(Datatype DataItem) {
  // 예외
  if (IsFull())
    printf("Pushing in Full Stack\n");
  Items[TOP] = DataItem;
  TOP++;
}
main() {
  myStack.push("Potato");
}
```

### 제거(Pop)
- 스택에서 원소 하나를 삭제하고 리턴하는 연산
- Pop은 TOP에서 가장 가까운 원소를 제거함
- 반드시 TOP를 1 감소시키고 제거할 것!
- 예외
  - Empty인 스택에서 원소를 제거하는 경우 -> Underflow
```cpp
Datatype Stack::pop() {
  // 예외
  if (IsEmpty())
    printf("Poping from Empty Stack\n");
  TOP--;
  return Items[TOP];
}
main() {
  Data = Stack.pop();
}
```

### 탑(Top)
- TOP에 저장된 원소를 제거하지 않고 리턴하는 연산
- 내부적으로는 TOP-1에 저장된 원소를 리턴함
```cpp
Datatype Stack::Top() {
  return Items[TOP - 1];
}
```

### 비교
![비교](https://user-images.githubusercontent.com/57928612/105668106-0278de80-5f20-11eb-8780-9ecaa980a750.png)
