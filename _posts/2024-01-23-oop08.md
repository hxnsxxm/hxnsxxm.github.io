---
title: "[OOP] 8. Optional"
excerpt: "OOP-08 객체지향적으로 프로그래밍하기 위한 Optional에 대해서 알아본다."

categories:
  - OOP
tags:
  - [OOP]

permalink: /oop/oop-08/

toc: true
toc_sticky: true

date: 2024-01-23
last_modified_at: 2024-01-23
---

&nbsp;안녕하세요, 새하입니다. 저번 게시물에 이어서 객체지향적으로 프로그래밍하기 위한 
Java 문법 중 Optional에 대해서 정리해보겠습니다.

<br>

&nbsp;<b>Optional</b>은 자바에서 비어 있는 값을 의미하는 `Null`을 처리하기 위한 문법입니다.
자바로 코드를 작성할 때 한 번쯤은 `NullPointerException`을 경험하게 됩니다.
이러한 `NullPointerException`을 피하려면 `Null` 체크를 해줘야 하는데, 이때 Optional을 활용하면
가독성 높은 코드로 만들 수 있습니다. 이번에는 Optional을 통해서 비어 있는 값을 다루는 방법을 정리해보겠습니다.

<br>

## Optional

### NullPointerException이 발생하는 코드

```java
pulbic static void main(String[] args) {
    String string = getNullString();

    System.out.println("string="+string);

    System.out.println(string.toUpperCase());
}

private static String getNullString() {
    return null;
}
```

위 코드에서는 `string.toUpperCase()`에서 NullPointerException이 발생합니다. 이러한 NullPointerException은 
`Null`이 들어 있는 레퍼런스 변수를 대상으로 필드 참조나 메서드 호출을 할 때 발생합니다. 

<br>

NullPointerException이 발생하지 않도록 코드를 바꿔보면 아래와 같이 `Null`체크를 하면 됩니다.

```java
pulbic static void main(String[] args) {
    String string = getNullString();

    System.out.println("string="+string);

    if (string != null)
        System.out.println(string.toUpperCase());
}

private static String getNullString() {
    return null;
}
```

이제부터는 <b>Optional</b>을 활용해서 위와 같은 코드를 가독성 높은 코드로 바꿔보겠습니다.

<br>

### Optional 사용하는 코드로 변경

#### Optional 사용 X

```java
public class MapRepository {

    private Map<String, String> map = new HashMap<>();

    MapRepository() {
        map.put("EXIST_KEY", "value");
    }

    public String getValue(String key) {
        return map.get(key);
    }
}
```

<br>

#### Optional 사용 O

```java
public class MapRepository {

    private Map<String, String> map = new HashMap<>();

    MapRepository() {
        map.put("EXIST_KEY", "value");
    }

    // Optional 사용 - Optional 객체 생성
    public Optional<String> getOptionalValue(String key) {
        return Optional.ofNullable(map.get(key));
    }

    public String getValue(String key) {
        return map.get(key);
    }
}
```

<br>

## Optional을 왜 사용하나?

Optional을 사용함으로써 API 사용을 세련되게 할 수 있다고 합니다.


```java
//Optional 사용 O
stream.getFirst().orElseThrow(() -> new MyFancyException())

//Optional 사용 X
Object obj = stream.getFirst();

if (obj == null)
    new MyFancyException();
```

위의 코드에서 Optional 사용 O/X의 차이는 (1) 무엇인가를 가져옴, (2) 그에 대한 예외 처리라는 두 역할이 
묶여 있느냐 아니냐의 차이를 만듭니다. Optional을 사용하게 되면 객체를 가져온 결과에 대해서 예외 처리를 진행함으로써 
한 줄로 코드를 만들 수 있게 해줍니다. 

<br>

### Optional이 보통 하는 일

그래서 Optional이 하는 일을 간단하게 정리해보면, 메서드 호출 결과가 Null이면, 

- 생성하여 메서드 호출을 계속한다. (나머지 로직을 실행)
- 혹은 예외를 던진다. 

