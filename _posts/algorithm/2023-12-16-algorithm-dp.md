---
title: "동적 계획법(DP)과 Java 구현 방법"
excerpt: "DP와 Java 구현 방법을 알아본다."

categories:
  - Basic
tags:
  - [algorithm, dp]

permalink: /algorithm/basic/dp/

toc: true
toc_sticky: true

date: 2023-12-16
last_modified_at: 2023-12-16
---

## DP
> - Optimization problem을 해결하는 전략 중 하나이다.  
> - 문제를 subproblem(s)로 나누고 해당 subproblem(s)의 optimal solution(s)을 활용해서 최종적으로 problem의 optimal solution을 찾는 방식이다.
> - 겹치는(overlapping) subproblems 는 <b>한 번만 계산하고 그 결과를 저장한 뒤 재사용</b>한다.

핵심은  
- subproblem(s)의 optimal solution(s)
- overlapping subproblems

### Optimization problem
- 문제를 해결하는 최적의 답(optimal solution)을 찾아야 하는 문제
- optimal solution은 하나 이상일 수 있다.
- maximum 혹은 minimum value를 가지는 solution을 찾는 문제들이 주를 이룬다.
- e.g. 가장 빨리 도착하는 경로의 소요 시간은?
- e.g. 언제 주식을 사고 팔 때 가장 수익이 높은지?
  
value &rarr; 소요 시간, 수익  
solution &rarr; 경로, 언제 사고 팔지

## DP의 두 가지 접근 방식

|  | Top-Down approach  | Bottom-Up approach |
| ---- | ----- | ------------------- |
| 구조 | recursive | iterative |
| subproblem 결과 저장 | memoization | tabulation |
| 언제 선호되는지 | subproblems의 일부만 계산해도<br>최종 optimal solution을 구할 수 있을 때 | 모든 subproblems를 계산해야<br>최종 optimal solution을 구할 수 있을 때 |

&rarr; 보통은 Bottom-Up approach가 자주 사용되는 것 같음  
&rarr; Top-Down approach는 재귀를 활용하기 때문에 <b>메모리 및 성능 이슈</b>가 있을 수 있음

## DP를 사용한 알고리즘 설계 순서
1. 주어진 문제의 optimal solution이 <b>구조적으로 어떤 특징</b>을 가지는지 분석한다.
2. 재귀적인 형태로 optimal solution의 value를 정의한다.
3. (주로) Bottom-Up 방식으로 optimal solution의 value를 구한다.
4. (추가될 수 있음) 지금까지 계산된 정보를 바탕으로 optimal solution을 구한다.  
  
<hr>

## 예제

### 1. Climbing Stairs (계단 오르기)

정상까지 오르는데 n 번의 스텝이 필요한 계단이 있다. 한 번에 한 스텝 혹은 두 스텝으로 오를 수 있을 때, 계산의 정상까지 오르기 위해 몇 개의 유니크한 방법이 있는지 구하시오.  

example 1: `n = 2` &rarr; 답 = 2 (1 + 1, 2)  
exmaple 2: `n = 3` &rarr; 답 = 3 (1 + 1 + 1, 1 + 2, 2 + 1)  

<b>optimization problem</b>  
총 몇 개의 유니크한 방법이 있는 지를 구한다는 것은 최대 몇 개의 유니크한 방법이 있는 지를 물어보는 것과 동일  

> 주어진 problem을 subproblem들로 나누고, 그 subproblem들 각각의 optimal solution을 활용해서 원래 problem의 optimal solution을 구할 수 있을까?  

when, `f(n)` = n 스텝의 계단을 오르는 유니크한 방법 수  
`f(n) = f(n - 1) + f(n - 2)`  

<img src="/assets/images/posts_img/algorithm-dp/dp-1.png" width="60%">

```java
int climb(int n) {
    if (n <= 2) 
        return n;
    return climb(n-1) + climb(n-2);
}
```
⭐ 탈출 조건 &rarr; `if (n <= 2) return`  

위의 코드는 <b>recursion 방식</b>이다 &rarr; DP로 문제를 푼 것이 아님  

<img src="/assets/images/posts_img/algorithm-dp/dp-3.png" width="80%">

재귀를 구조적으로 살펴보면 <b>동일하게 반복되는 부분</b>이 생긴다 &rarr; `c(1)`, `c(2)` 등  
&rarr; 성능 저하  
&rarr; 겹치는 subproblem들이 많다.  
&rarr; 동일한 input에 대한 function call은 처음 한 번만 계산하고 결과를 메모해 뒀다가 이후에 재사용하는 방식으로 해결한다.  

<b>recursion + memoization &rarr; Top-Down approach DP</b>  
```java
int[] memo = new memo[N + 1];

int climb(int n) {
    if (memo[n] != 0)
        return memo[n];
    if (n <= 2)
        return n;
    return memo[n] = climb(n-1) + climb(n-2);
}
```
<br>
<b>iterative + tabulation &rarr; Bottom-Up approach DP</b>
```java
int climb(int n) {
    int[] tabular = new int[N + 1];
    tabular[1] = 1;
    tabular[2] = 2;

    for (int i = 3; i <= n; i++) {
        tabular[i] = tabular[i-1] + tabular[i-2];
    }

    return tabular[n];
}
```
> <b>tabulation</b>  
> 작은 subproblem부터 시작해서 최종 problem 결과를 도출할 때까지 결과를 차례대로 기록하는 것  
  
### 시간 복잡도

|  | Recursion | Top-Down approach  | Bottom-Up approach |
| 시간 복잡도 | O(2^n) | O(n) | O(n) |  

<br>
<br>
  
## 언제 DP를 사용할까?
<b>DP를 사용하기 위해서 확인해야 할 두 가지 조건</b>  
1. optimization problem이 optimal substructure여야 한다.  
2. optimization problem이 overlapping subproblems를 가져야 한다.  
<br>

### Optimal substructure
optimization problem의 optimal solution이 subproblem(s)의 optimal solution(s)을 포함하는 구조  

<b>e.g.</b>
when, `f(n)` = n 스텝의 계단을 오르는 유니크한 방법 수  
`f(n) = f(n - 1) + f(n - 2)`

<br>
❗ 주의  
> optimal substructure가 아닌데 optimal substructure라고 판단하는 것을 주의해야 한다.  

<img src="/assets/images/posts_img/algorithm-dp/dp-4.png" width="45%">

- optimal substructure가 되려면 subproblems는 독립적(independent)이어야 한다.
- subproblem의 solution은 다른 subproblem solution에 영향을 줘선 안된다.  
<br>

### Overlapping subproblems
재귀적 형태의 알고리즘이 동작할 때 동일한 subproblems을 여러 번 해결해야 한다면  
이 optimization problem은 overlapping subproblems을 가진다고 표현한다.

<img src="/assets/images/posts_img/algorithm-dp/dp-3.png" width="80%">

<hr>
<b>Reference</b>  
[유튜브 채널 - 쉬운코드](https://www.youtube.com/watch?v=GtqHli8HIqk&list=PLBj9m0iFUTlJgZKEKS7q1G3eZkk8Pycac&index=25)