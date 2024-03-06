---
title: "[OOP] 10. getter와 setter"
excerpt: "OOP-10 객체지향적으로 프로그래밍하기 위한 getter와 setter에 대해서 알아본다."

categories:
  - OOP
tags:
  - [OOP]

permalink: /oop/oop-10/

toc: true
toc_sticky: true

date: 2024-01-25
last_modified_at: 2024-01-25
---

&nbsp;안녕하세요, 새하입니다. 저번 게시물에 이어서 객체지향적으로 프로그래밍하기 위한 
<b>getter와 setter</b>에 대해서 정리해보겠습니다.

<br>

&nbsp;지난 게시물에서는 if문을 제거하여 가독성 높은 코드를 만드는 방법에 대해서 알아보았습니다. 
이번에도 마찬가지로 가독성 높은 코드를 만들기 위한 방법으로 getter와 setter를 활용하는 법을 알아보겠습니다. 

<br>

## 캡슐화가 깨진 CalculateCommand

```java
public class Client{
    public int someMethod(CalculateCommand calculateCommand) {
        CalculateType calculateType = calculateCommand.getCalculateType();
        int num1 = calculateCommand.getNum1();
        int num2 = calculateCommand.getNum2();
        
        int result = calculateCommand.calculate(num1, num2);
        
        return result;
    }
}
```

위 코드는 이전 게시물에서 마지막에 살펴보았던 `Client` 클래스에서 `CalculateCommand`를 사용하는 코드입니다. 
이 코드의 문제점은 바로 <b>`Cilent` 코드가 `CalculateCommand`를 너무 잘 알고 있다</b>는 점입니다. 
구체적으로는 내부 필드가 무엇인지 알고 있습니다. 

<br>

우리는 `getter`를 사용하고 있기 때문에 필드에 직접 접근하지 않습니다. 하지만 
간접적으로 필드를 알고 있는 것이나 마찬가지인 상황입니다. 그 예로, `CalculateCommand`의 필드명을 바꾸거나 삭제하면 
`Client` 코드로 그 사항이 전달됩니다. 

<br>

### CalculateCommand 캡슐화하기

이러한 문제는 `someMethod` 메서드의 로직을 `CalculateCommand` 내부로 보냄으로써 해결할 수 있습니다. 

```java
public class CalculateCommand {
    private CalculateType calculateType;
    private int num1;
    private int num2;

    public CalculateCommand(CalculateType calculateType, int num1, int num2) {
        if(calculateType == null) {
            throw new RuntimeException("CalculateType은 필수 값 입니다.");
        }

        if(calculateType.equals(CalculateType.DIVIDE) && num2 == 0) {
            throw new RuntimeException("0으로 나눌 수 없습니다.");
        }

        this.calculateType = calculateType;
        this.num1 = num1;
        this.num2 = num2;
    }

    public int getCalculateResult() {
        CalculateType calculateType = this.calculateType;
        int num1 = this.num1;
        int num2 = this.num2;

        int result = calculateType.calculate(num1, num2);

        return result;
    }
}
```

<br>

그럼 `Client` 클래스에서도 다음과 같이 사용하면 됩니다.

```java
public class Client {
    public int someMethod(CalculateCommand calculateCommand) {
        int result = calculateCommand.getCalculateResult();

        return result;
    }
}
```

이러한 `CalculateCommand`의 코드를 <b>캡슐화가 잘 되어있다</b>라고 할 수 있습니다. 
여기서 캡슐화가 잘 되어있다라는 말은 어떤 의미를 지닐까요? 

<br>

## 캡슐화

캡슐화가 잘 되어있다라는 말의 의미는 클래스 내부 요소를 외부에 공개하지 않고, 제한된 요소만 외부에 공개하는 환경을 
잘 마련해놓은 것을 의미합니다. 또한 이러한 코드는 <b>결합도는 낮추고, 응집도는 높아진 코드</b>라고도 말할 수 있습니다. 

<br>

### 결합도와 응집도

```java
public class CalculateCommand {
    private CalculateType calculateType;
    private int num1;
    private int num2;

    public CalculateCommand(CalculateType calculateType, int num1, int num2) {
        if(calculateType == null) {
            throw new RuntimeException("CalculateType은 필수 값 입니다.");
        }

        if(calculateType.equals(CalculateType.DIVIDE) && num2 == 0) {
            throw new RuntimeException("0으로 나눌 수 없습니다.");
        }

        this.calculateType = calculateType;
        this.num1 = num1;
        this.num2 = num2;
    }

    public CalculateType getCalculateType() {
        return calculateType;
    }

    public int getNum1() {
        return num1;
    }

    public int getNum2() {
        return num2;
    }
}
```

