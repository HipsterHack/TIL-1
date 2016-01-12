# Tree

트리는 그래프의 일종이다.
여러 노드가 한 노드를 가리킬 수 없는 구조 이다. 

## 용어
* 최상위 노드를 root 라 부른다.
* A 노드가 B 노드를 가리킬때 B 노드는 자식 노드라고 부르고 A노드는 부모 노드
* 자식이 없는 노드를 leaf Node 라고 부른다.
* Siblings: 같은 parent를 가진 노드 
* height of node : 루트 노드로 부터 현재 노드의 edge 수
* height of tree : 루트노드서 부터 가장 깊은 노드까지 길이 
* degree of tree: 노드의 부속트리 개수 
* Degree: 노드의 subtree 개수
* Depth : 노드의 root로 부터 edge 개수 
* Edge: 두개 노드를 잇는 연결 

## 이 자료구조가 필요한 이유

예를 들면, 계층적인 데이터 형태들은 트리에 저장하면 자연스럽게 표현된다.

1. 회사나 정부의 조직 구조,
2. 나라, 지방, 시군별, 계층적인 데이터의 저장,
3. 인덱스 파일의 생성 등이다(인덱스는 계층적 자료 구조로 검색을 쉽게해준다.)

주로 컴퓨팅에선 3번의 이유로 많이 쓴다. 정렬 트리의 경우엔 탐색 비용이 비교적 싸다.

## 특징 
+ 이 구조를 응용하여 다양한 형태의 트리형 자료구조가 있다.
  - Heap
  - Binary Search Tree
  - Binary Tree : 자식 개수를 2개
  - B-Tree: binary tree의 자식 개수를 2개 이상으로 늘렸음

### Binary Tree
* 두개의 자식 노드를 가지는 트리

#### 종류
* rooted binary tree: 
* full binary tree
* perfect binary tree
* complete binary tree

https://ko.wikipedia.org/wiki/%EC%9D%B4%EC%A7%84_%ED%8A%B8%EB%A6%AC
### Binary Search Tree
* sorted binary trees or ordered binary trees.
* 각 노드에 값이 있고 왼쪽엔 노드보다 작은 값이, 오른쪽엔 노드보다 큰 값이 보통 들어간다. 중복 노드가 있어서는 안된다.

|      |Average  | Worst case|
|------|---------|-----------|
|Space | O(n)	 |  O(n)     |
|Search| O(log n)|	O(n)     |
|Insert| O(log n)|	O(n)     |
|Delete| O(log n)|	O(n)     |


### B-Tree
* 트리 구성을 leaf 에서 루트로 만들어 나간다.
https://ko.wikipedia.org/wiki/B_%ED%8A%B8%EB%A6%AC

### B+Tree
* B+트리는 모든 레코드들이 트리의 가장 하위 레벨에 정렬되어 있다.
* 오직 키들만이 내부 블록에 저장된다

[https://ko.wikipedia.org/wiki/B%2B_%ED%8A%B8%EB%A6%AC]

## 트리 순회 방법 (Tree Traversal)
* 재귀로 구현하면 참 편하다.
* 큐나 스택을 이용해서 구현하면 재귀를 돌지 않아도 된다.

### Preorder Traversal 
* (root, left, right) 의 순서로 순회한다

### Inorder Traversal
*  (left, root, right) 의 순서로 순회한다.

### Postorder Traversal
* (left, right, root) 의 순서로 순회한다.

### Traversal 구현 예제

```
	private static class TreeNode {
		int value;
		TreeNode left;
		TreeNode right;

		public TreeNode() {
		}

		public TreeNode(int value, TreeNode left, TreeNode right) {
			this.value = value;
			this.left = left;
			this.right = right;
		}
	}

	public static void main(String[] args) {
		TreeNode root = new TreeNode();
		root.value = 20;
		root.left = new TreeNode(10, null, new TreeNode(32, null, null));
		root.right = new TreeNode(14, new TreeNode(16, null, null), null);

		printVisitPreorder(root);
		System.out.println("\n");
		printVisitInorder(root);
		System.out.println("\n");
		printVisitPostorder(root);
	}

	public static void printVisitPreorder(TreeNode n) {
		if (n == null) {
			return;
		}
		System.out.print(n.value + " ");
		printVisitPreorder(n.left);
		printVisitPreorder(n.right);
	}

	public static void printVisitInorder(TreeNode n) {
		if (n == null) {
			return;
		}
		printVisitInorder(n.left);
		System.out.print(n.value + " ");
		printVisitInorder(n.right);
	}

	public static void printVisitPostorder(TreeNode n) {
		if (n == null) {
			return;
		}
		printVisitPostorder(n.left);		
		printVisitPostorder(n.right);
		System.out.print(n.value + " ");
	}
```

#### 참고: vector

vector: 연속 메모리 기반 컨테이너. 요소가 추가될때마다 재할당하므로 사용 메모리가 증가한다. (java는 10개씩 증가시킨다.)
* array와 비슷하지만 요소 수가 다이나믹 하게 증가한다. 요소가 용량보다 커지면 array 복사해서 새 메모리에 데이터를 할당한다.



## 참고

(https://en.wikipedia.org/wiki/Tree_(data_structure))
(http://robodream.tistory.com/182)
(http://dblab.duksung.ac.kr/ds/pdf/Chap08.pdf)
