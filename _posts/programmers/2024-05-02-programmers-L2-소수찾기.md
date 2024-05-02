---
title: "[Level 2] 소수 찾기, Java"
excerpt: "프로그래머스 레벨 2 : 소수 찾기 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/소수찾기

toc: true
toc_sticky: true

date: 2024-05-02
last_modified_at: 2024-05-02
---

## 문제

### 링크

[프로그래머스/소수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/42839)

<br>

### 문제 설명

한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

<br>

### 입력 & 출력

#### 제한사항

- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

#### 입출력 예

|numbers|return|
|---|---|
|"17"|3|
|"011"|2|

<br>

## 풀이

입력으로 주어진 n개의 숫자에 대해서 1 ~ n 자리의 숫자를 만들어 소수인지를 판별해야 한다.

예를 들어서 `{1, 2, 3}`이 입력으로 주어졌다면,
한 자리수 `{1}, {2}, {3}`, 두 자리수 `{12}, {13}, {21}, {23}, {31}, {32}`,
세 자리수 `{123}, {132}, {213}, {231}, {312}, {321}`을 만들 수 있다.

<b>백트래킹</b> 알고리즘을 이용해서 해당 경우의 수를 만든다.

이렇게 만든 숫자들이 소수인지 판별해주면 된다.

<br>

소수 판별은 <b>에라토스테네스의 체</b>를 활용하면 쉽게 구할 수 있다.

<br>

### 시간 복잡도

입력 `numbers`의 길이가 n이라면 `1! + 2! + 3! + ... + n!`의 경우를 가지기 때문에 O(n!)이 된다.

그리고 만들어지는 숫자에 대해서 에라토스테네스의 체를 적용하기 때문에 숫자 m에 대해서 O(m ^ 1/2)가 추가된다.

결과적으로 <b>O(n! * m^1/2)</b>정도 될 것으로 판단된다.

<br>

### 구현 코드

```java
package level2.소수찾기;

import java.util.HashSet;
import java.util.Set;

public class solve_1st {
    class Solution {
        Set<Integer> set = new HashSet<>();
        int len;
        String input;
        boolean[] used;

        public int solution(String numbers) {
            len = numbers.length();
            input = numbers;
            used = new boolean[len];
            for (int i = 1; i <= len; i++) {
                bt(0, i, "");
            }

            return set.size();
        }

        void bt(int depth, int goal, String str) {
            if (depth == goal) {
                if (isPrime(str)) {
                    set.add(Integer.parseInt(str));
                }

                return;
            }

            for (int i = 0; i < len; i++) {
                if (!used[i]) {
                    used[i] = true;
                    bt(depth + 1, goal, str + input.charAt(i));
                    used[i] = false;
                }
            }
        }

        boolean isPrime(String str) {
            int num = Integer.parseInt(str);

            if (num == 0 || num == 1)
                return false;
            if (num == 2 || num == 3)
                return true;
            for (int i = 2; i <= Math.sqrt(num); i++) {
                if (num % i == 0)
                    return false;
            }

            return true;
        }
    }
}
```


<hr>
<b>Reference</b>  
