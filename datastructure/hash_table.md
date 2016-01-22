
# hash table

* 키를 값에 매핑할 수 있는 자료구조. associative array를 구현한다
* hash 함수를 계산해서 인덱스를 만들고 그 인덱스로 bucket 이나 slot에 넣는다.

## Hashing
* 가장 단순한 hashing은 같은값을 쓰는 것이다.
* 좋은 해슁 함수는 고르기 어렵다.
* Perfect hash function (O(1)의 시간이 걸린다. )


## collision 해결법
* 두가지로 나뉜다. separate chaining, Open Addressing

## separate chaining
* 충돌이 발생했을때 리스트를 만드는 방식이다. 사용하는 자료구조론 아래처럼 있다.
 +  Linked List
 +  list head cells
 + other structures(self-balancing tree) : 자바처럼 Red-black BST를 사용하는 방식

## Open Addressing
* 해쉬 테이블 배열내 바로 저장하는 방식. 
* 충돌은 탐사(probe)로 해결한다
 + Linear probe : 순차적으로 탐사
 + Quadratic probing: 2차 함수를 이용해 탐색할 위치는 찾는다
 + Double hashing probing :  해쉬 함수에 의해 충돌이 발생하면 2차 해쉬 함수를 이용해 새로운 주소를 할당하는 방법이다. 연산량이 높고 캐시 효율은 안좋다. 
* load factor(전체 슬롯에서 사용중인 슬롯 비율)에 따라 영향을 많이 받는다.

## 두가지 방식의 비교
+ separate chaining
  -  삭제 작업이 간단하다.
  -  모든 버켓이 사용중이더라도 성능 저하가 더디게 나타나므로 open-addressing 방식에 비해 테이블 확장을 상당히 늦출 수 있다
  - 효과적으로 구현하기 쉽다. 
  - 기본적인 자료구조 정도만 필요하다
  - 테이블이 채워져도 성능 저하가 linear하다.
  - hash 함수의 성능에 영향을 받지 않는다.
  - bucket의 Array가 크고 빈공간이 많으면 공간 효율이 훨 좋다.
+ Open-Addressing
  - 추가 공간요구가 없어 메모리 효율이 좋다 
  - 삽입시 메모리 할당 오버헤드가 없다.
  - 외부 공간을 별도로 필요하지 안흔ㄴ다.
  - 데이터 크기가 적으면 separate chaining보다 성능이 좋다.
  - serialziation이 훨씬 낫다()

## 해쉬의 단점
* pseudo random 위치에 데이터를 저장한다. 그래서 데이터를 정렬된 순서로 접근하면 비용이 비싸다. 
*  locality-of-reference가 취약하다(접근 패턴이 hash value를 이용하기 때문.)


## 성능
* load factor(전체 슬롯에서 사용중인 슬롯 비율)에 따라 영향을 많이 받는다.
* 하지만 특정 퍼센트(예를 들어 10%, 100%)로 확장을 한다고 하자. 그러면 확장이 드물게 발생한다. 
  + lookup에 발생하는 평균적인 시간이 O(1)으로 되는 amortized(평소엔 좋지만 최악의 상황도 있는) 복잡도를 갖게 된다.
* 또한 키 넣을 값보다 버킷이 크면 문제가 없다.

### Big(O)

|      |Average|Worst|
|------|-------|-----|
|Space |O(n)   | O(n)|
|Search|O(1)   | O(n)|
|Insert|O(1)   | O(n)|
|Delete|O(1)   | O(n)|

## 참고

http://d2.naver.com/helloworld/831311

http://egloos.zum.com/sweeper/v/925740
