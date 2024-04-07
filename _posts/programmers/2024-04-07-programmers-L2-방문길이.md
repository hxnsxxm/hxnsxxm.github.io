---
title: "[Level 2] 방문 길이, Java"
excerpt: "프로그래머스 레벨 2 : 방문 길이 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/방문길이

toc: true
toc_sticky: true

date: 2024-04-07
last_modified_at: 2024-04-07
---

## 문제

### 링크

[프로그래머스/방문 길이](https://school.programmers.co.kr/learn/courses/30/lessons/49994)

<br>

### 문제 설명

게임 캐릭터를 4가지 명령어를 통해 움직이려 합니다. 명령어는 다음과 같습니다.

- U: 위쪽으로 한 칸 가기

- D: 아래쪽으로 한 칸 가기

- R: 오른쪽으로 한 칸 가기

- L: 왼쪽으로 한 칸 가기


캐릭터는 좌표평면의 (0, 0) 위치에서 시작합니다. 좌표평면의 경계는 왼쪽 위(-5, 5), 왼쪽 아래(-5, -5), 오른쪽 위(5, 5), 오른쪽 아래(5, -5)로 이루어져 있습니다.

![방문길이1_qpp9l3.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ace0e7bc-9092-4b95-9bfb-3a55a2aa780e/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B51_qpp9l3.png)

예를 들어, "ULURRDLLU"로 명령했다면

![방문길이2_lezmdo.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/668c7458-e184-472d-9d32-f5d2acca759a/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B52_lezmdo.png)

- 1번 명령어부터 7번 명령어까지 다음과 같이 움직입니다.

![방문길이3_sootjd.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/08558e36-d667-4160-bfec-b754c78a7d85/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B53_sootjd.png)

- 8번 명령어부터 9번 명령어까지 다음과 같이 움직입니다.

![방문길이4_hlpiej.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/a52af28e-5835-438b-9f40-5467ebf9bf03/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B54_hlpiej.png)

이때, 우리는 게임 캐릭터가 지나간 길 중 **캐릭터가 처음 걸어본 길의 길이**를 구하려고 합니다. 예를 들어 위의 예시에서 게임 캐릭터가 움직인 길이는 9이지만, 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다. (8, 9번 명령어에서 움직인 길은 2, 3번 명령어에서 이미 거쳐 간 길입니다)

단, 좌표평면의 경계를 넘어가는 명령어는 무시합니다.

예를 들어, "LULLLLLLU"로 명령했다면

![방문길이5_nitjwj.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/f631f005-f8de-4392-a76c-a9ef64b6de08/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B55_nitjwj.png)

- 1번 명령어부터 6번 명령어대로 움직인 후, 7, 8번 명령어는 무시합니다. 다시 9번 명령어대로 움직입니다.

![방문길이6_nzhumd.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/35e62f0a-43c6-4142-bec6-6d28fbc57216/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B56_nzhumd.png)

이때 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다.

명령어가 매개변수 dirs로 주어질 때, 게임 캐릭터가 처음 걸어본 길의 길이를 구하여 return 하는 solution 함수를 완성해 주세요.

<br>

### 입력 & 출력

#### 제한사항

- dirs는 string형으로 주어지며, 'U', 'D', 'R', 'L' 이외에 문자는 주어지지 않습니다.
- dirs의 길이는 500 이하의 자연수입니다.

#### 입출력 예

|dirs|answer|
|---|---|
|"ULURRDLLU"|7|
|"LULLLLLLU"|7|

<br>

## 풀이

게임 캐릭터가 방문한 <b>고유한 길의 개수</b>를 구하는 문제이다.

그를 위해서 길을 방문했을 때, 해당 길을 저장하고 이동할 때마다 방문했었는지 확인해야 한다.

문제는 좌표를 저장하는 것이 아니라, 좌표와 좌표 사이의 길을 저장해야 한다는 점이다.

이러한 길은 좌표 별로 4 방향으로 나아갈 수 있다.

예를 들어서 (0, 0) 에서 (1, 0)을 향할 때, 해당 길을 기억해야 한다.

![방문길이-01.png](/assets/images/posts_img/algorithm-programmers/방문길이-01.png)

그래서 길을 "x1y1x2y2"라는 <b>문자열로 표현하고 저장</b>할 수 있다.
주의해야 할 점은 역방향인 "x2y2x1y1" 또한 저장해야 한다.

그리고 <b>고유한 길</b>을 저장하기 위해서 자료 구조로 <b>HashSet</b>을 사용한다.

<br>

### 시간 복잡도

입력으로 받은 명령어(`dirs`)의 길이가 n일 때, 모든 문자를 반복해야 한다. &rarr; O(n)

`HashSet.add()`은 O(1)의 시간 복잡도를 갖는다.

결과적으로 <b>O(n)</b>으로 동작할 수 있다.

<br>

### 구현 코드

```java
package level2.방문길이;

import java.util.HashSet;
import java.util.Set;

public class solve_1st {
    class Solution {
        public int solution(String dirs) {
            int answer = 0;

            Set<String> set = new HashSet<>();
            int[] dx = {1, 0, -1, 0}; //우 하 좌 상
            int[] dy = {0, -1, 0, 1};
            int x = 0;
            int y = 0;
            StringBuilder sb = new StringBuilder();

            for (char ch : dirs.toCharArray()) {
                int tx = x;
                int ty = y;

                if (ch == 'R') {        //우
                    tx += dx[0];
                    ty += dy[0];
                } else if (ch == 'D') { //하
                    tx += dx[1];
                    ty += dy[1];
                } else if (ch == 'L') { //좌
                    tx += dx[2];
                    ty += dy[2];
                } else {                //상
                    tx += dx[3];
                    ty += dy[3];
                }

                if (tx < -5 || ty < -5 || tx > 5 || ty > 5)
                    continue;

                sb.append(x).append(y).append(tx).append(ty);
                set.add(sb.toString());
                sb.delete(0, sb.length());
                sb.append(tx).append(ty).append(x).append(y);
                set.add(sb.toString());
                sb.delete(0, sb.length());
                //System.out.println(sb.toString());
                x = tx;
                y = ty;

            }

            answer = set.size()/2;

            return answer;
        }
    }
}
```



<hr>
<b>Reference</b>  
