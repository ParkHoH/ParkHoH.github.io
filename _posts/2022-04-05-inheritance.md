---
title: "[Java] 타입 변환과 다형성"
excerpt: ""

categories: Java
tags: [Java, 타입 변환, 다형성]

date: 2022-04-05
last_modified_at: 2022-04-05
---

> [혼자 공부하는 자바] 책을 읽다가 상속과 다형성이라는 내용이 나왔다. 알고 있는 내용이라고 생각했는데 막상 읽어보니 새롭게 알게 된 부분도 많이 있어 이번 기회에 정리해보려고 한다.


## 다형성
> 사용 방법은 동일하지만 다양한 객체를 이용해서 다양한 실행결과가 나오도록 하는 성질

예를 들어 자동차는 타이어의 타입으로 금호 타이어와 한국 타이어를 사용할 수 있지만 각각의 성능과 특성은 다르다.

다형성을 구현하려면 <B>타입 변환</B>과 <B>메소드 재정의</B>가 필요하다.


### <U>자동 타입 변환</U>
먼저 자동 타입 변환에 대해 알아보자.

> 자동 타입 변환: 프로그램 실행 도중에 자동적으로 타입 변환이 이루어지는 것

```java
// 자동 타입 변환 방법
부모타입 변수 = 자식타입;

// 예시
Cat cat = new Cat();
Animal animal = cat; // new Cat()으로도 가능
```

위 예시는 Cat 클래스가 Animal 클래스의 자식 클래스임을 가정한 것이다.  
자식 클래스는 부모 클래스와 동일한 기능과 특징을 상속받기 때문에 부모와 동일하게 취급될 수 있다.


위 예시가 실행됐을 때 메모리의 상태는 아래 사진과 같다. 즉, 두 변수는 동일한 객체를 참조하고 있다.
![inheritance](/assets/images/posts/2022-04-05/inheritance.png){: .align-center}
```java
cat == animal // true
```
---
바로 위 부모가 아니더라도 상속 관계에서 상위 타입이라면 자동 변환이 이루어진다.  
하지만 반대의 경우는 가능하지 않다.


또한 부모 타입으로 자동 타입 변환이 이루어진 이후에는 부모 클래스의 필드와 메소드만 접근이 가능하다.  
즉 실제로는 대부분의 경우 자식 클래스에서 필드와 메소드가 추가로 정의됐을텐데, 상위 타입으로 타입 변환이 이루어지면 부모 클래스의 필드와 메소드만 사용 가능하다는 점이 특징이다.


여기서 예외가 있는데 메소드가 자식 클래스에서 재정의(오버라이딩)되었다면 자식 클래스의 메소드가 대신 호출된다.  
이 부분은 이 글의 제목 중 하나인 '다형성'과 관련있는 굉장히 중요한 특징이다.

아래 예시를 보자.

```java
class Parent {
    void method1() {}
    void method2() {}
}

class Child extends Parent {
    @Override // annotation은 안 써줘도 되지만, 빠른 구분을 위해 써주는 것이 좋다
    void method2() {} // 오버라이딩(메소드 재정의)
    void method3() {} // 새롭게 정의한 메소드
}

public class Example {
    public static void main(String[] args) {
        Child child = new child();
        Parent parent = child;
        praent.method1();
        praent.method2(); // Child 클래스의 method2 호출
        praent.method3(); // 호출 불가능
    }
}
```

### <U>다형성의 필요성</U>
처음 배웠을 때 왜 다형성이 왜 필요할까에 대한 궁금증이 들었다.  
그냥 자식 클래스에서 사용하면 될 것을 굳이 왜 부모 클래스로 타입 변환을 하는 것일까?

> 그 이유는 부모 타입으로 선언하면 다양한 자식 객체들이 변수에 저장될 수 있기 때문에, 필드 사용 결과가 달라질 수 있기 때문이다.

예를 들면 자동차를 구성하는 타이어의 경우 고장나면 다른 타이어로 대체하거나 더 좋은 성능의 타이어로 대체가 가능하다.  
프로그램에서 수많은 객체들이 연결되어 있는데 이런 객체들을 자유롭게 교체할 수 있어야 하기 때문에 다형성이 존재한다고 볼 수 있다.


### <U>매개 변수의 자동 타입 변환</U>
참고로 매개 변수의 타입이 클래스일 경우 자식 타입도 매개 변수로 사용이 가능하다.  
자식 타입을 매개값으로 사용할 경우 자동 타입 변환이 된다.



## 강제 타입 변환
> 부모 타입을 자식 타입으로 변환하는 것

위에서 살펴봤듯이 자식 타입이 부모 타입으로 자동 타입 변환을 하게 되면 자식 클래스에서 선언된 필드와 메소드는 사용하지 못한다.  
만약 자식 클래스에 선언된 필드나 메소드를 사용하고 싶은 경우, 부모 타입에서 자식 타입으로 강제 타입 변환을 하면 된다.


사용 조건은 아래와 같다.
1. 먼저 자식 타입에서 부모 타입으로 자동 타입 변환을 한다.
2. 이후 다시 부모 타입에서 자식 타입으로 강제 타입 변환을 한다.
```java
Parent parent = new Child(); // 자동 타입 변환
Child child = (Child) parent; // 강제 타입 변환
```

위에서 1번 조건이 지켜지지 않으면 강제 타입 변환은 불가능하다.  
따라서 부모-자식 관계를 제대로 파악하는 것이 필요하고, 자바에서는 이런 부모-자식 관계를 파악할 수 있도록 instanceof 연산자를 제공한다.
```java
boolean result = parent instanceof Child; // true
```


## 참고 자료
위 내용은 신용권님이 쓰신 [혼자 공부하는 자바]를 정리한 내용입니다. 책에 정말 자세한 설명과 예시들이 나와있으니 궁금하신 분들은 구매 후 읽어보시길 추천드립니다!