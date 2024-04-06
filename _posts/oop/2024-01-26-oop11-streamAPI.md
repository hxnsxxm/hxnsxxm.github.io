---
title: "[OOP] 11. Stream API"
excerpt: "OOP-11 객체지향적으로 프로그래밍하기 위한 자바 문법 중 stream API에 대해서 알아본다."

categories:
  - OOP
tags:
  - [OOP]

permalink: /oop/oop-11/

toc: true
toc_sticky: true

date: 2024-01-26
last_modified_at: 2024-01-26
---

&nbsp;안녕하세요, 새하입니다. 저번 게시물에 이어서 객체지향적으로 프로그래밍하기 위한 
<b>stream API</b>에 대해서 정리해보겠습니다.

<br>

&nbsp;Stream API 그 자체는 객체지향 프로그래밍과 연관이 크게 없습니다. 그러나 stream API를 활용한 코드는 
객체지향적인 코드와 함께 사용되었을 때, 가독성을 크게 향상시켜 줍니다.

<br>

## stream API

Stream API는 일반적으로 `for`, `if`문을 대체할 수 있습니다. 이번 게시물에서는 stream API 중에서도 
실무에서 가장 많이 사용된다는 `forEach`, `filter`, `map` 세 가지에 대해서 정리해보겠습니다.

<br>

### stream API 적용 대상

`forEach`, `filter`, `map`에 대해서 알아보기 전에 stream API를 적용할 수 있는 대상을 먼저 정리하겠습니다.  

![img.png](/assets/images/posts_img/study-oop/streamAPI-01.png)

stream API는 Collection 인터페이스를 구현하는 구현체들에서 사용할 수 있습니다. `Map`은 Collection 인터페이스를 
상속받지 않기 때문에 stream API를 사용할 수 없습니다.  
물론 우회하여 `map`에서도 사용하는 방법이 있긴 합니다. 이건 아래에서 다시 알아보겠습니다. 

<br>

## stream API 사용

```java
public static void main(String[] args) {
    List<Integer> integerList = new ArrayList<>();
    
    integerList.stream()...;
}
```

Collection 구현체에서 `.stream()` 메서드를 호출하여 반환된 값을 가지고 이어서 stream API를 실행시키면 됩니다. 

<br>

### forEach - 반복시키기

```java
List<Integer> integerList = new ArrayList<>();

integerList.add(10);
integerList.add(20);
integerList.add(30);
integerList.add(40);
integerList.add(50);
integerList.add(60);
integerList.add(70);


//반복문 사용하여 출력
for (int i = 0; i < integerList.size(); i++) {
    System.out.println(integerList.get(i));
}
```

위 코드의 `integerList`의 모든 값을 출력하라고 한다면 `for` 반복문을 사용하여 출력할 수 있습니다. 

다음은 stream API를 사용하여 리스트의 원소들을 순서대로 출력하는 코드입니다.  

```java
integerList.stream().forEach(integer -> {
    System.out.println(integer);
});
```

<br>

### for문과 forEach의 차이점

```java
for (int i = 0; i < integerList.size() - 1; i++) {
    System.out.println(integerList.get(i));
    System.out.println(integerList.get(i + 1));
}
```

먼저 `forEach`를 통해서 반복하면 `i` 변수같은 인덱스가 없습니다. 따라서 현재 반복하고 있는 요소 외에 요소에 
직접 접근하는 것이 불가능합니다. 

또한 `for`문은 실행 도중에 `break`가 가능합니다. 
`forEach`는 반복문이 아니라 함수이기 때문에 도중에 중단하는 것이 불가능합니다. 
물론 함수이기 때문에 return은 되지만 `forEach`를 끝내는 것이 아닙니다. 단지 해당 원소에 대한 처리 결과가 반환될 뿐입니다. 
반복문에서의 `continue`와 비슷한 역할입니다.  

`forEach`를 중단하는 방법이 있긴 합니다.  

```java
integerList.stream().forEach(integer -> {
    System.out.println(integer);
    
    if (integer.equals(40))
        throw new RuntimeException();
});
```

바로 위의 코드처럼 특정 조건일 때 예외를 던지게 하면 됩니다. 
다만 이렇게 작성하는 것은 일반적이지 않습니다. stream API의 `filter`를 이용하여 그 역할을 대신할 수 있습니다. 

<br>

### filter - 조건에 맞는 요소만 뽑기

```java
List<Integer> integerList = new ArrayList<>();

integerList.add(10);
integerList.add(20);
integerList.add(30);
integerList.add(40);
integerList.add(50);
integerList.add(60);
integerList.add(70);

//반복문 - break
Integer findNumber = null;

for (int i = 0; i < integerList.size() - 1; i++) {
    System.out.println(integerList.get(i));
    
    if (integerList.get(i).equals(40)) {
        findNumber = integerList.get(i);
        break;
    }
}

//stream API - filter
Integer findNumber = integerList.steram().filter(integer -> {
    System.out.println(integer);
    
    if (integer.equals(40))
        return true;
    
    return false;
}).findAny().get();
```

