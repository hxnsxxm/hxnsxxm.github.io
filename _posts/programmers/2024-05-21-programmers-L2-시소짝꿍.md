---
title: "[Level 2] 시소 짝꿍, Java"
excerpt: "프로그래머스 레벨 2 : 시소 짝꿍 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/시소짝꿍

toc: true
toc_sticky: true

date: 2024-05-21
last_modified_at: 2024-05-21
---

## 문제

### 링크

[프로그래머스/시소 짝꿍](https://school.programmers.co.kr/learn/courses/30/lessons/152996#)

<br>

### 문제 설명

어느 공원 놀이터에는 시소가 하나 설치되어 있습니다. 이 시소는 중심으로부터 2(m), 3(m), 4(m) 거리의 지점에 좌석이 하나씩 있습니다.  
이 시소를 두 명이 마주 보고 탄다고 할 때, 시소가 평형인 상태에서 각각에 의해 시소에 걸리는 토크의 크기가 서로 상쇄되어 완전한 균형을 이룰 수 있다면 그 두 사람을 시소 짝꿍이라고 합니다. 즉, 탑승한 사람의 무게와 시소 축과 좌석 간의 거리의 곱이 양쪽 다 같다면 시소 짝꿍이라고 할 수 있습니다.  
사람들의 몸무게 목록 `weights`이 주어질 때, 시소 짝꿍이 몇 쌍 존재하는지 구하여 return 하도록 solution 함수를 완성해주세요.

<br>

### 입력 & 출력

#### 제한 사항

- 2 ≤ `weights`의 길이 ≤ 100,000
- 100 ≤ `weights`[i] ≤ 1,000
    - 몸무게 단위는 N(뉴턴)으로 주어집니다.
    - 몸무게는 모두 정수입니다.

#### 입출력 예

|weights|result|
|---|---|
|[100,180,360,100,270]|4|

<br>

---

## 풀이

이 문제에서 유의해야 할 점이 몇 가지 있다.

1. {100, 100} 같이 동일한 몸무게에 대해서는 한 쌍으로 취급하지만, 200, 300, 400에 각각 포함 되어야 한다.
2. {300, 400, 600} 같은 경우 {300, 400}, {300, 600}, {400, 600} 이 모두 카운트 되어야 한다.

또한 두 몸무게를 비교하여 짝꿍을 구하기 위해서 비례식을 이용할 수 있는데,
`2 : 3 = a : b`와 같은 식이다.

이것을 바탕으로 작은 몸무게(`a`), 큰 몸무게(`b`)가 있다고 가정했을 때,
`a`는 `b`의 2/3, 1/2, 3/4만 검사해보면 된다. 그리고 `a`를 Map으로 관리하여 짝꿍을 검사하도록 한다.

이 과정에서 동일한 몸무게가 여러 명이라면 누적합으로 계산하면 된다.

<br>

### 구현 코드

```java
package level2.시소짝꿍;

import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class solve_1st {
    class Solution {
        public long solution(int[] weights) {
            long answer = 0;

            Arrays.sort(weights);
            Map<Double, Integer> map = new HashMap<>();
            for(int i : weights) {
                double a = i*1.0;
                double b = (i*2.0)/3.0;
                double c = (i*1.0)/2.0;
                double d = (i*3.0)/4.0;
                
                if(map.containsKey(a)) 
                    answer += map.get(a);
                if(map.containsKey(b)) 
                    answer += map.get(b);
                if(map.containsKey(c)) 
                    answer += map.get(c);
                if(map.containsKey(d)) 
                    answer += map.get(d);
                
                map.put((i*1.0), map.getOrDefault((i*1.0), 0)+1);
            }

            return answer;
        }
    }
}
```


<hr>
<b>Reference</b>  
