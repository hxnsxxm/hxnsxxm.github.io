---
title: "[Level 2] 땅따먹기, Java"
excerpt: "프로그래머스 레벨 2 : 땅따먹기 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/땅따먹기

toc: true
toc_sticky: true

date: 2024-04-10
last_modified_at: 2024-04-10
---

## 문제

### 링크

[프로그래머스/땅따먹기](https://school.programmers.co.kr/learn/courses/30/lessons/12913)

<br>

### 문제 설명

땅따먹기 게임을 하려고 합니다. 땅따먹기 게임의 땅(land)은 총 N행 4열로 이루어져 있고, 모든 칸에는 점수가 쓰여 있습니다. 1행부터 땅을 밟으며 한 행씩 내려올 때, 각 행의 4칸 중 한 칸만 밟으면서 내려와야 합니다. **단, 땅따먹기 게임에는 한 행씩 내려올 때, 같은 열을 연속해서 밟을 수 없는 특수 규칙이 있습니다.**

예를 들면,

| 1 | 2 | 3 | 5 |  
| 5 | 6 | 7 | 8 |  
| 4 | 3 | 2 | 1 |

로 땅이 주어졌다면, 1행에서 네번째 칸 (5)를 밟았으면, 2행의 네번째 칸 (8)은 밟을 수 없습니다.

마지막 행까지 모두 내려왔을 때, 얻을 수 있는 점수의 최대값을 return하는 solution 함수를 완성해 주세요. 위 예의 경우, 1행의 네번째 칸 (5), 2행의 세번째 칸 (7), 3행의 첫번째 칸 (4) 땅을 밟아 16점이 최고점이 되므로 16을 return 하면 됩니다.

<br>

### 입력 & 출력

#### 제한사항

- 행의 개수 N : 100,000 이하의 자연수
- 열의 개수는 4개이고, 땅(land)은 2차원 배열로 주어집니다.
- 점수 : 100 이하의 자연수

#### 입출력 예

| land                            | answer |
| ------------------------------- | ------ |
| [[1,2,3,5],[5,6,7,8],[4,3,2,1]] | 16     |

<br>

## 풀이

단순하게는 첫 번째 행부터 하나의 원소씩 아래 행으로 내려가고, 맨 아래 도착했을 때 값을 저장해두었다가 이후 모든 경우의 값들을
비교하면서 최댓값을 구하는 방법이 있다. 해당 방법의 모든 경우는 아래와 같다.

<b>1행의 첫 번째 `1`</b>
![땅따먹기-01.png](/assets/images/posts_img/algorithm-programmers/땅따먹기-01.png)

<b>1행의 두 번째 `2`</b>
![땅따먹기-02.png](/assets/images/posts_img/algorithm-programmers/땅따먹기-02.png)

<b>1행의 세 번째 `3`</b>
![땅따먹기-03.png](/assets/images/posts_img/algorithm-programmers/땅따먹기-03.png)

<b>1행의 네 번째 `5`</b>
![땅따먹기-04.png](/assets/images/posts_img/algorithm-programmers/땅따먹기-04.png)

이 방법은 첫 번째 행의 하나의 원소를 기준으로 시간 복잡도가 O(3 ^ (n-1))이다.

이것을 4개의 열에 대해서 진행해야 하니 결과적으로 시간 복잡도가 O(4 * 3^(n-1))이 된다.

입력의 최대 n이 `100,000`이기 때문에 이 방법은 적절하지 않다.

<br>

<b>다이나믹 프로그래밍 기법 활용</b>  

위의 그림들을 살펴보면 중복되는 경우들이 있다. 예를 들어서 2행부터 `5`, `6`, `7`, `8`은 매 번 반복된다.
2행의 `5`만 하더라도 1행의 `2`, `3`, `5`로부터 한 번씩 진행된다.

이것을 이용해서 다이나믹 프로그래밍(DP) 기법을 적용하여 문제를 해결할 수 있다.

새로운 2차원 배열을 만들고, 마지막 행부터 위로 올라오면서 값을 업데이드하는 방법이다.

![땅따먹기-05.png](/assets/images/posts_img/algorithm-programmers/땅따먹기-05.png)

예를 들어서 위처럼 입력(`land`) 외에 빈 배열(`dp`)를 만들고, 마지막 열만 초기화한다.

이후 2행 &rarr; 1행 방향으로 올라오면서 각 열의 값을 업데이트 하면 되는데, `dp` 2행 1열 자리에는
`dp` 3행의 `3`, `2`, `1`의 최댓값과 `land`의 `5`를 더한 값으로 업데이트하면 된다.

![땅따먹기-06.png](/assets/images/posts_img/algorithm-programmers/땅따먹기-06.png)

이걸 모두 반복하면 다음과 같은 결과를 얻을 수 있다.

![땅따먹기-07.png](/assets/images/posts_img/algorithm-programmers/땅따먹기-07.png)

마지막으로 배열 `dp`의 1행의 4개 열 중에서 최댓값을 반환하면 문제가 해결된다.

<br>

### 시간 복잡도

다이나믹 프로그래밍 방법은 n행부터 1행까지 반복하면 된다. 결과적으로 시간 복잡도는 <b>O(n)</b>이다.

<br>

### 구현 코드

```java
package level2.땅따먹기;

public class solve_1st {
    class Solution {
        int solution(int[][] land) {
            int answer = 0;

            int len = land.length;
            int[][] dp = new int[len][4];

            dp[len - 1] = land[len - 1];

            for (int i = len - 1; i > 0; i--) {
                dp[i - 1][0] = land[i - 1][0] + Math.max(dp[i][1], Math.max(dp[i][2], dp[i][3]));
                dp[i - 1][1] = land[i - 1][1] + Math.max(dp[i][0], Math.max(dp[i][2], dp[i][3]));
                dp[i - 1][2] = land[i - 1][2] + Math.max(dp[i][0], Math.max(dp[i][1], dp[i][3]));
                dp[i - 1][3] = land[i - 1][3] + Math.max(dp[i][0], Math.max(dp[i][1], dp[i][2]));
            }

            answer = Math.max(Math.max(dp[0][0], dp[0][1]), Math.max(dp[0][2], dp[0][3]));

            return answer;
        }
    }
}
```



<hr>
<b>Reference</b>  
