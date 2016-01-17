## Tree or Graph Traversal

### Breadth First Search
* 최단 경로 찾기에 용이함.
* Use Queue
* 경로가 매우 길 경우에는 탐색 가지가 급격히 증가함에 따라 보다 많은 기억 공간을 필요로 하게 된다.
* 해가 없으면 무한 그래프는 해를 찾지도 끝내지도 못한다.
* 해가 없으면 유한 그래프는 모든 그래프를 탐색후 실패한다.

### Depth First Search
* 목표 노드가 발견될때까지 자식노드를 먼저 탐색한다.
* use depth bound for infinite loops.
* Use Stack
* 현재 경로상 노드를 저장하면 되므로 공간 소모가 적음
* 목표 노드가 깊은 단계에 있으면 해를 빨리구한다.
* 최단 경로를 구할 수 없음.(그래프상)

## Search 

### Interpolation Search (보간탐색)

이진 탐색의 단점을 보완하기 위해 만든 방법이다.
인간이 전화 번호 부의 이름을 보고 앞쪽부터 탐색하는 것에 기반한 탐색 방법이다. 
정렬된 배열일때 적용이 가능하다.
접근할 중앙값 계산을 

### 성능

평균 수행시간은 O(log(log(n))), Worst case는 O(n) 정도가 걸린다.
각 요소간 차이가 비슷한 경우엔 최적의 성능이, 키 값이 극적으로 증가하면 O(n)이 걸린다.


### 중앙값 구하기
* 아래처럼 인덱스를 찾아려는 키가 있다하자. key - 최소값을 최대값-최소값의 차로 나눈 비율로 만든다.
* 그리고 인덱스와의 비율을 계산해서 대략적인 위치를 찍는 방식이다. 


 비율로 계산하여 index를 구하는 방식이다.

```
int mid = low + ( high - low ) * ((key - list[low]) / (list[high] - list[low]))
```

### 참고

[https://en.wikipedia.org/wiki/Interpolation_search]
[https://en.wikipedia.org/wiki/Breadth-first_search]
[https://en.wikipedia.org/wiki/Depth-first_search]
