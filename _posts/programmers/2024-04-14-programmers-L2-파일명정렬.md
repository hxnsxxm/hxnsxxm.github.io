---
title: "[Level 2] 파일명 정렬, Java"
excerpt: "프로그래머스 레벨 2 : 파일명 정렬 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/파일명정렬

toc: true
toc_sticky: true

date: 2024-04-14
last_modified_at: 2024-04-14
---

## 문제

### 링크

[프로그래머스/[3차] 파일명 정렬](https://school.programmers.co.kr/learn/courses/30/lessons/17686)

<br>

### 문제 설명

세 차례의 코딩 테스트와 두 차례의 면접이라는 기나긴 블라인드 공채를 무사히 통과해 카카오에 입사한 무지는 파일 저장소 서버 관리를 맡게 되었다.

저장소 서버에는 프로그램의 과거 버전을 모두 담고 있어, 이름 순으로 정렬된 파일 목록은 보기가 불편했다. 파일을 이름 순으로 정렬하면 나중에 만들어진 ver-10.zip이 ver-9.zip보다 먼저 표시되기 때문이다.

버전 번호 외에도 숫자가 포함된 파일 목록은 여러 면에서 관리하기 불편했다. 예컨대 파일 목록이 ["img12.png", "img10.png", "img2.png", "img1.png"]일 경우, 일반적인 정렬은 ["img1.png", "img10.png", "img12.png", "img2.png"] 순이 되지만, 숫자 순으로 정렬된 ["img1.png", "img2.png", "img10.png", img12.png"] 순이 훨씬 자연스럽다.

무지는 단순한 문자 코드 순이 아닌, 파일명에 포함된 숫자를 반영한 정렬 기능을 저장소 관리 프로그램에 구현하기로 했다.

소스 파일 저장소에 저장된 파일명은 100 글자 이내로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.

파일명은 크게 HEAD, NUMBER, TAIL의 세 부분으로 구성된다.

- HEAD는 숫자가 아닌 문자로 이루어져 있으며, 최소한 한 글자 이상이다.
- NUMBER는 한 글자에서 최대 다섯 글자 사이의 연속된 숫자로 이루어져 있으며, 앞쪽에 0이 올 수 있다. `0`부터 `99999` 사이의 숫자로, `00000`이나 `0101` 등도 가능하다.
- TAIL은 그 나머지 부분으로, 여기에는 숫자가 다시 나타날 수도 있으며, 아무 글자도 없을 수 있다.

|파일명|HEAD|NUMBER|TAIL|
|---|---|---|---|
|`foo9.txt`|`foo`|`9`|`.txt`|
|`foo010bar020.zip`|`foo`|`010`|`bar020.zip`|
|`F-15`|`F-`|`15`|(빈 문자열)|

파일명을 세 부분으로 나눈 후, 다음 기준에 따라 파일명을 정렬한다.

- 파일명은 우선 HEAD 부분을 기준으로 사전 순으로 정렬한다. 이때, 문자열 비교 시 대소문자 구분을 하지 않는다. `MUZI`와 `muzi`, `MuZi`는 정렬 시에 같은 순서로 취급된다.
- 파일명의 HEAD 부분이 대소문자 차이 외에는 같을 경우, NUMBER의 숫자 순으로 정렬한다. 9 < 10 < 0011 < 012 < 13 < 014 순으로 정렬된다. 숫자 앞의 0은 무시되며, 012와 12는 정렬 시에 같은 같은 값으로 처리된다.
- 두 파일의 HEAD 부분과, NUMBER의 숫자도 같을 경우, 원래 입력에 주어진 순서를 유지한다. `MUZI01.zip`과 `muzi1.png`가 입력으로 들어오면, 정렬 후에도 입력 시 주어진 두 파일의 순서가 바뀌어서는 안 된다.

무지를 도와 파일명 정렬 프로그램을 구현하라.

<br>

### 입력 & 출력

#### 입력 형식

입력으로 배열 `files`가 주어진다.

- `files`는 1000 개 이하의 파일명을 포함하는 문자열 배열이다.
- 각 파일명은 100 글자 이하 길이로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.
- 중복된 파일명은 없으나, 대소문자나 숫자 앞부분의 0 차이가 있는 경우는 함께 주어질 수 있다. (`muzi1.txt`, `MUZI1.txt`, `muzi001.txt`, `muzi1.TXT`는 함께 입력으로 주어질 수 있다.)

#### 출력 형식

위 기준에 따라 정렬된 배열을 출력한다.

#### 입출력 예제

입력: ["img12.png", "img10.png", "img02.png", "img1.png", "IMG01.GIF", "img2.JPG"]  
출력: ["img1.png", "IMG01.GIF", "img02.png", "img2.JPG", "img10.png", "img12.png"]

입력: ["F-5 Freedom Fighter", "B-50 Superfortress", "A-10 Thunderbolt II", "F-14 Tomcat"]  
출력: ["A-10 Thunderbolt II", "B-50 Superfortress", "F-5 Freedom Fighter", "F-14 Tomcat"]

<br>

## 풀이

파일 이름은 HEAD, NUMBER, TAIL로 구분할 수 있고, 그 분류에 따라 정렬을 진행해야 한다.

<b>분류에 따라 정렬하기 위해서</b> 크게 3단계로 코드를 구현한다.

1. 파일 이름을 HEAD, NUMBER, TAIL을 구분한다.
2. 주어진 기준에 따라 HEAD와 NUMBER를 정렬한다.
3. 반환하기 위한 형태로 변환한다.

<br>

### 풀이 1

<b>1. 파일 이름을 HEAD, NUMBER, TAIL로 구분</b>

인덱스 변수 `s`, `e` 두 개를 이용해서 HEAD와 NUMBER를 구분한다.

![파일명정렬-01.png](/assets/images/posts_img/algorithm-programmers/파일명정렬-01.png)

먼저 문자열 파일명(`file`)의 문자를 돌면서 숫자를 만날 때까지 `e`를 증가시킨다.
그리고 `substring()` 메서드를 이용해서 HEAD를 추출한다.

![파일명정렬-02.png](/assets/images/posts_img/algorithm-programmers/파일명정렬-02.png)

다음 `s`를 `e`까지 증가시키고, NUMBER를 찾기 위해서 숫자가 끝날 때까지 `e`를 증가시키고,
다시 추출한다.

![파일명정렬-03.png](/assets/images/posts_img/algorithm-programmers/파일명정렬-03.png)

그 이후 모든 문자를 TAIL로 추출한다.

![파일명정렬-04.png](/assets/images/posts_img/algorithm-programmers/파일명정렬-04.png)

<b>2. 주어진 기준에 따라 HEAD와 NUMBER를 정렬</b>

모든 파일명에 대해서 HEAD, NUMBER, TAIL로 분류한 문자열 배열을 주어진 기준에 따라 정렬한다.

- 파일명은 우선 HEAD 부분을 기준으로 사전 순으로 정렬한다. 이때, 문자열 비교 시 대소문자 구분을 하지 않는다. MUZI와 muzi, MuZi는 정렬 시에 같은 순서로 취급된다.
- 파일명의 HEAD 부분이 대소문자 차이 외에는 같을 경우, NUMBER의 숫자 순으로 정렬한다. 9 < 10 < 0011 < 012 < 13 < 014 순으로 정렬된다. 숫자 앞의 0은 무시되며, 012와 12는 정렬 시에 같은 같은 값으로 처리된다.
- 두 파일의 HEAD 부분과, NUMBER의 숫자도 같을 경우, 원래 입력에 주어진 순서를 유지한다. `MUZI01.zip`과 `muzi1.png`가 입력으로 들어오면, 정렬 후에도 입력 시 주어진 두 파일의 순서가 바뀌어서는 안 된다.

```java
Arrays.sort(tmp, new Comparator<String[]>(){
    @Override
    public int compare(String[] s1, String[] s2) {
        String l1 = s1[0].toLowerCase();
        String l2 = s2[0].toLowerCase();

        if (l1.equals(l2))
            return Integer.parseInt(s1[1]) - Integer.parseInt(s2[1]);

        return l1.compareTo(l2);//s1[0].compareTo(s2[0]);
    }
});
```

<b>3. 반환하기 위한 형태로 변환</b>

마지막으로 위에서 정렬한 문자열 배열을 반환 형태에 맞춰 변환하면 된다.

<br>

#### 시간 복잡도

입력(`files`)의 길이가 n일 때, 모든 원소(길이 m)를 반복하면서 문자를 탐색하고 추출한다. &rarr; O(n * m)

`Arrays.sort`의 시간 복잡도는 평균적으로 O(nlog(n)), 최악 O(n^2)이다.

결과적으로 최악을 고려하면 <b>O(n^2)</b>인데, 최대 `n = 1000`이니 1,000,000회 정도 반복한다.

<br>

#### 구현 코드

```java
package level2.파일명정렬;

import java.util.Arrays;
import java.util.Comparator;

public class solve_1st_01 {
    class Solution {
        public String[] solution(String[] files) {
            int len = files.length;
            String[] answer = new String[len];
            String[][] tmp = new String[len][3];

            for (int i = 0; i < len; i++) {
                tmp[i] = classify(files[i]);
                //System.out.println(Arrays.toString(tmp[i]));
            }

            //정렬
            Arrays.sort(tmp, new Comparator<String[]>(){
                @Override
                public int compare(String[] s1, String[] s2) {
                    String l1 = s1[0].toLowerCase();
                    String l2 = s2[0].toLowerCase();

                    if (l1.equals(l2))
                        return Integer.parseInt(s1[1]) - Integer.parseInt(s2[1]);

                    return l1.compareTo(l2);//s1[0].compareTo(s2[0]);
                }
            });
            // for (String[] t : tmp)
            //     System.out.println(Arrays.toString(t));

            //answer 변환
            for (int i = 0; i < len; i++) {
                answer[i] = tmp[i][0] + tmp[i][1] + tmp[i][2];
            }

            return answer;
        }

        String[] classify(String file) {
            int len = file.length();
            String[] str = new String[3];

            int s = 0;
            int e = 0;

            while (!('0' <= file.charAt(e) && file.charAt(e) <= '9')) {
                e++;
            }
            str[0] = file.substring(s, e);
            s = e;

            while ('0' <= file.charAt(e) && file.charAt(e) <= '9') {
                e++;
                if (e >= len)
                    break;
            }
            if (e < len) {
                str[1] = file.substring(s, e);
                s = e;

                str[2] = file.substring(s);
            } else {
                str[1] = file.substring(s);
                str[2] = "";
            }

            return str;
        }
    }
}
```

<br>

### 풀이 2

풀이 1에서 설명한 HEAD, NUMBER, TAIL 분류 방법을 <b>정규식을 이용</b>해서 간단하게 구현할 수 있다.

<br>

#### 구현 코드

```java
package level2.파일명정렬;

import java.util.Arrays;
import java.util.Comparator;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class solve_1st_02 {
    class Solution {
        public String[] solution(String[] files) {
            Pattern p = Pattern.compile("([a-z\\s.-]+)([0-9]{1,5})");

            Arrays.sort(files, new Comparator<String>() {
                @Override
                public int compare(String s1, String s2) {
                    Matcher m1 = p.matcher(s1.toLowerCase());
                    Matcher m2 = p.matcher(s2.toLowerCase());
                    m1.find();
                    m2.find();

                    if(!m1.group(1).equals(m2.group(1))) {
                        return m1.group(1).compareTo(m2.group(1));
                    } else {
                        return Integer.parseInt(m1.group(2)) - Integer.parseInt(m2.group(2));
                    }
                }
            });

            return files;
        }
    }
}
```


<hr>
<b>Reference</b>  
