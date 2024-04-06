---
title: "[Level 2] 뒤에 있는 큰 수 찾기, Java"
excerpt: "프로그래머스 레벨 2 : 뒤에 있는 큰 수 찾기 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/뒤에있는큰수찾기

toc: true
toc_sticky: true

date: 2024-04-05
last_modified_at: 2024-04-05
---

## 문제

### 링크

[프로그래머스/뒤에 있는 큰 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/154539)

<br>

### 문제 설명

정수로 이루어진 배열 `numbers`가 있습니다. 배열 의 각 원소들에 대해 자신보다 뒤에 있는 숫자 중에서 자신보다 크면서 가장 가까이 있는 수를 뒷 큰수라고 합니다.  
정수 배열 `numbers`가 매개변수로 주어질 때, 모든 원소에 대한 뒷 큰수들을 차례로 담은 배열을 return 하도록 solution 함수를 완성해주세요. 단, 뒷 큰수가 존재하지 않는 원소는 -1을 담습니다.

<br>

### 입력 & 출력

#### 제한사항

- 4 ≤ `numbers`의 길이 ≤ 1,000,000
  - 1 ≤ `numbers[i]` ≤ 1,000,000

<br>

#### 입출력 예

|numbers|result|
|---|---|
|[2, 3, 3, 5]|[3, 5, 5, -1]|
|[9, 1, 5, 3, 6, 2]|[-1, 5, 6, 6, -1, -1]|

<br>

## 풀이

입력으로 들어오는 `numbers`가 n개라면 모든 숫자에 대해서, 해당 숫자의 뒤의 숫자들을 모두 비교해봐야 한다.

이때 시간 복잡도는 <b>O(n ^ 2)</b>가 되는데, n이 최대 1,000,000일 수 있기 때문에
최악의 경우 1조일 수 있다. 그래서 다른 방법을 생각해야 한다.

<br>

### 풀이 1 : priority queue

먼저 연속된 두 값을 비교하여 뒤에 있는 값이 더 크다면 바로 "뒷 큰 수"로 여길 수 있다.

문제는 앞의 값이 크거나 같은 경우인데, 따로 저장해두었다가 인덱스를 반복하면서 비교해야 한다.

![뒤에있는큰수찾기-01.png](/assets/images/posts_img/algorithm-programmers/뒤에있는큰수찾기-01.png)

위와 같이 9, 1, 5, 3, 6, 2가 입력으로 주어진다고 가정한다.

이때 9와 1을 비교했을 때 9가 더 크기 때문에 9는 따로 저장해두고 그 다음 비교로 넘어가야 한다.

![뒤에있는큰수찾기-02.png](/assets/images/posts_img/algorithm-programmers/뒤에있는큰수찾기-02.png)

이후 1과 5를 비교했을 때 더 큰 수를 찾았기 때문에 1의 인덱스에 5를 넣는다.
또한, 이전에 저장해둔 9와 5를 다시 비교하지만, 9가 더 크기 때문에 더 큰 수가 아니다.

다시 5와 3을 비교했을 때 5가 더 크기 때문에 더 큰 수가 아니다. 5를 다시 저장해두어야 한다.

![뒤에있는큰수찾기-03.png](/assets/images/posts_img/algorithm-programmers/뒤에있는큰수찾기-03.png)

다시 3과 6을 비교했을 때 더 큰 수를 찾게 된다.
또한, 따로 저장해두었던 숫자 중에서 5 또한 더 큰 수로 6을 만족한다는 것을 찾을 수 있다.

이후 6과 2를 비교하여 더 이상 입력에서 더 큰 수를 찾을 수 없다는 것을 알 수 있다.
결과적으로 9, 6, 2가 따로 저장된 공간에 남아 있게 되고, 이 숫자들의 인덱스에 `-1`을 삽입한다.

![뒤에있는큰수찾기-04.png](/assets/images/posts_img/algorithm-programmers/뒤에있는큰수찾기-04.png)

이때 <b>따로 저장되는 공간</b>으로 Priority Queue를 쓸 수 있고,
값과 인덱스를 배열로 저장하여 사용할 수 있다.

<br>

#### 시간 복잡도

먼저 입력 `numbers`의 모든 숫자를 반복해야 한다. &rarr; O(n)

또한 매 번 Priority Queue가 동작한다. &rarr; O(log n)

결과적으로 <b>O(n * log n)</b>이다.

<br>

#### 구현 코드

```java
package level2.뒤에있는큰수찾기;

import java.util.Comparator;
import java.util.PriorityQueue;

public class solve_1st_01 {
    class Solution {
        public int[] solution(int[] numbers) {
            int[] answer = new int[numbers.length];


            PriorityQueue<Integer[]> q = new PriorityQueue<>(new Comparator<Integer[]>(){
                @Override
                public int compare(Integer[] n1, Integer[] n2) {
                    return n1[0] - n2[0];
                }
            });

            for (int i = 0; i < numbers.length; i++) {
                Integer num = numbers[i];
                q.add(new Integer[]{num, i});

                while (!q.isEmpty()) {
                    Integer[] n = q.peek();
                    if (num > n[0]) {
                        n = q.poll();
                        answer[n[1]] = num;
                    } else {
                        break;
                    }
                }
            }

            while (!q.isEmpty()) {
                Integer[] n = q.poll();
                answer[n[1]] = -1;
            }

            return answer;
        }
    }
}
```

