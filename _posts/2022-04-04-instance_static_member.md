---
title: "[Java] 인스턴스 멤버와 정적 멤버"
excerpt: ""

categories: Java
tags: [Java, 인스턴스 멤버, 정적 멤버, 클래스 멤버]

date: 2022-04-04
last_modified_at: 2022-04-04
---

> 클래스에 선언된 모든 필드와 메소드가 객체 내부에 포함되는 것은 아니다. 모든 객체에 공통적으로 포함되는 필드나 메소드까지 객체가 선언될 때마다 포함된다면 메모리가 낭비된다. 반대로 객체마다 필드값이 달라야 한다면 객체마다 가지고 있는 것이 맞다. 자바는 이런 경우를 위해서 인스턴스(instance)와 정적(static) 멤버로 클래스 멤버를 구분하고 있다.


## 인스턴스 멤버
> 객체마다 가지고 있는 멤버. 객체를 생성한 후 사용할 수 있는 필드와 메소드

### <U>선언</U>
```java
public class Car {
    // 필드
    int gas;
    
    // 메소드
    void setSpeed(int speed) {...}
}
```


### <U>사용</U>
여기서 gas 필드와 setSpeed 메소드의 경우, 인스턴스 멤버이기 때문에 외부 클래스에서 사용하기 위해서는 Car 객체(인스턴스)를 생성해줘야 한다.


```java
Car myCar = new Car();
myCar.gas = 100;
myCar.setSpeed(200);
```
위 코드가 실행된 이후에 메모리 상태를 보면, 
- JVM 스택 영역에 myCar라는 객체가 저장되게 되고,
- myCar는 힙 영역에 있는 Car 객체의 주소를 가리키게 된다.
- 이 때 힙 영역에는 field 값만 존재하게 되고, 메소드는 메소드 영역에 따로 저장되고 공유된다.

여기서 이상한 점은 메소드는 객체 내부에 존재하지 않고 메소드 영역에 따로 저장되어 공유한다는 점이다.  
이유는 동일한 코드 블록을 각각의 객체에 저장하는 것은 메모리 낭비이기 때문이다.


그럼 왜 인스턴스 메소드라는 용어가 붙었을까?


이유는 메모리 블록 내부에 인스턴스 필드 등이 사용될 수 있기 때문이다. 또한 인스턴스 메소드는 객체(인스턴스)가 생성되기 전에는 사용할 수 없기 때문이다.


### <U>this</U>
객체 내부에서 인스턴스 멤버에 접근하기 위해 this를 사용할 수 있다.
```java
Car(String model) {
    this.model = model;
}
```
여기서 this.model은 자신이 가지고 있는 model 필드라는 뜻이다. 


## 정적 멤버
> 클래스에 고정된 멤버로서 객체를 생성하지 않고 사용할 수 있는 필드와 메소드

### <U>선언</U>
```java
public class Calculator {
    // 정적 필드
    static double pi = 3.141592;

    // 정적 메소드
    static int plus(int a, int b) {...}
}
```
정정 멤버와 메소드 선언 시 static 키워드를 추가로 붙이면 된다.


보통 객체가 공통적으로 가지고 있어야 하는 데이터는 정적 멤버로 선언하고, 객체마다 가지고 있어야 하는 개별 데이터는 인스턴스 멤버로 선언한다.     


클래스 멤버 중 정적 멤버의 경우, 클래스 로더가 클래스(바이트 코드)를 로딩할 때 메소드 메모리 영역에 적재할 때 클래스별로 관리된다.  

![classloader](/assets/images/posts/2022-04-04/classloader.png){: .align-center}


### <U>사용</U>
정적 멤버를 사용하기 위해서는 클래스 이름과 함께 도트(.) 연산자가 필요하다.
```java
double result = 10 * Calculator.pi;
int plusResult = Calculator.plus(1, 2);
```

정적 멤버의 경우 원칙적으로는 클래스 이름을 바탕으로 접근해야 하지만, 아래와 같이 객체 참조 변수로도 접근이 가능하다.  
하지만 정적 멤버의 경우 클래스 이름으로 접근하는 것이 바람직하다.

