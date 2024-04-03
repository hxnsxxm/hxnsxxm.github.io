---
title: "[Level2] n진수 게임, Java"
excerpt: "프로그래머스 레벨 2 : n진수 게임 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/n진수게임

toc: true
toc_sticky: true

date: 2024-04-02
last_modified_at: 2024-04-02
---

## 문제

### 링크

[프로그래머스/n진수 게임](https://school.programmers.co.kr/learn/courses/30/lessons/17687)

<br>

### 문제 설명

튜브가 활동하는 코딩 동아리에서는 전통적으로 해오는 게임이 있다. 이 게임은 여러 사람이 둥글게 앉아서 숫자를 하나씩 차례대로 말하는 게임인데, 규칙은 다음과 같다.

1. 숫자를 0부터 시작해서 차례대로 말한다. 첫 번째 사람은 0, 두 번째 사람은 1, … 열 번째 사람은 9를 말한다.
2. 10 이상의 숫자부터는 한 자리씩 끊어서 말한다. 즉 열한 번째 사람은 10의 첫 자리인 1, 열두 번째 사람은 둘째 자리인 0을 말한다.

이렇게 게임을 진행할 경우,  
`0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 1, 0, 1, 1, 1, 2, 1, 3, 1, 4, …`  
순으로 숫자를 말하면 된다.

한편 코딩 동아리 일원들은 컴퓨터를 다루는 사람답게 이진수로 이 게임을 진행하기도 하는데, 이 경우에는  
`0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, …`  
순으로 숫자를 말하면 된다.

이진수로 진행하는 게임에 익숙해져 질려가던 사람들은 좀 더 난이도를 높이기 위해 이진법에서 십육진법까지 모든 진법으로 게임을 진행해보기로 했다. 숫자 게임이 익숙하지 않은 튜브는 게임에 져서 벌칙을 받는 굴욕을 피하기 위해, 자신이 말해야 하는 숫자를 스마트폰에 미리 출력해주는 프로그램을 만들려고 한다. 튜브의 프로그램을 구현하라.

<br>

### 입력 & 출력

#### 입력 형식

진법 `n`, 미리 구할 숫자의 갯수 `t`, 게임에 참가하는 인원 `m`, 튜브의 순서 `p` 가 주어진다.

- 2 ≦ `n` ≦ 16
- 0 ＜ `t` ≦ 1000
- 2 ≦ `m` ≦ 100
- 1 ≦ `p` ≦ `m`

#### 출력 형식

튜브가 말해야 하는 숫자 `t`개를 공백 없이 차례대로 나타낸 문자열. 단, `10`~`15`는 각각 대문자 `A`~`F`로 출력한다.

#### 입출력 예제

|n|t|m|p|result|
|---|---|---|---|---|
|2|4|2|1|"0111"|
|16|16|2|1|"02468ACE11111111"|
|16|16|2|2|"13579BDF01234567"|

<br>

## 풀이

![n진수게임-01.png](/assets/images/posts_img/algorithm-programmers/n진수게임-01.png)

문제 "n진수 게임"은 먼저 10진수를 n진수로 바꾸는 과정이 필요하다.  
10진수 `num`을 n진수로 바꾸는 과정은 다음과 같다.

```java
String nums = Integer.toString(num, n);
```

이후 n진수 `nums`를 한 글자씩 나눠 참여자들이 말할 때, 튜브의 차례를 반복하기 위해서 나눗셈 연산을 사용한다.  
이때 참여자가 `1`부터 시작함에 유의한다.

```java
다음 차례 = (현재 차례 % 참여자 수) + 1;
```

### 시간 복잡도

시간 복잡도의 경우 `m`명의 사람이 반복하면서 튜브의 차례를 `t`번 돌아야하기 때문에 <b>O(t * m)</b>이다.   
최악의 경우, 대략 1000 * 100 = 1,000,000 번이다.

<br>

### 구현 코드
```java
package level2.n진수게임;

public class solve_1st {
    public String solution(int n, int t, int m, int p) {
        String answer = "";

        int tt = 0;
        int num = 0;
        int mm = 1;
        while (tt < t) {
            String nums = Integer.toString(num, n);

            for (char ch : nums.toCharArray()) {
                if (mm == p) {
                    answer += ch;
                    tt++;
                }
                mm = mm%m + 1;

                if (tt == t)
                    break;
            }

            num++;
        }

        return answer.toUpperCase();
    }
}
```






<hr>
<b>Reference</b>  
