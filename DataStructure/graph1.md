### 1. 그래프의 정의

 어떤 자료나 개념을 표현하는 ‘정점의 모음’과 이 정점을 잇는 ‘간선의 모음’ 과의 결합

“정점의 집합을 V, 간선의 집합을 E, 그리고 그래프를 G라고 했을 때, G = (V, E)이다.”

![Untitled](https://github.com/GoForITStudy/CS_study/blob/main/DataStructure/image/graph.jpg)


*V - 정점(vertex/node)

*E - 간선(edge/link) : 정점과 정점이 ‘인접’ 관계에 있음을 나타내는 존재 

*인접(adjacent), 이웃 관계 : 간선으로 연결되어 있는 두 정점

*경로(Path) : 끝과 끝이 서로 연결된 간선들을 순서대로 나열한 것 예) (1, 6)

*사이클 : 어느 경로가 정점 하나를 두 번 이상 거치도록 되어 있는 것 

### 2. 그래프의 종류



1. **무방향 그래프(Undirected graphs)** : 두 관계 사이에 방향이 없다. 싸이클로 볼 수 있음(양방향그래프)
2. **방향 그래프(Directed graphs)** : 각 간선에 방향 존재 추가
    
    *DAG(Directed Acyclic Graph)  → 위상 정렬
    
    1) 그래프에 방향성이 있어야 한다.
    
    2) 그래프 내에 사이클이 없어야 한다. 
    
3. **가중치 그래프(Weighted graphs)** : 각 간선에 가중치라는 실수 속성이 부여 된다. -  최단 거리 문제 (다익스트라, 벨만-포드, 플로이드-워셜), 최소 신장 트리(프림, 크루스칼)

그외 단순 그래프, 이분 그래프 등등,,,,,

### 3. 그래프 표현법

-**인접 행렬(adjacency matrix)** : 정점끼리 인접 관계를 나타내는 행렬

1. 무방향 그래프 : (n,n)을 따라 선을 그어 행렬을 둘로 나눴을 때 인접 행렬이 대각선에 대칭을 이루고 있다.

![Untitled](https://github.com/yeGenieee/TIL/blob/master/data-structure/image/graph2.jpg)

1. 방향 그래프  

![Untitled](https://github.com/yeGenieee/TIL/blob/master/data-structure/image/graph3.jpg)

1. 가중치 그래프 

![Untitled](https://github.com/yeGenieee/TIL/blob/master/data-structure/image/graph4.jpg)

```java
int[][] graph;// 2차원 배열로 표현
graph = new int[정점의 개수 + 1][정점의 개수 + 1];
graph[정점][연결된 정점] = 1;
```

-**인접 리스트(adjacency list)** : 그래프 내의 각 정점의 인접 관계를 표현하는 리스트

1. 무방향 그래프

![Untitled](https://github.com/yeGenieee/TIL/blob/master/data-structure/image/graph5.jpg)

1. 방향 그래프  

![Untitled](https://github.com/yeGenieee/TIL/blob/master/data-structure/image/graph6.jpg)

```java
LinkedList<LinkedList<Integer>> graph;
graph = new LinkedList<LinkedList<Integer>>();
		for(int i = 0; i <= 정점의 개수; i++) {
			graph.add(new LinkedList<Integer>());
		}

graph.get(정점).add(연결된 정점);
```

1. 가중치 그래프 

![Untitled](https://github.com/yeGenieee/TIL/blob/master/data-structure/image/graph7.jpg)

```java
class Edge {
	public int vex;//연결된 정점
	public int cost;//가중치

	Edge(int vex, int cost){
		this.vex = vex;
		this.cost = cost;
	}
}

	static LinkedList<LinkedList<Edge>> graph;
	graph = new LinkedList<LinkedList<Edge>>();

	for(int i = 0; i <= 정점의 개수; i++) {
		graph.add(new LinkedList<Edge>());
	}
	
	graph.get(a).add(new Edge(vex, cost));
```

***인접 행렬과 인접 리스트 비교**

인접 행렬은 두 정점 간의 인접 여부를 배열의 접근만으로 빠르게 알 수 있다는 장점이 있지만 시간복잡도가 ‘정점의 크기 X N^2’(O(|V|^2)가 된다.

인접리스트의 시간복잡도 : O(|V|+|E|)

정점(노드)의 개수가 많아진다면 인접행렬은 비효율적이다.  → 순차 탐색을 하기 때문에 정점이 만개라면 만번을 정점마다 돌아야 하기 때문에!!! 

간선의 개수가 많다면(밀집 그래프) 인접 행렬을, 정점의 개수가 많다면(희소 그래프) 인접 리스트를 사용하는 것이 좋다. 

---

### 참고 자료

뇌를 자극하는 알고리즘(박상현) 

인프런 - 자바 알고리즘 문제풀이(김태원)

헬로우 코딩 그림으로 이해하는 알고리즘(아디트야 바르가바)

알고리즘 문제해결 전략(구종만)

사진 출처 : [https://www.softwaretestinghelp.com/graph-implementation-cpp/](https://www.softwaretestinghelp.com/graph-implementation-cpp/)

