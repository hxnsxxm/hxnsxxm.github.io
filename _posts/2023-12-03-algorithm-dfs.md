---
title: "깊이 우선 탐색(DFS)과 Java 구현 방법"
excerpt: "DFS와 Java 구현 방법을 알아본다."

categories:
  - Basic
tags:
  - [algorithm, dfs]

permalink: /algorithm/basic/dfs/

toc: true
toc_sticky: true

date: 2023-12-03
last_modified_at: 2023-12-16
---

## DFS 
> Depth-First Search의 약자이고, <b>깊이 우선 탐색</b>이라고 부른다.  
> Graph에서 깊은 부분을 우선적으로 탐색하는 알고리즘이다.
  
즉, 그래프의 루트 노드(혹은 임의의 노드)에서 시작해서 다음 분기(branch)로 넘어가기 전에 해당 분기를 완벽하게 탐색하는 방법이다.  
<hr>
## DFS를 사용하는 경우
- 각 경로의 특징을 저장해야 할 때
- 그래프의 규모가 클 때(비교적 공간을 적게 사용함)

### 주의점
다만, 주의할 점은 자기 자신을 호출하는 순환 알고리즘의 형태를 가지고 있기 때문에 어떤 노드를 방문했었는지 여부를 반드시 검사해야 한다. 그렇지 않으면 <b>무한루프</b>에 빠질 위험성이 있다.

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
스택 혹은 재귀(일반적으로 코딩테스트에서는 재귀를 씀)  
코드는 아래와 같이 구분할 수 있다.  
> 1. 그래프를 만드는 부분(노드 연결)
> 2. 그래프를 가지고 DFS 돌리는 부분

### 방문처리
위의 주의점에서도 언급했다시피, DFS는 무한루프에 빠질 위험성이 있다. 이를 방지하기 위해서 방문한 노드를 체크하는 작업이 필요하다.  
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

### 생성한 그래프로 DFS 돌리는 방법
```java
dfs(1); // 1번 노드부터 시작해서 dfs

public static void dfs(int vertex) {
    // 방문 처리
    visited[vertex] = true;

    // 방문 노드 출력 혹은 노드 내 특정 작업
    System.out.print(vertex + " > ");

    // 방문한 노드에 인접한 노드 찾기 -> 각 그래프 변수에 따라 반복한다.
    for (int v : graph.get(vertex)) {
        // 인접한 노드가 방문한 적이 없다면 dfs 수행
        if (!visited[v]) {
            dfs(v);
        }
    }
}
```