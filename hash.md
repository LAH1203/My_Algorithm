### 목차
> 1. [해시(Hash)의 정의](#해시hash의-정의)
> 2. [해시 함수(Hash function)](#해시-함수hash-function)
>> 1) [자릿수 선택(digit selection)](#자릿수-선택digit-selection)
>> 2) [자릿수 접기(digit folding)](#자릿수-접기digit-folding)
>> 3) [모듈로 연산(modulo function)](#모듈로-연산modulo-function)
> 3. [해시(Hash)의 연산](#해시hash의-연산)
>> 1) [해시 테이블 구조](#해시-테이블-구조)
>> 2) [검색 연산](#검색-연산)
>> 3) [삽입 연산](#삽입-연산)
>> 4) [삭제 연산](#삭제-연산)
> 4. [충돌 및 충돌 해소](#충돌-및-충돌-해소)
>> 1) [열린 어드레싱(open addressing)](#열린-어드레싱open-addressing)
>>> 1. [선형 탐사(linear probing)](#선형-탐사linear-probing)
>>> 2. [제곱 탐사(quadratic probing)](#제곱-탐사quadratic-probing)
>>> 3. [이중 해시(double hash)](#이중-해시double-hash)
>> 2) [닫힌 어드레싱(closed addressing)](#닫힌-어드레싱closed-addressing)
>>> 1. [버켓(bucket)](#버켓bucket)
>>> 2. [별도 체인(separate chain)](#별도-체인separate-chain)
> 5. [성능 비교](#성능-비교)

---

## 해시(Hash)의 정의
### 해시란?
- O(1)의 탐색 시간을 추구하는 자료구조 및 알고리즘
- 자료의 크기에 상관 없이 실시간에 탐색이 실행되어야 하는 경우에 해시를 사용
> <h4>"모든 키의 레코드를 산술 연산에 의해 한 번에 바로 접근할 수 있는 기법"</h4>

### 해시 관련 해시태그<sub>&nbsp;&nbsp;&nbsp;&nbsp;...ㅋㅋㅋ</sub>
```
# 해시 함수(Hash function)
# 해시 인덱스(Hash index)
# 해시 테이블(Hash table)
# 충돌(Collision) & 충돌 해소(Collision resolution)
```

### 해시의 구조
![해시의 구조](https://user-images.githubusercontent.com/57928612/107926852-925af700-6fb9-11eb-815b-942a0fa20c72.png)

---

## 해시 함수(Hash function)
### 자릿수 선택(digit selection)
- 키의 값 중에서 일부 자릿수만 골라내서 인덱스를 생성하는 함수
#### EX)
- 13자리 주민등록번호 중 홀수 자릿수만 선택
- h(8812152051218) = 8112528
##### 충돌 발생하는 경우?
- h(8812152051218) = 8112528
- h(8711142152238) = 8112528

### 자릿수 접기(digit folding)
- 키의 각각의 자릿수를 더해서 인덱스를 생성하는 함수
#### EX)
- h(8812152051218) = 8 + 8 + 1 + 2 + ... + 1 + 8 = 44
##### 충돌 발생하는 경우?
- h(8812152051218) = 8 + 8 + 1 + 2 + ... + 1 + 8 = 44
- h(8713142152208) = 8 + 7 + 1 + 3 + ... + 0 + 8 = 44

### 모듈로 연산(modulo function)
- 키를 해시 테이블의 크기로 나눈 나머지를 인덱스로 생성하는 함수
- h(KEY) = KEY mod TableSize
#### EX)
- h(8812152051218) = 8812152051218 % 100 = 18
##### 충돌 발생하는 경우?
- h(8812152051218) = 8812152051218 % 100 = 18
- h(8713142152218) = 8713142152218 % 100 = 18

---

## 해시(Hash)의 연산
### 해시 테이블 구조
- 인덱스의 하나의 데이터만 저장하는 경우(1 : 1)
```cpp
data_type hash_table[N];
```
- 인덱스에 k개의 데이터를 저장하는 경우(1 : k)
```cpp
data_type hash_table[N][k];
```

### 검색 연산
- 키 -> 해시 인덱스 -> 해시 테이블
- 1 : 1
```cpp
data_type hash_table[N];

data_type search(data_type key) {
  int index = hash_function(key);
  return hash_table[index];
}
```
- 1 : k
```cpp
data_type hash_table[N][k];

data_type search(int index, data_type key) {
  int index = hash_function(key);
  return search_hash_table(index, key);
}

data_type search_hash_table(int index, data_type key) {
  for (int i = 0; i < k; i++) {
    if (hash_table[index][i].key = key)
      return hash_table[index][i];
  }
  return NULL;
}
```

### 삽입 연산
- 키 -> 해시 인덱스 -> 해시 테이블
- 고려해야할 점
  - 해당 공간에 이미 데이터가 존재하거나 빈 공간이 없을 경우 충돌 발생 -> 충돌 해소 필요!
- 1 : 1
```cpp
data_type hash_table[N];

void insert(data_type data) {
  int index = hash_function(data.key);
  // 충돌(이에 관련된 내용은 밑에서 다룸)
  if (collision(index))
    index = collision_resolution(index);
  hash_table[index] = data;
}
```
- 1 : k
```cpp
data_type hash_table[N][k];

void insert(data_type data) {
  int index = hash_function(data.key);
  // 충돌(이에 관련된 내용은 밑에서 다룸)
  if (collision(index))
    index = collision_resolution(index);
  insert_hash_table(index, data);
}

void insert_hash_table(int index, data_type data) {
  for (int i = 0; i < k; i++) {
    if (hash_table[index][i] == NULL) {
      hash_table[index][i] = data;
      return;
    }
  }
}
```

### 삭제 연산
- 키 -> 해시 인덱스 -> 해시 테이블
- 1 : 1
```cpp
data_type hash_table[N];

void remove(data_type key) {
  int index = hash_function(key);
  hash_table[index] = NULL;
}
```
- 1 : k
```cpp
data_type hash_table[N][k];

void remove(data_type key) {
  int index = hash_function(key);
  remove_hash_table(index, key);
}

void remove_hash_table(int index, data_type key) {
  for (int i = 0; i < k; i++) {
    if (hash_table[index][i].key == key)
      hash_table[index][i] = NULL;
  }
}
```

### 각 연산의 시간 복잡도
- 검색 -> O(k) -> O(1)
- 삽입 -> O(k) -> O(1)
- 삭제 -> O(k) -> O(1)

---

## 충돌 및 충돌 해소
### 충돌(Collision)
- 서로 다른 키를 가진 레코드가 해시 함수에 의해 동일한 인덱스로 대응되는 현상
#### EX)
- Hash function
  - H(k) = k mod m, where m = 17
- For k<sub>1</sub> = 18 & k<sub>2</sub> = 171
  - H(k<sub>1</sub>) = 18 mod 17 = 1
  - H(k<sub>2</sub>) = 171 mod 17 = 1
    #### Collision!

### 충돌 해소(Collision solution)
- 충돌이 발생한 레코드를 다른 주소에 저장하는 기법
- 열린 어드레싱(open addressing)
  - 충돌이 발생한 레코드를 저장할 다른 주소를 찾는 방법
  - 레코드의 주소(address)가 변경될 수 있기 때문에 열린 어드레싱(open addressing)이라고 함
  - [선형 탐사(linear probing)](#선형-탐사linear-probing)
  - [제곱 탐사(quadratic probing)](#제곱-탐사quadratic-probing)
  - [이중 해시(double hash)](#이중-해시double-hash)
- 닫힌 어드레싱(closed addressing)
  - 충돌이 발생한 레코드를 동일한 주소에 저장하는 방법
  - 주소가 변경되지 않으므로 닫힌 어드레싱(closed addressing)이라고 함
  - [버켓(bucket)](#버켓bucket)
  - [별도 체인(separate chain)](#별도-체인separate-chain)

### 열린 어드레싱(Open addressing)
#### 선형 탐사(linear probing)
- 충돌이 일어나면 그 다음 빈 곳에 원소를 저장함
- T[h(KEY)]에 값이 이미 존재할 때,
  - T[h(KEY) + 1], T[h(KEY) + 2], ...
- 배열의 끝을 만나면 다시 처음으로 되돌아와서 거기서부터 빈 자리를 찾음
  - T[n - 1]
  - T[0]
- 태그(tag)
  - 선형 탐사에서 다음 원소를 가리키는 포인터
  - 태그의 필요성
    - 다음 자리(h(key) + 1)에 이미 다른 원소가 존재할 때 다음 원소의 위치를 가리킴
    - 중간 원소를 삭제할 때 태그를 수정함
- 단점
  - 클러스터링(clustering)
    - 레코드가 분산되지 않고 군집되는 현상

![선형탐사 예시](https://user-images.githubusercontent.com/57928612/107935055-38136380-6fc4-11eb-8fdc-07244c392e00.png)

#### 제곱 탐사(quadratic probing)
- 충돌이 일어날 때 바로 그 뒤에 넣지 않고 조금 간격을 두고 삽입
- T[h(KEY)]에 값이 이미 존재할 때,
  - T[h(KEY) + 1], T[h(KEY) + 2<sup>2</sup>], ...
- 충돌이 일어날 때 바로 그 뒤에 넣지 않고 조금 간격을 두고 삽입
- 클러스터링을 피하는 것이 가능한가?
  - 연속적인 배열을 피할 수 있음
  - 동일한 키를 가진 레코드는 동일한 자리에 삽입됨
  - 궁극적으로는 클러스터링을 피할 수 없음 -> **2차 클러스터링**

![제곱탐사 예시](https://user-images.githubusercontent.com/57928612/107938332-8dea0a80-6fc8-11eb-80a0-90dc42828080.png)

#### 이중 해시(double hash)
- 제곱 탐사의 단점인 2차 클러스터링을 방지하자는 목적
- 두 개의 해시 함수 h<sub>1</sub>, h<sub>2</sub>를 사용
  - h<sub>1</sub>은 주어진 키로부터 인덱스를 계산하는 해시 함수
  - h<sub>2</sub>는 충돌 시 탐색할 인덱스의 간격(step size)을 의미

![이중해시 예시](https://user-images.githubusercontent.com/57928612/107938938-65aedb80-6fc9-11eb-9ea4-697954895b74.png)

---

### 닫힌 어드레싱(Closed addressing)
#### 버켓(bucket)
- 해시 테이블의 각 원소들이 다시 여러 개의 원소로 이루어짐
- 2차원 배열
- 충돌되는 레코드를 하나의 인덱스에 둠
- 사용되지 않는 배열 공간이 낭비됨

![버켓 예시](https://user-images.githubusercontent.com/57928612/107939121-a6a6f000-6fc9-11eb-8cfd-9c2c94d93fdc.png)

#### 별도 체인(separate chain)
- 해시 테이블의 각 원소들이 연결 리스트를 가리키는 헤드
- 충돌이 일어날 때마다 해당 레코드를 연결 리스트의 첫 위치에 삽입
- 동적 구조(Dynamic structure)

![별도체인 예시](https://user-images.githubusercontent.com/57928612/107939299-de159c80-6fc9-11eb-894d-ebb42d45e715.png)

---

## 성능 비교

![성능비교](https://user-images.githubusercontent.com/57928612/107939437-0b624a80-6fca-11eb-8f3b-f029cb018819.png)