```java
Calculator calculator = new Calculator();
double result = 10 * calculator.pi;
int plusResult = calculator.plus(1, 2);
```

### <U>정적 메소드 선언 시 주의사항</U>
정적 메소드가 객체 없이 실행되기 위해서는 선언 시 클래스 내부에 있는 인스턴스 멤버를 사용할 수 없다.  
또한 객체 자신의 참조인 this 키워드도 사용이 불가능하다.


아래와 같은 경우 컴파일 에러가 발생한다.
```java
public class 클래스 {
    static int 정적필드;
    int 인스턴스필드;

    static void 정적메소드() {
        this.정적필드 = 10; // 컴파일 에러
        인스턴스필드 = 10; // 컴파일 에러

        // 정적 메소드 내에서 인스턴스 멤버를 사용하는 방법
        // 객체를 먼저 생성해야 함
        클래스 obj = new 클래스();
        obj.인스턴스필드 = 10;
    }
}
```


## 싱글톤(Singleton)
spring을 공부하면서 싱글톤 패턴에 대해 처음 알게 됐는데, 이번에 java를 공부하면서 java 자체에서도 싱글톤이 가능하다는 것을 알게 됐다. 이래서 기초가 중요하다고 느낀다..!


> 싱글톤은 전체 프로그램에서 객체를 하나만 만들어야 하는 경우, 이 객체를 싱글톤(Singleton)이라고 한다.


객체를 하나만 만들기 위해서는 클래스 외부에서 new 연산자를 통해 생성자를 호출할 수 없도록 막아야 한다.  
이를 위해 생성자 앞에 private를 붙여줘서 접근을 제한한다.


그리고 정적 필드를 하나 생성하고 자신의 객체를 생성해 초기화한다.  
또한 외부에서 호출이 가능한 정적 메소드인 getInstance()를 선언하고 선언한 객체를 리턴해준다.

```java
public class SingletonClass {
    // 정적 필드
    private static SingletonClass singleton = new SingletonClass();

    // 생성자
    private SingletonClass() {}

    // 정적 메소드
    static SingletonClass getInstance() {
        return singleton;
    }
}
```

외부에서 객체를 얻는 유일한 방법은 SingletonClass 클래스의 getInstance 메소드를 호출하는 방법뿐이다.


## final 필드와 상수
> final 필드는 초기값이 저장되면 프로그램 실행 도중에 수정할 수 없다는 의미

final 필드에 초기값을 줄 수 있는 방법은 두 가지가 존재한다.
1. <B>필드 선언 시</B>
2. <B>생성자</B>: 생성자는 final 필드의 최종 초기화를 마쳐야 하는데, 그렇지 않으면 컴파일 에러 발생
```java
public class Calculaor {
    static final double PI = 3.141592;
    final int number;
    
    public Calculaor(int number) {
        this.number = number;
    }
}
```

단순 값이라면 필드 선언 시 값을 주는 것이 적절하지만,  
복잡한 초기화 코드가 필요하거나 객체 생성 시에 외부 데이터로 초기화해야 한다면 생성자에서 초기값을 지정해야 한다.


### <U>상수</U>
불변의 값을 보통 상수(final static)라고 부른다. 위에 선언했던 PI의 경우 불변하는 값이고 상수의 예이다.


> 자바에서는 이런 불변 값을 저장하는 필드를 자바에서는 상수(Constant)라고 부른다.

단순히 final만 붙이는 것은 객체마다 값을 변할 수 있다는 것을 의미하기 때문에, 상수를 선언할 때는 static final을 붙여야 한다.


또한 상수의 변수명은 대문자로 선언하는 것이 관례이다. 만약 서로 다른 단어가 혼합된 이름의 경우 언더바(_)로 연결해준다.
```java
static final double PI = 3.1415;
static final double EARTH_RADIUS = 6400;
```


## 참고 자료
위 내용은 신용권님이 쓰신 [혼자 공부하는 자바]를 정리한 내용입니다. 책에 정말 자세한 설명과 예시들이 나와있으니 궁금하신 분들은 구매 후 읽어보시길 추천드립니다!