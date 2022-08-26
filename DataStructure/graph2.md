### 4. 그래프 탐색 - DFS & BFS

그래프의 모든 정점들을 특정한 순서에 따라 방문하는 알고리즘들

**깊이 우선 탐색(Depth First Search)** 

“더 나아갈 길이 보이지 않을 때까지 깊이 들어간다.”

→ 방문할 때 **깊이**를 우선으로 방문하는 알고리즘

*스택을 이용해 구현

*재귀함수를 이용해 구현

<구현방법>

1. 탐색 시작 노드를 스택에 삽입하고 방문처리한다. 
2. 스택의 최상단 노드에 방문하지 않은 인접한 노드가 하나라도 있으면, 그 노드를 스택에 넣고 방문처리한다. 방문하지 않은 인접 노드가 없으면, 스택에서 최상단 노드를 꺼낸다. 
3. 노드에 더 이상 방문하지 않은 인접 노드가 없으면, 이전 노드로 돌아가서 2를 수행
4. 이전 노드로 돌아가도 더 이상 방문할 이웃이 없다면 탐색 종료



```java
import java.util.*;

class DFS {
	private LinkedList<LinkedList<Integer>> graph;
	private boolean visited[];

	// Graph constructor
	DFS(int vertices) {
		graph = new LinkedList<LinkedList<Integer>>();
		visited = new boolean[vertices];

		for (int i = 0; i < vertices; i++)
			graph.add(new LinkedList<Integer>());
	}

	// Add edges : 현재 노드에서 인접한 노드들을 추가해준다. 
	//src : 시작점
	//dest 도착점
	void addEdge(int src, int dest) {
		graph.get(src).add(dest);
	}

	// DFS algorithm
	void DFS(int curVertex) {
		visited[curVertex] = true;
		System.out.print(curVertex + " ");

		for (int i = 0; i < graph.get(curVertex).size(); i++) {
			int adjVertex = graph.get(curVertex).get(i);
			if (!visited[adjVertex]) DFS(adjVertex);
		}
	}

	public static void main(String args[]) {
		DFS g = new DFS(5);

		g.addEdge(0, 1);
		g.addEdge(0, 2);
		g.addEdge(0, 3);
		g.addEdge(1, 0);
		g.addEdge(1, 2);
		g.addEdge(2, 0);
		g.addEdge(2, 1);
		g.addEdge(2, 4);
		g.addEdge(3, 0);

		System.out.println("Following is Depth First Traversal");

		g.DFS(0);//0 1 2 4 3
		//g.DFS(1);//1 0 2 4 3
	}
}
```

- 시간복잡도
    - 인접리스트 사용시 : O(|V|+|E|)
    - 인접행렬 사용시 : O(|V|^2)

**너비 우선 탐색(Breadth First Search)**

깊이가 낮은 정점들부터 탐색 후, 점점 더 깊은 정점들을 방문하는 방식

→ 방문할 때 **너비**를 우선으로 방문하는 알고리즘

*다익스트라의 최단 거리 알고리즘이나 프림의 최소 스패닝 트리 알고리즘 등이 너비 우선 탐색을 골격으로 하고 있다.  

*큐 이용

<구현방법>

1. 탐색 시작 노드를  큐에 삽입하고 방문 처리를 한다.
2. 큐에서 노드를 꺼낸 뒤에 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리한다. 
3. 큐가 비게 되면 탐색 종료. 큐가 빌 때까지 2 반복



```java

import java.util.*;

class BFS {
	private LinkedList<LinkedList<Integer>> graph;
	private boolean visited[];

	// Graph constructor
	BFS(int vertices) {
		graph = new LinkedList<LinkedList<Integer>>();
		visited = new boolean[vertices];

		for (int i = 0; i < vertices; i++)
			graph.add(new LinkedList<Integer>());
	}

	// Add edges
	//src : 시작점
	//dest 도착점
	void addEdge(int src, int dest) {
		graph.get(src).add(dest);
	}

	// DFS algorithm
	void BFS(int curVertex) {
		Queue<Integer> queue = new LinkedList<>();
		
		visited[curVertex] = true;
		queue.offer(curVertex);
		
		while(!queue.isEmpty()) {
            int current = queue.poll();
            System.out.print(current + " ");
            
            // 해당 원소와 연결된, 아직 방문하지 않은 원소들을 큐에 삽입
            for(int i = 0; i < graph.get(current).size(); i++) {
                int adjVertex = graph.get(current).get(i);
                if(!visited[adjVertex]) {
                    queue.offer(adjVertex );
                    visited[adjVertex ] = true;
                }
            }
        }

		
	}

	public static void main(String args[]) {
		BFS g = new BFS(5);

		g.addEdge(0, 1);
		g.addEdge(0, 2);
		g.addEdge(0, 3);
		g.addEdge(1, 0);
		g.addEdge(1, 2);
		g.addEdge(2, 0);
		g.addEdge(2, 1);
		g.addEdge(2, 4);
		g.addEdge(3, 0);

		System.out.println("Following is Breadth First Traversal");

		g.BFS(0);//0 1 2 3 4
		//g.BFS(1);//1 0 2 4 3
	}
}
```

*깊이 우선 탐색과는 달리 너비 우선 탐색에서는 발견과 방문이 같지 않다. 

- 시간복잡도
    - 인접리스트 사용시 : O(|V|+|E|)
    - 인접행렬 사용시 : O(|V|^2)

너비 우선 탐색은 주로 최단 거리를 구하는데 사용한다!

사진 출처 [https://www.programiz.com/dsa/graph-bfs](https://www.programiz.com/dsa/graph-bfs)

소스 출처 : 이것이 취업을 위한 코딩 테스트다 with 파이썬(나동빈), [https://www.programiz.com/dsa/graph-bfs](https://www.programiz.com/dsa/graph-bfs)