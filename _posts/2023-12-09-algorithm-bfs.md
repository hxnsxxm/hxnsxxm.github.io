---
title: "너비 우선 탐색(BFS)과 Java 구현 방법"
excerpt: "BFS와 Java 구현 방법을 알아본다."

categories:
  - Algorithm
tags:
  - [algorithm, bfs]

permalink: /algorithm/bfs/

toc: true
toc_sticky: true

date: 2023-12-09
last_modified_at: 2023-12-16
---

## BFS 
> Breadth-First Search의 약자이고, <b>너비 우선 탐색</b>이라고 부른다.  
> Graph에서 인접한 부분을 우선적으로 탐색하는 알고리즘이다.
  
즉, 그래프의 루트 노드(혹은 임의의 노드)에서 시작해서 가까운 노드들을 먼저 방문하고 이후에 멀리 떨어져 있는 노드를 방문하는 순회 알고리즘이다.  
<hr>

## BFS를 사용하는 경우
- 두 노드 사이의 최단 경로
- 임의의 경로

예를 들어서
> 지구상에 존재하는 모든 친구 관계를 그래프로 표현한 후 Sam과 Jeny 사이에 존재하는 경로를 찾는 경우

### BFS의 특징
- BFS는 재귀적으로 동작하지 않는다.
- 어떤 노드를 <b>방문했었는지 여부</b>를 반드시 검사해야 한다.
  - 무한 루프에 빠질 수 있음
- 큐(Queue)를 사용한다.
  - 선입선출(FIFO) 원칙으로 탐색
  - 다음 탐색할 노드들을 저장하기 때문에 DFS에 비해 더 큰 저장공간을 사용한다.

### 복잡도
정점의 개수를 V, 간선의 개수를 E라고 할 때,  

| | 인접 행렬 | 인접 리스트 |  
---|---|---  
| 시간 복잡도 | O(V^2) | O(V+E) |  
| 공간 복잡도 |	O(V^2) |	O(E) |
| A->B 연결여부 확인 |	O(1) <br> graph[x][y] 의 값으로 한번에 확인 | 	O(min(deg(A), deg(B))), 방향성 그래프는 O(deg(A)) <br> graph<x> 의 원소에서 y가 나올때까지 탐색 |
| A에 연결된 정점 확인 |	O(V) |	O(deg(A)) |
| 인접 노드 파악 방법 |	N * N만큼 반복문을 돌아 확인한다. |	각 리스트에 담겨있는 원소를 확인한다. | 


<hr>
## 구현 방법
큐
코드는 아래와 같이 구분할 수 있다.  
> 1. 그래프를 만드는 부분(노드 연결)
> 2. 그래프를 가지고 BFS 돌리는 부분

### 방문처리
위의 주의점에서도 언급했다시피, BFS는 무한루프에 빠질 위험성이 있다. 이를 방지하기 위해서 방문한 노드를 체크하는 작업이 필요하다.  
```java
boolean[] visited = new boolean[V + 1]; // boolean 배열을 만들어서 방문 노드를 체크(true/false)한다.
```

### 그래프 생성을 위한 변수
그래프를 생성하는 방법은 여러 가지가 있다.
```java
// 인접 행렬
int[][] graph;

// 인접 리스트 = 리스트 + 행렬
ArrayList<Integer>[] graph;

// 인접 리스트 = 리스트 + 리스트
ArrayList<ArrayList<Integer>> graph = new ArrayList<>();

// 해시맵 + 리스트
Map<Integer, List<Integer>> graph = new HashMap<>();
```
  
### 입력 형태에 따른 그래프 생성 방법

#### 1) Input : 정점의 개수, 간선의 개수, 연결 노드
```java
4 5 1 // 정점 개수, 간선 개수, 탐색을 시작할 정점
1 2   // 간선이 연결하는 두 정점의 번호들 (양방향 간선)
1 3
1 4
2 4
3 4
```
입력으로 <b>정점의 개수</b>가 오는 경우는 쉽다. 처음부터 노드(정점)의 개수만큼 변수를 할당하여 그래프를 생성할 수 있기 때문이다.  
```java
// 인접 행렬
graph = new int[V + 1][V + 1]; // 노드 번호가 1부터 시작해서 index도 맞춰줌

// 인접 리스트 = 리스트 + 행렬
graph = new ArrayList[V + 1];
for (int i = 0; i <= V; i++) {
    graph[i] = new ArrayList<>();
}

// 인접 리스트 = 리스트 + 리스트
for (int i = 0; i <= V; i++) {
    graph.add(new ArrayList<>());
}
```
이후에 입력되는 연결 노드들을 graph 변수에 저장한다.
```java
// 인접 행렬
for (int v = 0; v < V; v++) { // 간선의 개수만큼 반복
    int out = 노드번호;
    int in = 노드번호;

    graph[out][in] = 1;
    graph[in][out] = 1; // 방향 없는 그래프면 반대 방향도 추가
}

// 인접 리스트 = 리스트 + 행렬
for (int v = 0; v < V; v++) { 
    int out = 노드번호;
    int in = 노드번호;

    graph[out].add(in);
    graph[in].add(out); // 방향 없는 그래프면 반대 방향도 추가
}

// 인접 리스트 = 리스트 + 리스트
for (int v = 0; v < V; v++) { 
    int out = 노드번호;
    int in = 노드번호;

    graph.get(out).add(in);
    graph.get(in).add(out); // 방향 없는 그래프면 반대 방향도 추가
}
```

#### 2) Input : 연결 노드
```java
1 2   // 간선이 연결하는 두 정점의 번호들 (양방향 간선)
1 3
1 4
2 4
3 4
```
연결 노드만 나오는 경우에는 정점의 개수를 알 수 없다. 이 경우에는 `HashMap`을 이용할 수 있다.  
```java
// 노드 연결
for (int[] edge : edges) {
    int out = edge[0];
    int in = edge[1];

    graph.computeIfAbsent(out, k -> new ArrayList<>()).add(in);
    graph.computeIfAbsent(in, k -> new ArrayList<>()).add(out); // 방향 없는 그래프면 반대 방향도 추가
}
```

### 생성한 그래프로 BFS 돌리는 방법
```java
void bfs(int start) {
    // 방문 체크
    boolean[] visited = new boolean[V]; // 정점의 개수

    // 큐 생성
    LinkedList<Integer> queue = new LinkedList<>();

    // 시작 노드에 대해서 초기 작업: 방문 체크, 큐 삽입
    visited[start] = true;
    queue.add(start);

    // 본격적인 bfs: 인접한 노드 방문
    while (!queue.isEmpty()) {
        // 방문한 노드 추출
        int node = queue.poll();
        System.out.println(node + " > ");

        // 인접한 모든 노드를 가져옴
        for (int n : graph.get(node)) {
            if (!visited[n]) {
                visited[n] = true;
                queue.add(n);
            }
        }
    }
}
```
<hr>
<b>Reference</b>  
[[알고리즘] 너비 우선 탐색(BFS)이란](https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html)  
[[파이썬으로 배우는 알고리즘] BFS(너비 우선 탐색)](https://c4u-rdav.tistory.com/29)