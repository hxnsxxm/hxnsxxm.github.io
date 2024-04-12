---
title: "[Level 2] 오픈채팅방, Java"
excerpt: "프로그래머스 레벨 2 : 오픈채팅방 풀이 및 해설"

categories:
  - Programmers
tags:
  - [algorithm, programmers]

permalink: /algorithm/programmers/오픈채팅방

toc: true
toc_sticky: true

date: 2024-04-12
last_modified_at: 2024-04-12
---

## 문제

### 링크

[프로그래머스/오픈채팅방](https://school.programmers.co.kr/learn/courses/30/lessons/42888)

<br>

### 문제 설명

카카오톡 오픈채팅방에서는 친구가 아닌 사람들과 대화를 할 수 있는데, 본래 닉네임이 아닌 가상의 닉네임을 사용하여 채팅방에 들어갈 수 있다.

신입사원인 김크루는 카카오톡 오픈 채팅방을 개설한 사람을 위해, 다양한 사람들이 들어오고, 나가는 것을 지켜볼 수 있는 관리자창을 만들기로 했다. 채팅방에 누군가 들어오면 다음 메시지가 출력된다.

"[닉네임]님이 들어왔습니다."

채팅방에서 누군가 나가면 다음 메시지가 출력된다.

"[닉네임]님이 나갔습니다."

채팅방에서 닉네임을 변경하는 방법은 다음과 같이 두 가지이다.

- 채팅방을 나간 후, 새로운 닉네임으로 다시 들어간다.
- 채팅방에서 닉네임을 변경한다.

닉네임을 변경할 때는 기존에 채팅방에 출력되어 있던 메시지의 닉네임도 전부 변경된다.

예를 들어, 채팅방에 "Muzi"와 "Prodo"라는 닉네임을 사용하는 사람이 순서대로 들어오면 채팅방에는 다음과 같이 메시지가 출력된다.

"Muzi님이 들어왔습니다."  
"Prodo님이 들어왔습니다."

채팅방에 있던 사람이 나가면 채팅방에는 다음과 같이 메시지가 남는다.

"Muzi님이 들어왔습니다."  
"Prodo님이 들어왔습니다."  
"Muzi님이 나갔습니다."

Muzi가 나간후 다시 들어올 때, Prodo 라는 닉네임으로 들어올 경우 기존에 채팅방에 남아있던 Muzi도 Prodo로 다음과 같이 변경된다.

"Prodo님이 들어왔습니다."  
"Prodo님이 들어왔습니다."  
"Prodo님이 나갔습니다."  
"Prodo님이 들어왔습니다."

채팅방은 중복 닉네임을 허용하기 때문에, 현재 채팅방에는 Prodo라는 닉네임을 사용하는 사람이 두 명이 있다. 이제, 채팅방에 두 번째로 들어왔던 Prodo가 Ryan으로 닉네임을 변경하면 채팅방 메시지는 다음과 같이 변경된다.

"Prodo님이 들어왔습니다."  
"Ryan님이 들어왔습니다."  
"Prodo님이 나갔습니다."  
"Prodo님이 들어왔습니다."

채팅방에 들어오고 나가거나, 닉네임을 변경한 기록이 담긴 문자열 배열 record가 매개변수로 주어질 때, 모든 기록이 처리된 후, 최종적으로 방을 개설한 사람이 보게 되는 메시지를 문자열 배열 형태로 return 하도록 solution 함수를 완성하라.

<br>

### 입력 & 출력

#### 제한사항

- record는 다음과 같은 문자열이 담긴 배열이며, 길이는 `1` 이상 `100,000` 이하이다.
- 다음은 record에 담긴 문자열에 대한 설명이다.
  - 모든 유저는 [유저 아이디]로 구분한다.
  - [유저 아이디] 사용자가 [닉네임]으로 채팅방에 입장 - "Enter [유저 아이디] [닉네임]" (ex. "Enter uid1234 Muzi")
  - [유저 아이디] 사용자가 채팅방에서 퇴장 - "Leave [유저 아이디]" (ex. "Leave uid1234")
  - [유저 아이디] 사용자가 닉네임을 [닉네임]으로 변경 - "Change [유저 아이디] [닉네임]" (ex. "Change uid1234 Muzi")
  - 첫 단어는 Enter, Leave, Change 중 하나이다.
  - 각 단어는 공백으로 구분되어 있으며, 알파벳 대문자, 소문자, 숫자로만 이루어져있다.
  - 유저 아이디와 닉네임은 알파벳 대문자, 소문자를 구별한다.
  - 유저 아이디와 닉네임의 길이는 `1` 이상 `10` 이하이다.
  - 채팅방에서 나간 유저가 닉네임을 변경하는 등 잘못 된 입력은 주어지지 않는다.

#### 입출력 예

|record|result|
|---|---|
|`["Enter uid1234 Muzi", "Enter uid4567 Prodo","Leave uid1234","Enter uid1234 Prodo","Change uid4567 Ryan"]`|`["Prodo님이 들어왔습니다.", "Ryan님이 들어왔습니다.", "Prodo님이 나갔습니다.", "Prodo님이 들어왔습니다."]`|

<br>

## 풀이

우리는 채팅방 사용자들의 "유저 아이디"와 "닉네임"을 알 수 있다. 이것을 가지고 유저 아이디 별 닉네임을 별도로 관리한다.
이를 통해서 사용자들의 닉네임이 변경되어도 그 정보를 업데이트 할 수 있다.

또한, 관리자창에 닉네임을 그대로 사용하게 되면 닉네임이 변경될 때마다 일일이 수정해야 한다.
그러한 작업을 막기 위해서 관리자창의 사용자들은 유저 아이디로 기입하여 관리한다.

![오픈채팅방-01.png](/assets/images/posts_img/algorithm-programmers/오픈채팅방-01.png)

이후 최종적으로 메시지를 보게 될 때 해당 관리자창의 정보대로 닉네임을 매칭하여 반환한다.
이때 관리자창 `[1]`의 `null`이 의미하는 것은 Change이다. 이것은 건너뛴다.

<br>

### 시간 복잡도

입력 `record`의 길이가 n일 때, n을 중복없이 2번 반복한다. <b>O(n)</b>

<br>

### 구현 코드
```java
package level2.오픈채팅방;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class sovle_1st {
    class Solution {
        public String[] solution(String[] record) {
            int len = record.length;
            List<String> answer = new ArrayList<>();
            Map<String, String> map = new HashMap<>(); //유저아이디 : 닉네임
            String[][] admin = new String[len][2];

            for (int i = 0; i < len; i++) {
                String[] tmp = record[i].split(" ");

                if (tmp[0].equals("Enter")) {
                    map.put(tmp[1], tmp[2]);
                    admin[i][1] = "님이 들어왔습니다.";
                } else if (tmp[0].equals("Leave")) {
                    admin[i][1] = "님이 나갔습니다.";
                } else if (tmp[0].equals("Change")) {
                    map.put(tmp[1], tmp[2]);
                }
                admin[i][0] = tmp[1];
            }

            for (int i = 0; i < len; i++) {
                if (admin[i][1] == null)
                    continue;
                answer.add(map.get(admin[i][0]) + admin[i][1]);
            }

            return answer.stream().toArray(String[]::new);
        }
    }
}
```


<hr>
<b>Reference</b>  