<br>

### 풀이 2 : stack

위의 따로 저장하는 공간은 Priority Queue를 사용하였다.

그 외에도 Stack을 사용할 수 있는데, 위에 설명한 그림을 그대로 구현할 수 있는 게 Stack 자료 구조이다.

오히려 정렬하는 시간을 없앨 수 있어서 더 효율적이다.

<br>

#### 시간 복잡도

먼저 입력 `numbers`의 모든 숫자를 반복해야 한다. &rarr; O(n)

또한, Priorty Queue 에서의 정렬을 안해도 되기 때문에 Stack 크기 m 에 대해서만 고려한다.

결과적으로  O(n) < <b> O(n * m) </b> < O(n * log n)이다.

<br>

#### 구현 코드

```java
package level2.뒤에있는큰수찾기;

import java.util.Arrays;
import java.util.Stack;

public class solve_1st_02 {
    class Solution {
        public int[] solution(int[] numbers) {
            int[] answer = new int[numbers.length];
            Arrays.fill(answer, -1);
            Stack<Integer> s = new Stack<>();
            s.push(0);
            for(int i = 1; i < numbers.length; i++){
                while(!s.isEmpty()){
                    int idx = s.pop();
                    if(numbers[i] > numbers[idx]){ // 뒤가 더 클때
                        answer[idx] = numbers[i];
                    } else { // 앞이 더 크거나 같을 때
                        s.push(idx);
                        break;
                    }
                }
                s.push(i);
            }

            return answer;
        }
    }
}
```

<br>

### 정합성 비교 : 풀이 1 vs 풀이 2


|          | 풀이 1                 | 풀이 2                     |
| -------- | -------------------- | ------------------------ |
| 테스트 1 〉  | 통과 (1.48ms, 79MB)    | 통과 (0.17ms, 74.2MB)  |
| 테스트 2 〉  | 통과 (0.77ms, 70.8MB)  | 통과 (0.11ms, 74.5MB)  |
| 테스트 3 〉  | 통과 (0.96ms, 102MB)   | 통과 (0.17ms, 74.2MB)  |
| 테스트 4 〉  | 통과 (1.11ms, 77.9MB)  | 통과 (0.36ms, 79.3MB)  |
| 테스트 5 〉  | 통과 (1.80ms, 76.6MB)  | 통과 (1.71ms, 80.5MB)  |
| 테스트 6 〉  | 통과 (7.68ms, 84.2MB)  | 통과 (7.67ms, 77.2MB)  |
| 테스트 7 〉  | 통과 (6.45ms, 84.6MB)  | 통과 (5.11ms, 85.7MB)  |
| 테스트 8 〉  | 통과 (22.86ms, 105MB)  | 통과 (16.64ms, 106MB)  |
| 테스트 9 〉  | 통과 (33.91ms, 91.5MB) | 통과 (23.93ms, 107MB)  |
| 테스트 10 〉 | 통과 (51.68ms, 114MB)  | 통과 (38.58ms, 117MB)  |
| 테스트 11 〉 | 통과 (33.91ms, 102MB)  | 통과 (35.20ms, 107MB)  |
| 테스트 12 〉 | 통과 (57.87ms, 116MB)  | 통과 (41.57ms, 129MB)  |
| 테스트 13 〉 | 통과 (77.56ms, 131MB)  | 통과 (35.47ms, 120MB)  |
| 테스트 14 〉 | 통과 (78.20ms, 155MB)  | 통과 (50.94ms, 150MB)  |
| 테스트 15 〉 | 통과 (151.83ms, 200MB) | 통과 (84.04ms, 218MB)  |
| 테스트 16 〉 | 통과 (136.64ms, 216MB) | 통과 (76.46ms, 200MB)  |
| 테스트 17 〉 | 통과 (138.06ms, 205MB) | 통과 (97.92ms, 206MB)  |
| 테스트 18 〉 | 통과 (141.21ms, 212MB) | 통과 (112.57ms, 199MB) |
| 테스트 19 〉 | 통과 (135.07ms, 204MB) | 통과 (90.04ms, 203MB)  |
| 테스트 20 〉 | 통과 (143.34ms, 241MB) | 통과 (102.45ms, 226MB) |
| 테스트 21 〉 | 통과 (544.92ms, 228MB) | 통과 (134.70ms, 188MB) |
| 테스트 22 〉 | 통과 (155.79ms, 183MB) | 통과 (67.97ms, 192MB)  |
| 테스트 23 〉 | 통과 (144.35ms, 270MB) | 통과 (122.95ms, 223MB) |


<hr>
<b>Reference</b>  
