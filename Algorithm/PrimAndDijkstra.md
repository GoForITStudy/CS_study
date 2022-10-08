# BFS 그리고 다익스트라와 프림

# BFS

![image.gif](./images/bfs.gif)

- 기본적으로 BFS는 방문한 노드들을 차례로 저장한 후 꺼낼 수 있는 자료 구조인 큐(Queue)를 사용한다.
    - 즉, 선입선출(FIFO) 원칙으로 탐색

```python
def iterative_bfs(self, v):
    discovered = [v]
    queue = [v]

    while queue:
        v = queue.pop(0)
				# 정점 v의 인접 정점 순회
				for w in self.graph[v]:
            if w not in discovered:
                queue.append(w)
                discovered.append(w)
    return discovered
```

# 최단 경로 문제

- 최단 경로 탐색 알고리즘은 여러가지가 있지만, 우선은 상대적으로 많이 쓰이고, 난이도가 그렇게 어렵지 않은 알고리즘으로 다익스트라, 벨만-포드, 플로이드, BFS를 이용한 알고리즘이 존재한다.

## 가중치가 없는 (혹은 모두 동일한) 그래프

- BFS를 이용해 문제를 해결한다.
    
    ![https://blog.kakaocdn.net/dn/tTymq/btqPnayNKwK/oRVFeA6Jjkc4rE6llc4rsk/img.gif](https://blog.kakaocdn.net/dn/tTymq/btqPnayNKwK/oRVFeA6Jjkc4rE6llc4rsk/img.gif)
    

BFS 예시, 출처 : https://victorqi.gitbooks.io/swift-algorithm/content/breadth-first_search_bfs.html

## 가중치가 1보다 큰 그래프

- **다익스트라**를 활용한다.
    - `한 정점(노드)에서 다른 정점까지의 최단 경로`를 구하는 알고리즘 중 하나이다.
    - 이 과정에서 특정한 도착 정점 만 아니라 **모든 다른 정점까지 최단 경로**를 모두 찾게 된다.
    - 참고로 그래프 알고리즘 중 최소 비용을 구하는 데는 다익스트라 알고리즘 외에도 벨만-포드 알고리즘, 프로이드 워샬 알고리즘 등이 있다.
- (음수가 존재할 때는 벨만 포드 알고리즘 활용)
    
    ![https://blog.kakaocdn.net/dn/cofnIo/btqEuY3ZsSW/TnQX9kZ9RpyYtvElsrkMz1/img.gif](https://blog.kakaocdn.net/dn/cofnIo/btqEuY3ZsSW/TnQX9kZ9RpyYtvElsrkMz1/img.gif)
    

```c
Dijkstra(G, r)
// G = (V, E): 주어진 그래프
// r: 시작 정점
{
    S ← Ø: // S: 방문한 정점 집합

    for each u ∈ V
        dist[u] ← ∞; // 거리 배열 초기화
    
    d[r] ← 0; // 시작 정점 거리 0

    while(S≠V) { // n회 순환
        u ← extractMin(V-S, dist); // 최소 힙으로 구현할 수 있다.
        s ← S ∪ {u}; // 방문 정점으로 추가
        
				for each v ∈ L(u) // L(u) : 정점 u의 인접 정점 집합
						
						// v는 아직 방문전 노드이며, 새로 계산된 거리가 더 짧다면,
            if(v ∈ V-S and dist[u] + w(u, v) < dist[v]) then {
                dist[v] ← dist[u] + w(u, v);// 중요한 차이점 -> 직전 노드까지의 거리도 더한다.
            }
    }
}

extractMin(Q, d[]){
    집합 Q에서 d값이 가장 작은 정점 u를 반환한다;
}
```

### 다익스트라 알고리즘 시간 복잡도

- 정점마다 인접한 간선을 모두 검사하는 작업 : O(|E|)
- 우선순위 큐에 원소를 넣고 삭제하는 작업 : O(|E| log |E|)
- O(|E| log |V|)

# 최소 신장 트리

- 신장 트리에서 최소 비용을 구하는 문제이다.
    - 프림 혹은 크루스칼 알고리즘을 이용해 문제를 해결한다.

![https://onepwnman.github.io/assets/images/mst/prim.gif](https://onepwnman.github.io/assets/images/mst/prim.gif)

```c
Prim(G, r)
// G = (V, E): 주어진 그래프
// r: 시작 정점
{
    S ← Ø: // S: 방문한 정점 집합
    
    for each u ∈ V
        dist[u] ← ∞; // 거리 배열 초기화
    
    d[r] ← 0; // 시작 정점 거리 0

    while(S≠V) { // n회 순환
        u ← extractMin(V-S, dist);
        s ← S ∪ {u};
        for each v ∈ L(u) // L(u) : 정점 u의 인접 정점 집합

            if(v ∈ V-S and w(u, v) < dist[v]) then {
                // 중요한 차이점이 여깄다.
                dist[v] ← w(u, v);
            }
    }
}

extractMin(Q, d[]){
    집합 Q에서 d값이 가장 작은 정점 u를 반환한다;
}
```

- 최소 힙구조를 사용하면 시간 복잡도는 O(E log E)

# 프림과 다익스트라 **차이점**

1. 프림은 다익스트라와 달리 두 노드 사이가 최단거리가 아닐 수도 있다.
2. 프림은 `무향 그래프`에서만 작동하고, 다익스트라는 `무향, 유향 그래프`에서 모두 작동한다.
3. 프림이 다익스트라를, 다익스트라가 프림을 보장해주지 않는다.
    1. 최소 신장 트리가 최단 경로 트리를, 최단 경로 트리가 최소 신장 트리를 보장하지 않는다.

### 참고자료

[https://haningya.tistory.com/116](https://haningya.tistory.com/116)

[최소 신장 트리](https://velog.io/@uoayop/%EC%B5%9C%EC%86%8C-%EC%8B%A0%EC%9E%A5-%ED%8A%B8%EB%A6%AC)

[https://velog.io/@kasterra/핵심-자료구조-그래프-최단-경로-탐색](https://velog.io/@kasterra/%ED%95%B5%EC%8B%AC-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%9E%98%ED%94%84-%EC%B5%9C%EB%8B%A8-%EA%B2%BD%EB%A1%9C-%ED%83%90%EC%83%89)