두 역할을 가집니다. 그 중에서도 예외를 던지는 일이 많은데, 그 이유는 메서드를 호출하여 인스턴스를 조회하는 경우는 
의도 자체가 인스턴스 조회 혹은 인스턴스의 값 바꾸기인 경우가 많기 때문입니다.  

<br>

## Optional 인스턴스를 생성하는 방법

```java
MapRepository mapRepository = new MapRepository();
Optional<String> string = mapRepository.getOptionalValue("NOT_EXIST_KEY");

Optional.of(string); //객체가 null인 경우 NullPointerException -> Optional객체 생성 시점에 발생, 의도적
Optional.ofNullable(string); //null인 경우 비어 있는 Optional 객체 생성, 자주 사용
Optional.empty(); //null 반환하고 싶을 때 사용
```

<br>

### `.ifPresentOrElse()`

```java
public static void main(String[] args) {
    MapRepository mapRepository = new MapRepository();
    Optional<String> string = mapRepository.getOptionalValue("NOT_EXIST_KEY");

    string.ifPresentOrElse(
            str -> System.out.println(str.toUpperCase()),          // Optional이 Empty가 아닐 때 실행
            () -> {
                throw new RuntimeException("키가 존재하지 않습니다."); // Optional이 Empty일 때 실행
            }
    );
}
```

<br>

### `.orElseThrow()`

```java
public static void main(String[] args) {
    // 이전 코드보다 간결한 코드
    
    MapRepository mapRepository = new MapRepository();
    String string = mapRepository.getOptionalValue("NOT_EXIST_KEY").orElseThrow(
            () -> {throw new RuntimeException("키가 존재하지 않습니다.");}
    );

    System.out.println(string.toUpperCase());
}
```

<br>

### Optional 객체 직접 생성
```java
public static void main(String[] args) {
    // Optional의 정적 팩토리 메서드 사용
    
    MapRepository mapRepository = new MapRepository();
    String string = Optional.ofNullable(mapRepository.getValue("NOT_EXIST_KEY"))
            .orElseThrow(RuntimeException::new); //메서드 참조 문법

    System.out.println(string.toUpperCase());
}
```

<br>

## Optional의 실제 활용

### 1. 내가 쓰는 라이브러리의 메서드 반환 타입이 Optional인 경우

그대로 `.orElseThrow()`같은 메서드로 사용합니다.


<br>

### 2. 내가 쓰는 라이브러리의 메서드 반환 타입이 Optional이 아닌 경우

`Optional.ofNullable`로 Optional 객체 생성 후에 `.orElseThrow()` 같은 메서드 사용합니다.  

<br>

## Optional을 잘못 사용하고 있는 경우

```java
public static void main(String[] args) {
    MapRepository mapRepository = new MapRepository();
    Optional<String> string = mapRepository.getOptionalValue("NOT_EXIST_KEY");

    if(string.isPresent())
        System.out.println(string.get().toUpperCase());
    else
        throw new RuntimeException("키가 존재하지 않습니다.");
}
```

위의 코드에서는 Optional 인스턴스 생성 후 `Null` 체크와 완전히 동일하게 조건문으로 `.isPresent()`를 분기시키고 있습니다. 
대표적인 Optional 사용의 <b>안티 패턴</b>입니다. 

<br>

## stream API와 같이 사용

```java
public static void main(String[] args) {
    List<Integer> list = new ArrayList<>();

    list.add(1);
    list.add(2);
    list.add(3);
    list.add(4);
    list.add(5);

    Integer filteredInteger = list.stream()
            .filter(value -> value.equals(100))
            .findFirst()
            .orElseThrow(() -> {
                throw new RuntimeException("100에 해당하는 요소가 없습니다.");
            });

    System.out.println(filteredInteger);
}
```

<br>

<hr>
<b>Reference</b>  
[실무 자바 개발을 위한 OOP와 핵심 디자인 패턴](https://school.programmers.co.kr/learn/courses/17778/17778-%EC%8B%A4%EB%AC%B4-%EC%9E%90%EB%B0%94-%EA%B0%9C%EB%B0%9C%EC%9D%84-%EC%9C%84%ED%95%9C-oop%EC%99%80-%ED%95%B5%EC%8B%AC-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)  
