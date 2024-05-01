---
title: "[Level 2] 가장 큰 수, Java"
excerpt: "프로그래머스 레벨 2 : 가장 큰 수 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/가장큰수

toc: true
toc_sticky: true

date: 2024-05-01
last_modified_at: 2024-05-01
---

## 문제

### 링크

[프로그래머스/가장 큰 수](https://school.programmers.co.kr/learn/courses/30/lessons/42746)

<br>

### 문제 설명

0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

<br>

### 입력 & 출력

#### 제한 사항

- numbers의 길이는 1 이상 100,000 이하입니다.
- numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

#### 입출력 예

|numbers|return|
|---|---|
|[6, 10, 2]|"6210"|
|[3, 30, 34, 5, 9]|"9534330"|

<br>

## 풀이

숫자를 덧붙여 가장 큰 수를 만들기 위해서는 아래와 같은 사항을 고려해야 한다.

| number 1 | number 2 | 큰 수   | 설명                |
|----------|----------|-------|-------------------|
| 1        | 9143     | 91431 | 9143이 1보다 먼저 와야 함 |
| 9        | 9143     | 99143 | 9가 먼저 와야 함        |
| 3        | 34       | 343   | 34가 먼저 와야 함       |
| 92       | 91       | 9291  | 92가 먼저 와야 함       |
| 92       | 23       | 9223  | 92가 먼저 와야 함       |

큰 수를 만들기 위해서 <b>정렬</b>을 활용하는데, 위의 경우들을 만족하는 조건으로 정렬을 만든다.

정렬 조건은 쉽다. 두 수를 문자열 덧셈을 해서 더 큰 순서를 만족하게 하면 된다.

예를 들어서, `1`과 `9143`에 대해서 문자열 덧셈을 하면 "19143"과 "91431"을 만들 수 있다. 이것을 조건으로 사용한다.

<br>

### 시간 복잡도

정렬 방법 `Collections.sort()`는 TimeSort (삽입 정렬과 병합 정렬 결합)을 사용하며, 평균/최악 <b>O(nlog(n))</b>을 만족한다.

<br>

### 구현 코드

```java
package level2.가장큰수;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class solve_1st {
    class Solution {
        public String solution(int[] numbers) {
            String answer = "";

            List<String> str = new ArrayList<>();
            for (int i = 0; i < numbers.length; i++) {
                str.add(String.valueOf(numbers[i]));
            }

            Collections.sort(str, new Comparator<String>() {
                @Override
                public int compare(String s1, String s2) {
                    return (s2+s1).compareTo(s1+s2);
                }
            });

            if (str.get(0).equals("0"))
                return "0";

            return String.join("", str);
        }
    }
}
```


<hr>
<b>Reference</b>  
