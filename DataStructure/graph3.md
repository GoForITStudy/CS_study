### 5. 위상 정렬(topological sort)

방향 그래프에서 간선으로 주어진 정점 간 선후관계를 위배하지 않도록 나열하는 정렬, 순서대로 수행해야 할 때 사용한다.(선형 순서가 존재)

***위상 정렬 용어들**

- 진입 간선(incomming Edge) : 들어오는 간선

- 진입차수(indegree) : 자신에게 들어오는 간선의 수

- 진출 간선(out going Edge) : 나가는 간선

- source(src) : 진입 간선이 없고, 진출 간선만 있는 정점, 출발점 

- destination(sink) : 진출 간선이 없고, 진입 간선만 있는 정점, 도착점

*위상정렬은 **DAG(Directed Acyclic Graph)**일때만 가능

1) 그래프에 방향성이 있어야 한다.

2) 그래프 내에 사이클이 없어야 한다. 

→ 사이클이 존재하면 주어진 정점 간 선후 관계에 모순이 발생한다. 



그림 출처 : [https://guides.codepath.com/compsci/Topological-Sort](https://guides.codepath.com/compsci/Topological-Sort)

*위상정렬은 큐나 DFS를 이용해 구현한다.

```java

import java.util.*;

//A class to represent a graph object
class TopologicalSort{

	private LinkedList<LinkedList<Integer>> graph;
	

	//constructor
	TopologicalSort(int vertices){
		graph = new LinkedList<LinkedList<Integer>>();
		
		for (int i = 0; i < vertices; i++)
			graph.add(new LinkedList<Integer>());
	}

	// Add edges : 현재 노드에서 인접한 노드들을 추가해준다. 
	//src : 시작점
	//dest 도착점
	void addEdge(int src, int dest) {
		graph.get(src).add(dest);
	}
	
	
	// Perform DFS on the graph and set the departure time of all
	int DFS(int curVertex, boolean[] visited, int[] departure, int level){
		
		// mark the current node as visited
		visited[curVertex] = true;

		// set the arrival time of vertex `v`
		level++;

		for (int u: graph.get(curVertex)){
			if (!visited[u]) {
				level = DFS(u, visited, departure, level);
			}
		}

		// ready to backtrack
		// set departure time of vertex `v`
		departure[level] = curVertex;
		level++;

		return level;
	}

	// Function to perform a topological sort on a given DAG
	public void doTopologicalSort(int n){
		
		//정점들의 순서를 저장할 배열 생성
		int[] departure = new int[2*n];
		Arrays.fill(departure, -1);

		

		// 방문한 노드 기록 배열 생성
		boolean[] visited = new boolean[n];
		
		
		int level = 0;

		// perform DFS on all undiscovered vertices
		for (int i = 0; i < n; i++){
			if (!visited[i]) {
				level = DFS(i, visited, departure, level);
			}
		}

		// Print the vertices in order of their decreasing
		// departure time in DFS, i.e., in topological order
		for (int i = 2*n - 1; i >= 0; i--){
			if (departure[i] != -1) {
				System.out.print(departure[i] + " ");
			}
		}
	}

	public static void main(String[] args){
		TopologicalSort g = new TopologicalSort(7);
		g.addEdge(0, 1);
		g.addEdge(0, 2);
		g.addEdge(6, 1);
		g.addEdge(6, 5);
		g.addEdge(1, 2);
		g.addEdge(1, 5);
		g.addEdge(2, 3);
		g.addEdge(5, 3);
		g.addEdge(5, 4);
		
		
		
		g.doTopologicalSort(7);//6 0 1 5 4 2 3  → 여러 개의 답이 존재할 수 있다. 

	}
}
```



- 시간복잡도
    - 인접리스트 사용시 : O(|V|+|E|)
    - 인접행렬 사용시 : O(|V|^2)

참고자료

실전 알고리즘 강좌([https://www.youtube.com/watch?v=qzfeVeajuyc&t=94s](https://www.youtube.com/watch?v=qzfeVeajuyc&t=94s)) - 동빈나

바킹독의 실전 알고리즘 - BaaakingDog

뇌를 자극하는 알고리즘(박상현)