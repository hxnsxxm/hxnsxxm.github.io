---
title: "[Level 2] 게임 맵 최단거리, Java"
excerpt: "프로그래머스 레벨 2 : 게임 맵 최단거리 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/게임맵최단거리

toc: true
toc_sticky: true

date: 2024-04-04
last_modified_at: 2024-04-04
---

## 문제

### 링크

[프로그래머스/게임 맵 최단거리](https://school.programmers.co.kr/learn/courses/30/lessons/1844)

<br>

### 문제 설명

ROR 게임은 두 팀으로 나누어서 진행하며, 상대 팀 진영을 먼저 파괴하면 이기는 게임입니다. 따라서, 각 팀은 상대 팀 진영에 최대한 빨리 도착하는 것이 유리합니다.

지금부터 당신은 한 팀의 팀원이 되어 게임을 진행하려고 합니다. 다음은 5 x 5 크기의 맵에, 당신의 캐릭터가 (행: 1, 열: 1) 위치에 있고, 상대 팀 진영은 (행: 5, 열: 5) 위치에 있는 경우의 예시입니다.

![최단거리1_sxuruo.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/dc3a1b49-13d3-4047-b6f8-6cc40b2702a7/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B51_sxuruo.png)

위 그림에서 검은색 부분은 벽으로 막혀있어 갈 수 없는 길이며, 흰색 부분은 갈 수 있는 길입니다. 캐릭터가 움직일 때는 동, 서, 남, 북 방향으로 한 칸씩 이동하며, 게임 맵을 벗어난 길은 갈 수 없습니다.  
아래 예시는 캐릭터가 상대 팀 진영으로 가는 두 가지 방법을 나타내고 있습니다.

- 첫 번째 방법은 11개의 칸을 지나서 상대 팀 진영에 도착했습니다.

![최단거리2_hnjd3b.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/9d909e5a-ca95-4088-9df9-d84cb804b2b0/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B52_hnjd3b.png)

- 두 번째 방법은 15개의 칸을 지나서 상대팀 진영에 도착했습니다.

![최단거리3_ntxygd.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/4b7cd629-a3c2-4e02-b748-a707211131de/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B53_ntxygd.png)

위 예시에서는 첫 번째 방법보다 더 빠르게 상대팀 진영에 도착하는 방법은 없으므로, 이 방법이 상대 팀 진영으로 가는 가장 빠른 방법입니다.

만약, 상대 팀이 자신의 팀 진영 주위에 벽을 세워두었다면 상대 팀 진영에 도착하지 못할 수도 있습니다. 예를 들어, 다음과 같은 경우에 당신의 캐릭터는 상대 팀 진영에 도착할 수 없습니다.

![최단거리4_of9xfg.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d963b4bd-12e5-45da-9ca7-549e453d58a9/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B54_of9xfg.png)

게임 맵의 상태 maps가 매개변수로 주어질 때, 캐릭터가 상대 팀 진영에 도착하기 위해서 지나가야 하는 칸의 개수의 **최솟값**을 return 하도록 solution 함수를 완성해주세요. 단, 상대 팀 진영에 도착할 수 없을 때는 -1을 return 해주세요.

<br>

### 입력 & 출력

#### 제한사항

- maps는 n x m 크기의 게임 맵의 상태가 들어있는 2차원 배열로, n과 m은 각각 1 이상 100 이하의 자연수입니다.
    - n과 m은 서로 같을 수도, 다를 수도 있지만, n과 m이 모두 1인 경우는 입력으로 주어지지 않습니다.
- maps는 0과 1로만 이루어져 있으며, 0은 벽이 있는 자리, 1은 벽이 없는 자리를 나타냅니다.
- 처음에 캐릭터는 게임 맵의 좌측 상단인 (1, 1) 위치에 있으며, 상대방 진영은 게임 맵의 우측 하단인 (n, m) 위치에 있습니다.

<br>

#### 입출력 예

|maps|answer|
|---|---|
|[[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,1],[0,0,0,0,1]]|11|
|[[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,0],[0,0,0,0,1]]|-1|

<br>

## 풀이

![게임맵최단거리-01.png](/assets/images/posts_img/algorithm-programmers/게임맵최단거리-01.png)

최단거리를 구하는 만큼 <b>BFS</b>를 이용하면 된다.

현재 좌표를 기준으로 상, 하, 좌, 우를 검사하여 나아갈 수 있다.

이때 벽은 `0`이고 한 번이라도 방문한 좌표는 `2`이상의 숫자를 갖고 있으므로 `maps`의 정수값으로

벽 확인 및 방문처리를 진행할 수 있다.

<br>

### 시간 복잡도

행의 개수 `n`, 열의 개수 `m`이라고 할 때, 모든 좌표의 개수는 `n * m`이다.

또한, 좌표 하나에서 4개의 방향(상하좌우)를 검사해야 하기 때문에 시간 복잡도는 <b>O(4 * n * m)</b>이다.

<br>

### 구현 코드

```java
package level2.게임맵최단거리;

import java.util.LinkedList;
import java.util.Queue;

public class solve_1st {
    class Solution {

        int n;
        int m;
        int[] dn = {0, 1, 0, -1};
        int[] dm = {1, 0, -1, 0};

        public int solution(int[][] maps) {
            n = maps.length;
            m = maps[0].length;

            bfs(maps);

            if (maps[n - 1][m - 1] == 1)
                return -1;
            return maps[n - 1][m - 1];
        }

        void bfs(int[][] maps) {
            Queue<int[]> queue = new LinkedList<>();
            queue.add(new int[]{0, 0});

            while (!queue.isEmpty()) {
                int[] pos = queue.poll();

                for (int i = 0; i < 4; i++) {
                    int tn = pos[0] + dn[i];
                    int tm = pos[1] + dm[i];

                    if (tn < 0 || tm < 0 || tn >= n || tm >= m)
                        continue;
                    if (maps[tn][tm] > 1 || maps[tn][tm] == 0)
                        continue;

                    maps[tn][tm] += maps[pos[0]][pos[1]];
                    queue.add(new int[]{tn, tm});
                }
            }
        }
    }
}
```



<hr>
<b>Reference</b>  