다시 위의 코드로 돌아가서 살펴보면, `Client`는 `CalculateCommand`의 getter를 사용합니다. 
이렇게 되면 `CalculateCommand`의 필드명 수정/삭제 등의 변화에 영향을 크게 받을 수 밖에 없습니다. 
이러한 코드를 <b>결합도가 높은 코드</b>라고 합니다. 이는 코드 변경을 어렵게 만들고, 
한 쪽에서 일어난 변경을 다른 쪽으로 전파하는 문제가 있습니다. 결과적으로 우리는 결합도가 낮은 코드를 작성하기 위해서 
노력해야 합니다.

<br>

```java
public class Client{
    public int someMethod(CalculateCommand calculateCommand) {
        CalculateType calculateType = calculateCommand.getCalculateType();
        int num1 = calculateCommand.getNum1();
        int num2 = calculateCommand.getNum2();

        int result = calculateCommand.calculate(num1, num2);

        return result;
    }
}
```

또한 `Client` 클래스에서는 `CalculateCommand`의 필드와 계산 로직을 따로 호출하여 사용하고 있습니다. 
이러한 코드를 <b>응집도가 낮은 코드</b>라고 부릅니다. 

<br>

결과적으로 객체지향적인 코드는 결합도는 낮고, 응집도는 높은 코드를 말합니다. 이러한 코드가 변경에 강하기 때문에 
유지보수 측면에서도 유리합니다. 

<br>

## 생성자와 setter

```java
public class CalculateCommand {
    //생략

// 초기화 방법 1. 생성자
    public CalculateCommand(CalculateType calculateType, int num1, int num2) {
        if(calculateType == null) {
            throw new RuntimeException("CalculateType은 필수 값 입니다.");
        }
    
        if(calculateType.equals(CalculateType.DIVIDE) && num2 == 0) {
            throw new RuntimeException("0으로 나눌 수 없습니다.");
        }
    
        this.calculateType = calculateType;
        this.num1 = num1;
        this.num2 = num2;
    }
// 초기화 방법 2. setter
    public void setCalculateType(CalculateType calculateType) {
        this.calculateType = calculateType;
    }

    public void setNum1(int num1) {
        this.num1 = num1;
    }

    public void setNum2(int num2) {
        this.num2 = num2;
    }

    // 생략
}
```

위의 코드처럼 생성자에서 setter로 바꾸면 어떤 문제가 있을까요? 
가장 먼저 떠오르는 건 getter 때와 마찬가지로 `CalculateCommand`의 내부 필드가 외부로 간접적으로 노출된다는 문제점이 있습니다. 
그보다 더한 문제는 불완전한 `CalculateCommand` 인스턴스가 생성될 가능성이 있다는 것입니다. 바로 아래의 코드로 확인할 수 있습니다. 

```java
public class SetterCodeExampleMain {
    public static void main(String[] args) {
        CalculateCommand calculateCommand = new CalculateCommand();

//        실수로 아래 코드를 누락한다면?
//        calculateCommand.setCalculateType(CalculateType.ADD);
        calculateCommand.setNum1(100);
        calculateCommand.setNum2(3);

        Client client = new Client();
        int result = client.someMethod(calculateCommand);

        System.out.println(result);
    }
}
```

특히 불완전한 인스턴스를 생성하면 안되는 경우에는 생성자를 활용해야 한다는 것을 알 수 있습니다.

<br>

## 기술적인 문제로 getter가 필요한 경우

앞서 getter와 setter를 제거하는 방법과 이유를 알아보았습니다. 하지만 
기술적으로 getter를 사용해야 하는 경우도 있습니다. 

<br>

외부와 접점이 있는 클래스와 내부에서 사용하는 데이터 클래스를 나누는 경 우입니다. 
외부와 접점이 있는 클래스를 보통 <b>DTO</b>라고 지칭합니다. 
DTO로 받은 외부 값은 내부에서 사용하기 위해서 변환되는 과정을 거칩니다. 
이때 DTO의 getter가 필요합니다. DTO의 getter를 통해 가져온 값을 내부 클래스의 파라미터 값으로 받아 사용하는 것입니다. 

<br>

또 다른 경우는 DTO가 HTTP 요청의 반환값으로 사용되는 경우입니다. 
HTTP 요청의 접점인 Controller의 반환값으로 DTO가 사용될 수 있습니다. 이때 또한 getter가 반드시 필요합니다. 
DTO에 getter가 하나도 없으면 HTTP 요청에 실패하게 됩니다. 
특히 이 경우가 getter를 만들지 않아서 HTTP 요청에 실패하게 되어 삽질을 하는 경우가 많다고 합니다. 







<br>

<hr>
<b>Reference</b>  
[실무 자바 개발을 위한 OOP와 핵심 디자인 패턴](https://school.programmers.co.kr/learn/courses/17778/17778-%EC%8B%A4%EB%AC%B4-%EC%9E%90%EB%B0%94-%EA%B0%9C%EB%B0%9C%EC%9D%84-%EC%9C%84%ED%95%9C-oop%EC%99%80-%ED%95%B5%EC%8B%AC-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)  
