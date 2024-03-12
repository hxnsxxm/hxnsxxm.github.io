---
title: "[OOP] 12. Stream API와 Optional"
excerpt: "OOP-12 객체지향적으로 프로그래밍하기 위한 자바 문법 중 stream API와 Optional에 대해서 알아본다."

categories:
  - OOP
tags:
  - [OOP]

permalink: /oop/oop-12/

toc: true
toc_sticky: true

date: 2024-01-28
last_modified_at: 2024-01-28
---

&nbsp;안녕하세요, 새하입니다. 저번 게시물에 이어서 객체지향적으로 프로그래밍하기 위한 
<b>stream API와 Optional</b>에 대해서 정리해보겠습니다.

<br>

&nbsp;stream API는 그 자체로도 좋지만, Optional과 함께 사용되었을 때 가독성을 더욱 향상시켜 줍니다. 
따라서 이번 게시물에서는 여러 상황에서 stream API를 사용하지 않았을 때와 사용했을 때를 비교해보며 
stream API의 사용이 얼마나 이로운지 알아보겠습니다.  

<br>

## 실무에서 자주 발생하는 패턴

![img.png](/assets/images/posts_img/study-oop/streamAPI_Optional-01.png)

<br>

프로그램을 개발하다 보면 데이터를 조회한 후 그 결과에 따라서 예외 던지거나 다음 작업을 실행하는 패턴이 정말 많습니다.  

<br>

### 해당 패턴을 for문과 if문으로 처리하기

굳이 데이터베이스가 아니더라도 리스트(ArrayList)에서 `1234`라는 값을 조회해 없으면 예외를 던지고,
있으면 찾은 결과(`1234`)를 출력하는 상황을 가정해보겠습니다.  

<br>

```java
int[] integerArray = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
List<Integer> integerList = Arrays.stream(integerArray)
        .boxed()
        .toList();

Integer findNumber = null;

for (int i = 0; i < integerList.size(); i++) {
    if (integerList.get(i).equals(1234)) {
        findNumber = integerList.get(i);
        break;
    }
}

//찾는 값 없으면 예외 발생
if (findNumber == null)
    throw new RuntimeException();

//찾는 값 있으면 결과 출력
System.out.println("findNumber = " + findNumber);
```

<br>

### 해당 패턴을 stream API와 Optional로 처리하기

```java
int[] integerArray = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
List<Integer> integerList = Arrays.stream(integerArray)
        .boxed()
        .toList();

Integer findNumber = integerList.stream()
        .filter(integer -> {
            if (integer.equals(1234))
                return true;
            return false;
        })
        .findAny()
        .orElseThrow(); //NoSuchElementException

System.out.println("findNumber = " + findNumber);
```

<b>`for` + `if` 로 처리하기</b> vs <b>stream API + Optional</b>로 처리하기 중에서 
stream API + Optional 조합이 더 가독성이 뛰어나다고 생각됩니다.  

<br>

## 실전적인 예제 - 아이디 중복 검사에 활용하기

아이디 중복 검사 기능은 새로운 사용자가 서비스에 가입하는 과정에서 다른 회원과의 아이디가 중복되는 것을 방지합니다. 
이번에는 아이디 중복 검사에 stream API + Optional을 활용해보겠습니다.  

<br>

```java
public class User {
    private String id;
    private String name;
    
    public User(String id, String name) {
        this.id = id;
        this.name = name;
    }
    
    public boolean sameId(String idToFind) {
        return this.id.equals(idToFind);
    }
}
```

<br>

```java
String idtoFind = "programmers_user1";

List<User> list = new ArrayList<>();

list.add(new User("programmers_user1", "새하"));
list.add(new User("programmers_user2", "seha"));
list.add(new User("programmers_user3", "user"));

list.stream()
        .filter(user -> user.sameId(idToFind))
        .findAny()
        .ifPresentOrElse(user -> {
            throw new RuntimeException(idToFind + "는 이미 존재하는 아이디입니다.");
        }, () -> {
            System.out.println(idToFind + "는 사용할 수 있는 아이디입니다.");
        });
```

이 경우에는 `list`에 데이터가 존재하면 예외를 발생시켜야 하기 때문에 `ifPresentOrElse()`를 사용하였습니다. 
반면 존재하지 않는다면 중복되지 않는 아이디이기 때문에 사용할 수 있습니다.  

<br>

## stream API는 for문보다 느린가?

경우에 따라 다르지만, 실제로 느릴 수 있습니다. 이처럼 실제로 개발을 하다 보면 가독성이 높은 코드와 
성능이 좋은 코드 사이에서 고민하는 상황이 생길 겁니다. 
이때는 가독성을 우선하여 개발을 완료한 이후에 목표로 하는 성능에 맞춰서 튜닝을 하면 됩니다.  

물론 한 눈에 봐도 최악의 성능을 보이는 알고리즘이나 자료구조를 사용하는 건 지양해야 합니다. 
크게 차이를 느끼기 어렵다면 가독성을 우선시하는 것이 좋습니다.



<br>

<hr>
<b>Reference</b>  
[실무 자바 개발을 위한 OOP와 핵심 디자인 패턴](https://school.programmers.co.kr/learn/courses/17778/17778-%EC%8B%A4%EB%AC%B4-%EC%9E%90%EB%B0%94-%EA%B0%9C%EB%B0%9C%EC%9D%84-%EC%9C%84%ED%95%9C-oop%EC%99%80-%ED%95%B5%EC%8B%AC-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)  
