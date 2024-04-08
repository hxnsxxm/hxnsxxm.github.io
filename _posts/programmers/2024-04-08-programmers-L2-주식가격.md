---
title: "[Level 2] 주식가격, Java"
excerpt: "프로그래머스 레벨 2 : 주식가격 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/주식가격

toc: true
toc_sticky: true

date: 2024-04-08
last_modified_at: 2024-04-08
---

## 문제

### 링크

[프로그래머스/주식가격](https://school.programmers.co.kr/learn/courses/30/lessons/42584)

<br>

### 문제 설명

초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

<br>

### 입력 & 출력

#### 제한사항

- prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
- prices의 길이는 2 이상 100,000 이하입니다.

#### 입출력 예

|prices|return|
|---|---|
|[1, 2, 3, 2, 3]|[4, 3, 1, 1, 0]|

<br>

## 풀이

입력으로 주어진 <b>모든 주식 가격에 대해서</b> 가격이 떨어지지 않은 기간을 구해야 한다.

<b>2중 반복문</b>으로 구현할 수 있지만 시간 복잡도가 O(n^2)이기 때문에 입력 제한인 `100,000`에는 적합하지 않다.

그래서 다른 방법을 생각해야 한다.

![주식가격-01.png](/assets/images/posts_img/algorithm-programmers/주식가격-01.png)

예를 들어서, `{1, 2, 3, 2, 3}`의 입력이 주어졌을 때를 가정해 보겠다.

첫 번째 값 `1`에 대해서 뒤의 값들인 `2, 3, 2, 3`이 모두 떨어지지 않은 값이다.  
두 번째 값 `2`에 대해서 뒤의 값들인 `3, 2, 3`이 모두 떨어지지 않은 값이다.  
세 번째 값 `3`에 대해서 뒤의 값 `2`는 가격이 떨어진 값이다.

이로써 알 수 있는 사실은 뒤의 값이 가격이 떨어지는 값이라면 이후부터는 비교할 필요가 없다는 것이다.

즉, 첫 번째, 두 번째 값인 `1`과 `2`는 뒤의 값들에서 떨어지는 값을 만나지 않았기 때문에 인덱스 반복을 거듭해도
계속 비교 대상이 된다.

반면 세 번째 값인 `3`은 뒤에서 떨어지는 값을 만났기 때문에 이후부터는 비교하지 않아도 된다. (즉, 카운트를 하지 않아도 된다)

![주식가격-02.png](/assets/images/posts_img/algorithm-programmers/주식가격-02.png)

결과적으로 비교 대상들을 저장하고 있다가 떨어지는 값을 만났을 때 해당 인덱스 간격을 구하면 그것이 기간이 된다.

이는 `Stack` 자료구조로 구현할 수 있다.

![주식가격-03.png](/assets/images/posts_img/algorithm-programmers/주식가격-03.png)

인덱스 `0`, `1`, `2`를 반복하면서 가격이 떨어지지 않아 모두 `Stack`에 넣는다.

![주식가격-04.png](/assets/images/posts_img/algorithm-programmers/주식가격-04.png)

다음, 인덱스 `3`에 대해서 값 `2`가 `Stack`의 값보다 작기 때문에 가격이 떨어진다는 것을 알 수 있다.

그래서 해당 인덱스(`2`)가 가격이 떨어지지 않고 유지한 기간은 `3 - 2`가 되며, `Stack`에서 제거한다.

![주식가격-05.png](/assets/images/posts_img/algorithm-programmers/주식가격-05.png)

마지막으로 모든 주식가격을 비교했는데도, 떨어지는 값을 만나지 않아서 `Stack`에 여전히 들어 있는 값들에 대해서
처리를 해주면 완료이다.

![주식가격-06.png](/assets/images/posts_img/algorithm-programmers/주식가격-06.png)

<br>

### 시간 복잡도

입력 배열(`prices`)의 길이가 `n`이라고 할 때 모두 반복해야 한다. &rarr; O(n)

`Stack`의 크기와 가격 조건에 따라 달라진다.

시간 복잡도는 O(n) < <b>O(n * m)</b> < O(n ^ 2)

<br>

### 구현 코드
```java
package level2.주식가격;

import java.util.Stack;

public class solve_1st {
    class Solution {
        public int[] solution(int[] prices) {
            int len = prices.length;
            int[] answer = new int[len];
            Stack<Integer> stack = new Stack<>();

            stack.push(0);
            for (int i = 1; i < len; i++) {
                while (!stack.isEmpty() && prices[stack.peek()] > prices[i]) {
                    int idx = stack.pop();
                    answer[idx] = i - idx;
                }
                stack.push(i);
            }

            while (!stack.isEmpty()) {
                int idx = stack.pop();
                answer[idx] = len - idx - 1;
            }

            return answer;
        }
    }
}
```



<hr>
<b>Reference</b>  
