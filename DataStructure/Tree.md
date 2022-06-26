# Tree

  트리란, **계층적 관계 (hierarchical realationship)** 을 표현하는 자료구조이다. 트리는 비선형 자료구조이고, 트리가 표현 할 수 있는 것들은 컴퓨터의 디렉토리 구조, 기업의 조직도들을 예시로 들 수 있다. 


## 0. 왜 Tree를 이용할까?

1. 계층적 관계를 나타내는 데 탁월해서
2. 탐색에 용이해서 (연결 리스트보다 빠르고, 배열보다는 느리다)
3. 삽입 삭제에 용이해서 (배열보다는 빠르고, 정렬되지 않은 연결 리스트보다는 느리다)
4. 연결 리스트나 배열과는 다르게, 노드의 최대 개수 제한이 없다 (노드가 포인터를 이용하여 연결되기 때문)



## 1. Tree란

  트리는 배열, 연결 리스트, 스택과 큐와는 다르게 비선형 자료구조에 속한다. 트리의 특징은 아래와 같다.

1. 하나의 루트 노드를 갖는 자료 구조
2. 루트 노드는 0개 이상의 자식 노드를 가진다
3. 그 자식 노드 또한 0개 이상의 자식 노드를 갖고 있고, 이는 반복적으로 정의된다
4. 트리에는 사이클이 존재할 수 없다
5. 큰 트리는 작은 트리로 구성된다. 큰 트리에 속하는 작은 트리를 subtree 라고 한다.

<img src="https://github.com/yeGenieee/TIL/blob/master/data-structure/image/tree.png" alt="image-20210725202050595" style="zoom:50%;" />

*[출처] :https://adrianmejia.com/data-structures-for-beginners-trees-binary-search-tree-tutorial/*



먼저, 트리와 관련된 기본적인 용어를 알아보자.

#### Node (노드)

- 트리의 구성요소 ex) A, B, C, ... J 와 같은 요소들

#### Edge (간선)

- 노드와 노드를 연결하는 연결선

#### Root node (루트 노드)

- 트리 구조에서 최상위에 존재하는 노드 ex) A

#### Leaf node (terminal node, 단말 노드)

- 아래로 또 다른 노드가 연결되어 있지 않은 노드 ex) E, F, G, I, J

#### Internal node (nonterminal node, 내부 노드)

- 단말 노드를 제외한 모든 노드 ex) A, B, C, D, H

#### Parent

- 특정 노드의 위에 위치한 노드

#### Child

- 특정 노드의 바로 아래에 있는 노드

#### Sibling

- 특정 노드와 계층이 같은 노드

#### Depth = Height

- 트리의 높이 (3)



### 트리의 구현

##### 트리의 노드

```java
class Node {
	int key;
	Node left, right;
	
	public Node(int item) {
		key = item;
		left = right = null;
	}
}
```

```java
class Node {
	public String name;
	public Node[] children;
}
```



## 2. Binary Tree

  Binary Tree, 이진 트리란 각 노드가 **최대 두 개의 자식을 갖는 트리를** 말한다. 모든 이진 트리는 아래의 조건을 만족한다.

1. 루트 노드를 중심으로 두 개의 서브 트리로 나누어진다
2. 나뉘어진 두 서브 트리도 모두 이진 트리여야 한다



그렇다면, 아래 트리도 이진 트리라고 할 수 있을까?

<img src="https://github.com/yeGenieee/TIL/blob/master/data-structure/image/binary_tree.png" alt="image-20210725215759400" style="zoom:50%;" />

*[출처] : https://www.geeksforgeeks.org/applications-of-tree-data-structure/*

- 노드가 위치할 수 있는 곳에 노드가 존재하지 않는다면, 공집합 (empty set) 노드가 존재하는 것으로 간주하여, 공집합 노드도 이진 트리의 판단에 있어서 노드로 인정한다. 따라서 위의 트리도 이진 트리가 된다



### 이진 트리 종류

<img src="https://github.com/yeGenieee/TIL/blob/master/data-structure/image/binary_tree_type.png" alt="image-20210725233602325" style="zoom: 33%;" />

*[출처] : https://towardsdatascience.com/5-types-of-binary-tree-with-cool-illustrations-9b335c430254*

#### 1. Full Binary Tree (포화 이진 트리)

- 모든 레벨이 꽉 찬 이진 트리

#### 2. Complete Binary Tree

