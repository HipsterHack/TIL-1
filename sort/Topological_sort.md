# Topological sort

한국 말로는 위상정렬이라 부른다.

## 특징
* 여러 일들에 순서가 정해져 있을 때 순서에 맞게끔 나열하는 것
* 유향 그래프의 꼭짓점들을 변의 방향을 거스르지 않도록 나열하는 것
* 다양한 규칙을 적용할 수 있음.

![위상 정렬 예시](https://upload.wikimedia.org/wikipedia/commons/thumb/0/08/Directed_acyclic_graph.png/180px-Directed_acyclic_graph.png)

## 케이스 

* 5, 7, 3, 11, 8, 2, 9, 10 (그래프에서 왼쪽에서 오른쪽으로, 위에서 아래로)
* 3, 5, 7, 8, 11, 2, 9, 10 (값이 가장 작은 vertax부터.)
* 5, 7, 3, 8, 11, 10, 9, 2 (링크(edge)가 적은 것부터 정렬)
* 7, 5, 11, 3, 10, 8, 9, 2 (가장 큰 숫자로 연결된 것부터 정렬	)
* 5, 7, 11, 2, 3, 8, 9, 10 (위→아래, 왼쪽→오른쪽 정렬)
* 3, 7, 8, 5, 11, 10, 2, 9 (임의로 정렬)

## 응용
* make파일 만들때 compilation 순서를 지정하는 경우
* 데이터 직렬화
* 링커에서 심볼의 의존성을 체크할 때
* DB에서 외래키를 이용한 데이터를 불러와서 정렬 할때.

## 성능

* BFS를 응용해서 만들면  O(NC^2). 병렬 컴퓨팅을 이용하면  O(log^2 n)의 수행시간이 걸린다.

## Topological Sort 알고리즘
### Kahn's algorithm
* BFS와 비슷함. 시작노드를 찾고 set에 넣는다. 적어도 한개 이상은 되야 한다.

#### Pesudo code

```
L ← Empty list that will contain the sorted elements
S ← Set of all nodes with no incoming edges
while S is non-empty do
    remove a node n from S
    add n to tail of L
    for each node m with an edge e from n to m do
        remove edge e from the graph
        if m has no other incoming edges then
            insert m into S
if graph has edges then
    return error (graph has at least one cycle)
else 
    return L (a topologically sorted order)
```
### 구현

### Tarjan's algorithm
* DFS를 응용한 알고리즘이다.
1. DFS 수행하여, 모든 node의 indegree(진입차수) 계산
2. indegree가 0인 node를 출력한 후 제거
3. 모든 노드가 출력될 때 까지 1), 2)과정 반복
 + indegree : 하나의 노드입장에서 자신에게 들어오는 Edge의 갯수
 
#### Pesudo code

```
L ← Empty list that will contain the sorted nodes
while there are unmarked nodes do
    select an unmarked node n
    visit(n) 
function visit(node n)
    if n has a temporary mark then stop (not a DAG)
    if n is not marked (i.e. has not been visited yet) then
        mark n temporarily
        for each node m with an edge from n to m do
            visit(m)
        mark n permanently
        unmark n temporarily
        add n to head of L
```


# 참고

https://en.wikipedia.org/wiki/Topological_sorting
http://navercast.naver.com/contents.nhn?rid=2871&contents_id=91824
