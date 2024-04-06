---
title: "[Level 2] 더 맵게, Java"
excerpt: "프로그래머스 레벨 2 : 더 맵게 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/더맵게

toc: true
toc_sticky: true

date: 2024-04-06
last_modified_at: 2024-04-06
---

## 문제

### 링크

[프로그래머스/더 맵게](https://school.programmers.co.kr/learn/courses/30/lessons/42626)

<br>

### 문제 설명

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

```
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
```

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.  
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

<br>

### 입력 & 출력

#### 제한 사항

- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

#### 입출력 예

|scoville|K|return|
|---|---|---|
|[1, 2, 3, 9, 10, 12]|7|2|

#### 입출력 예 설명

1. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.  
   새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5  
   가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]

2. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.  
   새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13  
   가진 음식의 스코빌 지수 = [13, 9, 10, 12]


모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.

<br>

## 풀이

스코빌 지수가 가장 낮은 두 개의 음식을 섞은 이후에, 해당 음식의 스코빌을 포함하여
다시 가장 낮은 두 개의 음식을 섞어야 한다.

즉, 음식을 섞을 때마다 새롭게 가장 낮은 두 개의 음식을 알아야 하고, 매 번 정렬이 필요하다는 것을 알 수 있다.

![더맵게-01.png](/assets/images/posts_img/algorithm-programmers/더맵게-01.png)

이는 자료 구조 중 <b>Priority Queue</b>로 구현할 수 있다.

<br>

### 시간 복잡도

모든 음식(`n`)에 대하여 priority queue에 추가하는 작업(`log n`)이 필요하다.

결과적으로 <b>O(n * log n)</b>이다.

<br>

### 구현 코드

```java
package level2.더맵게;

import java.util.PriorityQueue;

public class solve_1st {
    class Solution {
        public int solution(int[] scoville, int K) {
            int answer = 0;

            PriorityQueue<Integer> q = new PriorityQueue<>();

            for (int s : scoville)
                q.add(s);

            while (q.size() > 1 && q.peek() < K) {
                int s1 = q.poll();
                int s2 = q.poll();

                q.add(s1 + s2 * 2);
                answer++;
            }

            if (q.size() == 1 && q.peek() < K)
                answer = -1;

            return answer;
        }
    }
}
```





<hr>
<b>Reference</b>  