`filter`는 조건을 만족하는 요소들만 뽑아서 <b>새로운 stream</b>을 만들어 줍니다. 
이렇게 새로 생성된 stream에 대해서는 다시 stream API를 적용할 수 있습니다.  

`for`문과 `filter`의 또 다른 차이점은 결과를 저장하는 변수 선언 시점입니다. 
`for` 반복문에서는 반복이 시작하기 전에 `Integer findNumber = null`로 변수를 선언해 줍니다. 
반면에 `filter`는 `.findAny().get()`을 통해 선언과 동시에 초기화할 수 있습니다.  

<br>

추가적으로 `findAny()`는 조건을 만족하는 요소를 찾게 되면 그 이후 요소들은 반복하지 않고 결과를 반환해줍니다.  

<br>

### map - 요소를 매핑시키기

stream API에서 `map`은 요소들에 특정한 연산을 진행한 후 새로운 stream을 생성하는 역할을 합니다. 

```java
List<Integer> integerList = new ArrayList<>();

integerList.add(10);
integerList.add(20);
integerList.add(30);
integerList.add(40);
integerList.add(50);
integerList.add(60);
integerList.add(70);

//반복문 사용
List<Integer> x10IntegerList = new ArrayList<>();

for (int i = 0; i < integerList.size(); i++) {
    x10IntegerList.add(integerList.get(i) * 10);
}

//stream API - map
List<Integer> x10IntegerList = integerList.stream()
        .map(integer -> integer * 10)
        .toList();  //stream을 List로 변환하는 메서드
```

<br>

### filter와 map의 결합

리스트 요소들 중에서  
1. 짝수만 뽑아  
2. 곱하기 10  
을 한 리스트를 만들고 싶다고 가정해 보겠습니다.  

먼저 `for`, `if`를 사용한 코드입니다.  
```java
int[] integerArray = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
List<Integer> integerList = Arrays.stream(integerArray)
        .boxed()
        .toList();

//1. 짝수만 뽑기
List<Integer> evennumberList = new ArrayList<>();

for (int i = 0; i < integerList.size(); i++) {
    if (integerList.get(i) % 2 == 0)
        evenNumberList.add(integerList.get(i));
}

//2. 곱하기 10
List<Integer> evenX10NumberList = new ArrayList<>();

for (int i = 0; i < evenNumberList.size(); i++) {
    evenX10NumberList.add(evenNumberList.get(i) * 10);
}
```

<br>

위의 코드를 stream API를 사용한 코드로 바꿔보겠습니다.  
```java
int[] integerArray = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
List<Integer> integerList = Arrays.stream(integerArray)
        .boxed()
        .toList();

List<Integer> evenNumberList = integerList.stream()
        .filter(integer -> integer % 2 == 0)
        .toList();

List<Integer> evenX10NumberList = evenNumberList.stream()
        .map(integer -> integer * 10)
        .toList();
```

`filter`로 짝수를 골라내고, `map`으로 곱하기 10을 진행하였습니다. 
이러한 작업은 다음과 같이 합쳐서 진행할 수 있습니다.  

```java
int[] integerArray = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
List<Integer> integerList = Arrays.stream(integerArray)
        .boxed()
        .toList();

List<Integer> evenX10NumberList = integerList.stream()
        .filter(integer -> integer % 2 == 0)
        .map(integer -> integer * 10)
        .toList();
```

<br>

## Map 인터페이스에 대해서 stream API 사용하기

```java
Map<String, Integer> someMap = new HashMap<>();
Set<Map.Entry<String, Integer>> entries = someMap.entrySet();
Set<String> keySet = somemap.keySet();
List<Integer> valueList = someMap.values().stream().toList();

entries.stream().forEach(stringIntegerEntry -> {
    String key = stringIntegerEntry.getKey();
    Integer value = stringIntegerEntry.getValue();
    
    //key, value 사용
});
```

`Map`은 Collection 인터페이스를 상속받지 않고 독자적인 인터페이스를 사용하고 있기 때문에 
stream API를 사용할 수 없습니다.  

따라서 stream API를 사용하기 위해서는 `Set`으로 만들어줘야 합니다.




<br>

<hr>
<b>Reference</b>  
[실무 자바 개발을 위한 OOP와 핵심 디자인 패턴](https://school.programmers.co.kr/learn/courses/17778/17778-%EC%8B%A4%EB%AC%B4-%EC%9E%90%EB%B0%94-%EA%B0%9C%EB%B0%9C%EC%9D%84-%EC%9C%84%ED%95%9C-oop%EC%99%80-%ED%95%B5%EC%8B%AC-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)  
