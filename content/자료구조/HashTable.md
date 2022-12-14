# HashTable

<br/>

## 해시 테이블(Hash Table)
해시 테이블은 **무한에 가까운 데이터들을 유한한 개수의 해시 값으로 매핑한 테이블**.

### 해시 테이블의 특징
- (key, value)로 데이터를 저장하는 자료구조 중 하나.
- 내부적으로 배열을 사용하여 데이터를 저장하기 때문에 빠른 검색 속도를 가짐.
- key 값에 해시 함수를 적용해 고유한 index를 생성하여 그 index에 저장된 값을 꺼내오는 구조. 
- 이를 통해 작은 크기의 캐시 메모리로도 프로세스를 관리하도록 할 수 있음.
- 고유한 index로 값을 조회하기 때문에 접근, 탐색, 삽입, 삭제 시 평균적으로 O(1)의 시간 복잡도를 가짐. (특정 데이터가 저장되는 인덱스는 그 데이터만의 고유한 위치이기 때문에, 삽입 연산 시 다른 데이터의 사이에 끼어들거나, 삭제 시 다른 데이터로 채울 필요가 없으므로 연산에서 추가적인 비용이 없도록 만들어진 구조)
- 하지만 해시 충돌이 발생한 경우, 충돌된 index값에 연결된 데이터들을 조회하게 되기 때문에 최악의 경우 O(N)까지 증가할 수 있음.

### 해시 테이블의 시간복잡도
|경우|접근(read)|탐색(search)|삽입(insert)|식제(delete)|
|:---|:---:|:---:|:---:|:---:|
|평균|$O(1)$|$O(1)$|$O(1)$|$O(1)$|
|최악|$O(n)$|$O(n)$|$O(n)$|$O(n)$|

<br/>

## 해시 함수(Hash Function)
- 고유한 인덱스 값을 생성하는 알고리즘을 해시 함수(hash function)라고 하고, 이 함수에 의해 반환된 데이터의 고유 숫자 값은 hashcode.
- 저장되는 값들의 key 값을 해시 함수를 통해서 작은 범위의 값들로 바꿔줌.
- 해시 함수를 통해서 동일한 인덱스 값이 도출되게 되어, 서로 다른 두 개의 키가 같은 인덱스로 hashing되면서 복수 개의 데이터가 하나의 테이블에 존재하게 되는 것을 해시 충돌(collision)이라고 함.

### 좋은 해시 함수의 조건
충돌이 많아질 수록 탐색에 필요한 시간 복잡도가 O(1)에서 O(n)에 가까워짐. 따라서 좋은 해시 함수를 선택하는 것은 해시 테이블의 성능 향상에 필수적.
- 좋은 해시 함수는 키의 일부분을 참조하여 해시 값을 만들지 않고, 키 전체를 참조하여 해시 값을 만들어 냄. 
- 해시 함수를 무조건 1:1로 만드는 것보다 충돌(collision)을 최소화하는 방향으로 설계하고 발생하는 충돌에 대비해 어떻게 대응할 것인가가 더 중요.
- 1:1 대응이 되도록 만드는 것이 거의 불가능하기도 하지만 그런 해시 함수를 만들어봤자 array와 다를바 없고 메모리를 너무 차지하게 됨.
- hashing된 인덱스에 이미 다른 값이 들어 있다면 새 데이터를 저장할 다른 위치를 찾은 뒤에 저장하는 충돌 해결이 필수.

<br/>

## 해시 충돌 해결(Resolve Conflict)
충돌 해결 방법은 기본적으로 두 가지가 있음. 이외에는 두 가지 방법을 응용한 방법들.
### Open Address 방식(개방 주소법)
해시 충돌이 발생하면 (삽입하려는 해시 버킷이 이미 사용 중이면) **다른 해시 버킷에 해당 자료를 삽입하는 방식**.<br/>
공개 주소 방식이라고도 불리는 이 알고리즘은 충돌이 발생하면 데이터를 저장할 장소를 찾아 헤맴.<br/> Worst Case의 경우 비어있는 버킷을 찾지못하고 탐색을 시작한 위치까지 되돌아 올 수 있음.<br/><br/>
새 버킷 할당 방식 :<br/>
(1) **Linear Probing** : 순차적으로 탐색하며 비어있는 버킷을 찾을 때까지 계속 진행된다.<br/>
(2) **Quadratic probing** : 2차 함수를 이용해 탐색할 위치를 찾는다.<br/>
(3) **Double hashing probing** : 하나의 해쉬 함수에서 충돌이 발생하면 2차 해쉬 함수를 이용해 새로운 주소를 할당한다. 위 두 가지 방법에 비해 많은 연산량을 요구하게 된다.

