
# Trie

많은 텍스트 문자열을 쉽게 찾게 만들라고 트리 사용한 자료구조 이다.

## 응용 
* 보통 사전이나 자동완성에서 많이 사용한다.
* full text 검색에 사용
* 정렬 목적으로도 사용한다. pre-order 순회일때 사용한다.
* 바이너리 검색 트리 대용으로도 사용한다.


![Trie 예제](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/250px-Trie_example.svg.png)


## 성능 

* Trie는 O(M) 만큼 걸리게 만들 수 있다.(hashMap 등을 사용하면 말이지)
* 공간 복잡도는 최악에 경우엔 길이 1개마다 늘어날때 문자 개수 만큼 공간을 차지 할 수 있다.

## 특징 

* 키워드 정보는 제일 마지막에만 있다.
* 루트 노드는 보통 비워둔다.
* 상대적으로 데이터를 많이 쓸 수 있다.
* 문자를 Integer 키로 바꿔 만들 수 있으므로 성능성 크게 나쁘지 않다. 
* 실제 데이터는 마지막 노드에서만 저장하고 있다. 

## 개선책

* 위에 처럼 대충 구현 했다간 공간상으로 엄청나게 낭비가 되게 된다. 그래서 radix tree를 사용한다. 
* radix trie : 만약 특정노드의 자손이 하나밖에 없다면 그 노드는 자식노드와 결합된다는 것이다. 성능은 O(N) 정도가 필요하다. (삽입 삭제 찾기할때 방식이 조금 바뀐다.)

![예제](https://upload.wikimedia.org/wikipedia/commons/thumb/6/63/An_example_of_how_to_find_a_string_in_a_Patricia_trie.png/220px-An_example_of_how_to_find_a_string_in_a_Patricia_trie.png)