- 차곡차곡 빈 틈 없이 노드가 채워진 이진 트리
  - 빈틈 없이 노드가 채워졌다의 의미
    - 노드가 위에서 아래로, 그리고 왼쪽에서 오른쪽의 순서대로 채워짐



## 3. Binary Search Tree

Binary Search Tree란, 이진 탐색 트리의 특별한 형태이다. 각 노드의 값들은 무조건 해당 노드의 왼쪽 서브 트리보다는 크고, 오른쪽 서브 트리보다는 작아야 한다.

1. 트리의 각 노드의 값은 왼쪽 서브 트리에 저장된 어떠한 값보다도 커야한다.
2. 트리의 각 노드의 값은 오른쪽 서브 트리에 저장된 어떠한 값보다도 작아야 한다.
3. <img src="https://github.com/yeGenieee/TIL/blob/master/data-structure/image/BST.png" alt="image-20210725233409729" style="zoom:50%;" />

*[출처] : https://leetcode.com/explore/learn/card/introduction-to-data-structure-binary-search-tree/140/introduction-to-a-bst/1017/*



## 4. Tree Traversal (Inorder, Preorder, Postorder)

  이진 트리의 순회 방법에는 세 가지가 있다. 모두 재귀적인 방법이다.

1. 중위 순회 (Inorder Traversal) : 루트 노드를 중간에 !
2. 전위 순회 (Preorder Traversal) : 루트 노드를 먼저 !
3. 후위 순회 (Postorder Traversal) : 루트 노드를 마지막에 !

이를 정하는 기준은 **루트 노드를 언제 방문하느냐** 이다. 

<img src="https://github.com/yeGenieee/TIL/blob/master/data-structure/image/traversal.png" alt="image-20210725232025438" style="zoom: 33%;" />

*[출처] : https://medium.com/@konduruharish/binary-search-tree-in-typescript-and-c-25fa5107cc5d*



#### 세 가지 순회 방법을 재귀적으로 구현만 하면 된다

### 1. Inorder - 중위 순회

1. 왼쪽 서브트리를 순회
2. **루트를 방문**
3. 오른쪽 서브트리를 순회

```java
void printInorder(Node node) {
	if (node == null) {
		return;
	}
	
	printInorder(node.left); // 왼쪽 서브트리 순회
	System.out.print(node.key + " "); // 루트 방문
	printInorder(node.right); // 오른쪽 서브트리 순회
}
```

- 방문 순서 : 1 3 5 6 7 9 11



### 2. Preorder - 전위 순회

1. **루트를 방문**
2. 왼쪽 서브트리를 순회
3. 오른쪽 서브트리를 순회

```java
void printPreorder(Node node) {
	if (node == null) {
		return;
	}
	
	System.out.print(node.key + " "); // 루트 방문
	printPreorder(node.left); // 왼쪽 서브트리 순회
	printPreorder(node.right); // 오른쪽 서브트리 순회
}
```

- 방문 순서 : 6 3 1 5 9 7 11



### Postorder - 후위 순회

1. 왼쪽 서브트리를 순회
2. 오른쪽 서브트리를 순회
3. **루트를 방문**

```java
void printPostorder(Node node) {
	if (node == null) {
		return;
	}
	
	printPostorder(node.left); // 왼쪽 서브트리 순회
	printPostorder(node.right); // 오른쪽 서브트리 순회
	System.out.print(node.key + " "); // 루트 방문
}
```

- 방문 순서 : 1 5 3 7 11 9 6


 

## Reference

- https://leetcode.com/explore/learn/card/data-structure-tree/
- https://leetcode.com/explore/learn/card/introduction-to-data-structure-binary-search-tree/
- 윤성우, 『윤성우의 열혈 자료구조』, 오렌지미디어(2013), p.290 ~ p.319
- 게일 라크만 맥도웰, 『코딩 인터뷰 완전 분석』, 프로그래밍인사이트(2019), p.144 ~ p.145
- https://www.geeksforgeeks.org/binary-tree-set-1-introduction/
- https://www.geeksforgeeks.org/tree-traversals-inorder-preorder-and-postorder/
- https://adrianmejia.com/data-structures-for-beginners-trees-binary-search-tree-tutorial/
- https://medium.com/@konduruharish/binary-search-tree-in-typescript-and-c-25fa5107cc5d
- https://towardsdatascience.com/5-types-of-binary-tree-with-cool-illustrations-9b335c430254