### Separate Chaining 방식 (분리 연결법)
일반적으로 Open Addressing은 Separate Chaining 보다 느림.<br/> 
Open Addressing 의 경우 해시 버킷을 채운 밀도가 높아질수록 Worst Case 발생 빈도가 더 높아지기 때문.<br/> 
반면 Separate Chaining 방식의 경우 해시 충돌이 잘 발생하지 않도록 보조 해시 함수를 통해 조정할 수 있다면 Worst Case 에 가까워 지는 빈도를 줄일 수 있음.<br/> 
Java 7 에서는 Separate Chaining 방식을 사용하여 HashMap 을 구현하고 있음.<br/><br/>
두 가지 구현 방식이 존재 :<br/>

(1) **연결 리스트를 사용하는 방식(Linked List)** :<br/> 
각각의 버킷들을 연결리스트(Linked List)로 만들어 충돌이 발생하면 해당 버킷의 list에 추가하는 방식<br/> 
연결 리스트의 특징을 그대로 이어받아 삭제 또는 삽입이 간단함.<br/> 
하지만 단점도 그대로 물려받아 작은 데이터들을 저장할 때 연결 리스트 자체의 오버헤드가 부담이 됨.<br/>
버킷을 계속해서 사용하는 Open Address 방식에 비해 테이블의 확장을 늦출 수 있음.<br/>

(2) **Tree 를 사용하는 방식 (Red-Black Tree)** :<br/>
 기본적인 알고리즘은 Separate Chaining 방식과 동일하며 연결 리스트 대신 트리를 사용하는 방식.<br/>
 연결 리스트를 사용할 것인가와 트리를 사용할 것인가에 대한 기준은 하나의 해시 버킷에 할당된 key-value 쌍의 개수.<br/> 
 데이터의 개수가 적다면 연결 리스트를 사용. 트리는 기본적으로 메모리 사용량이 많기 때문.<br/>
 데이터 개수가 적을 때 Worst Case를 살펴보면 트리와 연결 리스트의 성능상 차이가 거의 없음.<br/>
 데이터의 개수가 많다면 트리 사용.<br/><br/>
연결 리스트와 트리의 기준 : <br/> 

하나의 해시 버킷에 할당된 key-value 쌍의 개수가 **6개일 경우 연결 리스트**를 사용하고, **8개일 경우 트리**를 사용.<br/>
6과 8로 잡은 것은 변경하는데 소요되는 비용을 줄이기 위함.<br/>
기준이 6과 7이라면, 해시 버킷에 6개의 key-value 쌍이 들어있다가 하나의 값이 추가되었을 경우,  자료구조를 연결 리스트에서 트리로 변경해야 하고, 이후 바로 하나의 값이 삭제된다면 다시 트리에서 연결 리스트로 자료구조를 변경해야 함.<br/>
따라서 각각 자료구조로 넘어가는 기준이 1 이라면 Switching 비용이 너무 많이 필요. 그래서 2라는 여유를 남겨두고 기준을 잡아준 것. <br/>
따라서 데이터의 개수가 6 개에서 7 개로 증가했을 때는 연결 리스트의 자료구조를 취하고 있을 것이고 8 개에서 7 개로 감소했을 때는 트리의 자료구조를 취하고 있을 것.

### Open Address vs Separate Chaining
두 방식 모두 Worst Case 에서 O(n).<br/>
하지만 Open Address방식은 연속된 공간에 데이터를 저장하기 때문에 Separate Chaining에 비해 캐시 효율이 높음.<br/> 
따라서 데이터의 개수가 충분히 적다면 Open Address방식이 Separate Chaining보다 더 성능이 좋음.<br/> 
또한 Separate Chaining 방식에 비해 Open Address방식은 버킷을 계속해서 사용함.<br/> 
따라서 Separate Chaining 방식은 테이블의 확장을 보다 늦출 수 있음

### 보조 해시 함수
보조 해시 함수(supplement hash function)의 목적은 key의 해시 값을 변형하여 해시 충돌 가능성을 줄이는 것.<br/> 
Separate Chaining 방식을 사용할 때 함께 사용되며 보조 해시 함수로 Worst Case 에 가까워지는 경우를 줄일 수 있음.

<br/>

## 해시 버킷 동적 확장(Resize)
해시 버킷의 개수가 적다면 메모리 사용을 아낄 수 있지만 해시 충돌로 인해 성능상 손실 발생함.<br/>
그래서 HashMap은 key-value쌍 데이터 개수가 일정 개수 이상이 되면 해시 버킷의 개수를 두 배로 늘림.<br/> 
이렇게 늘리면 해시 충돌로 인한 성능 손실 문제를 어느 정도 해결 가능.<br/> 
해시 버킷 크기를 두 배로 확장하는 임계점은 현재 데이터 개수가 해시 버킷의 개수의 75%가 될 때.<br/> 
0.75라는 숫자는 load factor 라고 불림.