---
title: "[Level 2] 호텔 대실, Java"
excerpt: "프로그래머스 레벨 2 : 호텔 대실 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/호텔대실

toc: true
toc_sticky: true

date: 2024-05-28
last_modified_at: 2024-05-28
---

## 문제

### 링크

[프로그래머스/호텔 대실](https://school.programmers.co.kr/learn/courses/30/lessons/155651)

<br>

### 문제 설명

호텔을 운영 중인 코니는 최소한의 객실만을 사용하여 예약 손님들을 받으려고 합니다. 한 번 사용한 객실은 퇴실 시간을 기준으로 10분간 청소를 하고 다음 손님들이 사용할 수 있습니다.  
예약 시각이 문자열 형태로 담긴 2차원 배열 `book_time`이 매개변수로 주어질 때, 코니에게 필요한 최소 객실의 수를 return 하는 solution 함수를 완성해주세요.

<br>

### 입력 & 출력

#### 제한사항

- 1 ≤ `book_time`의 길이 ≤ 1,000
  - `book_time[i]`는 ["HH:MM", "HH:MM"]의 형태로 이루어진 배열입니다
    - [대실 시작 시각, 대실 종료 시각] 형태입니다.
  - 시각은 HH:MM 형태로 24시간 표기법을 따르며, "00:00" 부터 "23:59" 까지로 주어집니다.
    - 예약 시각이 자정을 넘어가는 경우는 없습니다.
    - 시작 시각은 항상 종료 시각보다 빠릅니다.

#### 입출력 예

|book_time|result|
|---|---|
|[["15:00", "17:00"], ["16:40", "18:20"], ["14:20", "15:20"], ["14:10", "19:20"], ["18:20", "21:20"]]|3|
|[["09:10", "10:10"], ["10:20", "12:20"]]|1|
|[["10:20", "12:30"], ["10:20", "12:30"], ["10:20", "12:30"]]|3|

<br>

---

## 풀이

### 풀이 1

호텔 대실, 회의실 배정 등 시간에 따라 장소를 이용하려는 문제의 경우, Greedy 알고리즘을 사용하여 해결할 수 있다.

모든 이용시간들을 종료 시간을 기준으로 정렬하여 순서대로 다음 이용의 시작 시간과 비교하여 이용 가능 여부를 판단한다.

![호텔대실-01.png](/assets/images/posts_img/algorithm-programmers/호텔대실-01.png)

예를 들어서 1번의 종료 시간(`15:20`)과 2번의 시작 시간(`15:00`)을 비교하면 하나의 객실에 배정할 수 없다는 것을 알 수 있다.

이 경우에는 새로운 객실이 필요하다.

3번 시작 시간(`16:40`)은 1번 종료(`15:20`) 이후에 객실을 사용할 수 있기 때문에 해당 객실을 이용할 수 있다.

그래서 각 객실마다 이용객의 종료 시간을 기록하여 다음 예정자와 비교해봄으로써 구현할 수 있다.

다만 <b>주의해야 할 점은</b> 이용 가능한 객실이 2개 이상이라면 최적의 객실을 구해야 한다는 것이다.

시작 시간에서 최근에 종료한 객실을 이용하도록 하면 되는데, 객실 배정 후 정렬(sort)하여 구현한다.

<br>

#### 시간 복잡도

입력(`book_time`)의 길이를 n이라고 할 때 모두 반복하고, 정렬하기 때문에 <b>O(n * n * log(n))</b>이다.

<br>

#### 구현 코드

```java
package level2.호텔대실;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class solve_1st_01 {
    class Solution {
        public int solution(String[][] book_time) {
            int len = book_time.length;
            Integer[][] times = new Integer[len][2];
            for (int i = 0; i < len; i++) {
                times[i][0] = string2Sec(book_time[i][0]);
                times[i][1] = string2Sec(book_time[i][1]);
            }
            Arrays.sort(times, new Comparator<Integer[]>(){
                @Override
                public int compare(Integer[] s1, Integer[] s2) {
                    return s1[1] - s2[1];
                }
            });

            List<Integer> list = new ArrayList<>();

            list.add(times[0][1]);
            for (int i = 1; i < len; i++) {
                int currStart = times[i][0];
                int currEnd = times[i][1];

                int size = list.size();
                for (int j = 0; j < size; j++) {
                    if (currStart >= list.get(j) + 600) {
                        list.set(j, currEnd);
                        break;
                    } else if (j == size - 1) {
                        list.add(currEnd);
                    }
                }
                Collections.sort(list, Collections.reverseOrder());
            }

            return list.size();
        }

        private int string2Sec(String time) {
            String[] tmp = time.split(":");
            return Integer.parseInt(tmp[0]) * 60 * 60 + Integer.parseInt(tmp[1]) * 60;
        }
    }
}
```

<br>

### 풀이 2

모든 이용 시간을 분으로 쪼개어 각 분을 손님에 따라 카운팅한다.

이때 겹치는 손님들은 다른 방에 배정해야 하기 때문에 카운트가 가장 큰 분(min)을 구하면 된다.

<br>

#### 시간 복잡도

입력(`book_time`)의 길이를 n이라고 할 때 <b>O(n)</b>이다.

<br>

#### 구현 코드

```java
package level2.호텔대실;

public class solve_1st_02 {
    class Solution {
        public int solution(String[][] book_time) {
            int answer = 0;
            int[] howManyBooksInTime = new int[1440];
            for(String[] book : book_time){
                int start = toInt(book[0]);
                int end = Math.min(1439,toInt(book[1])+10);
                for(int i=start; i<end; i++){
                    howManyBooksInTime[i]++;
                }
            }
            for(int i=0; i<1440; i++){
                answer = Math.max(answer,howManyBooksInTime[i]);
            }
            return answer;
        }

        public int toInt(String s){
            String[] ss = s.split(":");
            return Integer.parseInt(ss[0])*60 + Integer.parseInt(ss[1]);
        }
    }
}
```


<hr>
<b>Reference</b>  
