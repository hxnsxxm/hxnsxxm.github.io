---
title: "[Level 2] 모음사전, Java"
excerpt: "프로그래머스 레벨 2 : 모음사전 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/모음사전

toc: true
toc_sticky: true

date: 2024-04-03
last_modified_at: 2024-04-03
---

## 문제

### 링크

[프로그래머스/모음사전](https://school.programmers.co.kr/learn/courses/30/lessons/84512)

<br>

### 문제 설명

사전에 알파벳 모음 'A', 'E', 'I', 'O', 'U'만을 사용하여 만들 수 있는, 길이 5 이하의 모든 단어가 수록되어 있습니다. 사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA"이며, 마지막 단어는 "UUUUU"입니다.

단어 하나 word가 매개변수로 주어질 때, 이 단어가 사전에서 몇 번째 단어인지 return 하도록 solution 함수를 완성해주세요.

<br>

### 입력 & 출력

#### 제한사항

- word의 길이는 1 이상 5 이하입니다.
- word는 알파벳 대문자 'A', 'E', 'I', 'O', 'U'로만 이루어져 있습니다.

#### 입출력 예

|word|result|
|---|---|
|`"AAAAE"`|6|
|`"AAAE"`|10|
|`"I"`|1563|
|`"EIO"`|1189|

#### 입출력 예 설명

입출력 예 #1

사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA", "AAA", "AAAA", "AAAAA", "AAAAE", ... 와 같습니다. "AAAAE"는 사전에서 6번째 단어입니다.

입출력 예 #2

"AAAE"는 "A", "AA", "AAA", "AAAA", "AAAAA", "AAAAE", "AAAAI", "AAAAO", "AAAAU"의 다음인 10번째 단어입니다.

입출력 예 #3

"I"는 1563번째 단어입니다.

입출력 예 #4

"EIO"는 1189번째 단어입니다.

<br>

## 풀이

### 풀이 1

1. 사전의 알파벳 모음들{'A', 'E', 'I', 'O', 'U'}로 구성할 수 있는 모든 문자열을 `TreeSet`에 등록한다.
2. 입력 `word`에 해당하는 인덱스(index)를 `TreeSet`에서 구한다.

```java
answer = set.headSet(word, true).size();
```

<br>

#### 시간 복잡도

사전의 알파벳 모음 n 개를 가지고 사전의 모든 문자열을 만들 때는 <b>O(n ^ n)</b>이다.   

그리고 입력의 인덱스를 구하는 작업은 `TreeSet`의 개수가 m일 때, `set.headSet()`은 <b>O(m)</b>이다.  

만약, 문자의 개수가 증가하게 된다면 시간 복잡도가 상당히 커지게 될 것이다.  

<br>

#### 구현 코드

```java
import java.util.*;

class Solution {
    TreeSet<String> set = new TreeSet<>();
    String[] chs = {"", "A", "E", "I", "O", "U"};

    public int solution(String word) {
        int answer = 0;

        bt(0, "");
        //System.out.println(set);

        answer = set.headSet(word, true).size();

        return answer;
    }

    void bt(int depth, String str) {
        if (depth == 5) {
            if (!str.equals("")) {
                set.add(str);
            }
            return;
        }

        for (int i = 0; i < 6; i++) {
            bt(depth + 1, str + chs[i]);
        }
    }
}
```

<br>

### 풀이 2

사전의 알파벳 모음{'A', 'E', 'I', 'O', 'U'}으로 만들 수 있는 문자열은 <b>한 자리 문자열 ~ 다섯 자리 문자열</b>까지 존재한다.

1. 한 자리 문자열 &rarr; `5` 가지
2. 두 자리 문자열 &rarr; `5 * 5` 가지
3. 세 자리 문자열 &rarr; `5 * 5 * 5` 가지
4. 네 자리 문자열 &rarr; `5 * 5 * 5 * 5` 가지
5. 다섯 자리 문자열 &rarr; `5 * 5 * 5 * 5 * 5` 가지

모두 더하면 총 `3,905` 가지의 문자열이 존재한다는 것을 알 수 있다.

또한 사전순으로 정렬되어 있기 때문에 인덱스(index)로 입력 `word`를 파악할 수 있다.

![모음사전-01.png](/assets/images/posts_img/algorithm-programmers/모음사전-01.png)

먼저 `word`의 문자 개수만큼 반복하면서 해당 문자의 인덱스를 offset만큼 곱해주면 사전에서의 문자열 인덱스에 접근할 수 있다.

<b>문자의 인덱스</b>
```java
0 : 'A'
1 : 'E'
2 : 'I'
3 : 'O'
4 : 'U'
```

<b>offset</b>

| word     | offset          |
|----------|-----------------|
| 첫 번째 글자  | 3,905 / 5 = 781 |
| 두 번째 글자  | 781 / 5 = 156   |
| 세 번째 글자  | 156 / 5 = 31    |
| 네 번째 글자  | 31 / 5 = 6      |
| 다섯 번째 글자 | 6 / 5 = 1       |

<br>

결과적으로 알파벳 모음의 인덱스와 offset을 이용해 각 자리의 문자의 인덱스를 더함으로써 입력 `word`의 인덱스를 구할 수 있다.
```java
answer += "AEIOU".indexOf(해당 글자) * (offset) + 1;
```

<br>

#### 시간 복잡도

입력 `word`의 길이가 n 일 때, <b>O(n)</b>이다.  

<br>

#### 구현 코드
```java
package level2.모음사전;

public class solve_1st {
    public int solution(String word) {
        int answer = 0, per = 3905;
        for(String s : word.split("")) {
            //System.out.println(per);
            answer += "AEIOU".indexOf(s) * (per /= 5) + 1;
        }
        return answer;
    }
}
```



<hr>
<b>Reference</b>  
