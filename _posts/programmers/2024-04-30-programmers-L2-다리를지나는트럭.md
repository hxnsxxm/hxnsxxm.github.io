---
title: "[Level 2] 다리를 지나는 트럭, Java"
excerpt: "프로그래머스 레벨 2 : 다리를 지나는 트럭 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/다리를지나는트럭

toc: true
toc_sticky: true

date: 2024-04-30
last_modified_at: 2024-04-30
---

## 문제

### 링크

[프로그래머스/다리를 지나는 트럭](https://school.programmers.co.kr/learn/courses/30/lessons/42583)

<br>

### 문제 설명

트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

|경과 시간|다리를 지난 트럭|다리를 건너는 트럭|대기 트럭|
|---|---|---|---|
|0|[]|[]|[7,4,5,6]|
|1~2|[]|[7]|[4,5,6]|
|3|[7]|[4]|[5,6]|
|4|[7]|[4,5]|[6]|
|5|[7,4]|[5]|[6]|
|6~7|[7,4,5]|[6]|[]|
|8|[7,4,5,6]|[]|[]|

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

<br>

### 입력 & 출력

#### 제한 조건

- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

#### 입출력 예

|bridge_length|weight|truck_weights|return|
|---|---|---|---|
|2|10|[7,4,5,6]|8|
|100|100|[10]|101|
|100|100|[10,10,10,10,10,10,10,10,10,10]|110|

<br>

## 풀이

다리를 의미하는 Queue를 생성하고, `0`으로 다리 길이만큼 채워넣는다.

| bridge_length |weight| truck_weights |
|---------------|---|---------------|
| 4             |10| [7,4,5,1,6]   |

예를 들어서 위의 테이블처럼 입력이 주어진다면 아래와 같이 나타낼 수 있다.

![다리를지나는트럭-01.png](/assets/images/posts_img/algorithm-programmers/다리를지나는트럭-01.png)

이후 트럭을 반복하면서 다리 위의 트럭이 지나가고, 새로운 트럭이 다리 위로 올라갈 수 있는지 판단한다.

트럭 `7`이 다리 위로 올라온 순간부터 다음 트럭 `4`는 다리 위로 올라올 수 없기 때문에 트럭 `7`이 다리를 완전히
지나가는 것을 기다려야 한다.

![다리를지나는트럭-02.png](/assets/images/posts_img/algorithm-programmers/다리를지나는트럭-02.png)

이후 트럭 `4`가 다리 위로 올라오면 트럭 `5`, `1` 또한 다리 위로 올라올 수 있다.

![다리를지나는트럭-03.png](/assets/images/posts_img/algorithm-programmers/다리를지나는트럭-03.png)

이러한 과정을 거치면서 마지막 트럭 `6`이 완전히 다리를 지나는 것을 카운트한다.

<br>

### 시간 복잡도

최악의 경우는 모든 트럭에 대해서 다리 위에 올라갈 수 있는 트럭이 한 대만 가능할 때이다.

이 경우는 `bridge_length`가 `n`이고, `truck_weights`의 길이가 `m`일 때 <b>O(nm)</b>이다.

문제에서는 `100,000,000`번 정도이다.

<br>

### 구현 코드

```java
package level2.다리를지나는트럭;

import java.util.LinkedList;
import java.util.Queue;

public class solve_1st {
    class Solution {
        public int solution(int bridge_length, int weight, int[] truck_weights) {
            Queue<Integer> q = new LinkedList<>();
            for(int i = 0; i < bridge_length; i++)
                q.offer(0);

            int idx = 0;
            int total = 0;
            int sec = 0;

            while (idx < truck_weights.length) {
                total -= q.poll();
                sec++;

                if (total + truck_weights[idx] <= weight) {
                    q.offer(truck_weights[idx]);
                    total += truck_weights[idx];
                    idx++;
                }
                else
                    q.offer(0);
            }

            return sec + bridge_length;
        }
    }
}
```


<hr>
<b>Reference</b>  
