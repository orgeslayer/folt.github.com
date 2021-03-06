---
layout: post
title: 람다식 - (3)
author: orgeslayer
update:  2017-07-009T11:15:00Z
published: true
complete: true
tags: java1.8, lambda, FunctionalInterface, 함수적프로그래밍
excerpt: 메소드 참조(Method Reference)는 **메소드를 참조해서 매개 변수의 정보 및 리턴 타입을 알아내어 람다식에서 불필요한 매개 변수를 제거**하는 것이 목적이다.
---
# 람다식 (Lambda Expression) 

#### 목차
1. [람다식 기본](/2017/07/09/java-lambda-1/)
2. [Java 표준API](/2017/07/09/java-lambda-2/)
3. [메소드참조](/2017/07/09/java-lambda-3/)

------------------------

### 메소드 참조
메소드 참조(Method Reference)는 **메소드를 참조해서 매개 변수의 정보 및 리턴 타입을 알아내어 람다식에서 불필요한 매개 변수를 제거**하는 것이 목적이다.  람다식은 종종 기존 메소드를 단순히 호출해주는 경우가 많다. 예를 들어 두 개의 값을 받아 큰 수를 리턴하는 람다식의 경우 아래와 같을 수 있다.
```java
IntBinaryOperator max = (left, right) -> Math.max(left, right);
```
람다식은 단순히 두 개의 인자값을 Math.max() 메소드의 매개값으로 전달하는 역할만 하기 때문에 불필요한 입력이 있으며, 이 경우 다음과 같이 메소드참조를 이용하면 좀 더 코드를 간결하게 바꿀 수 있다.
```java
IntBinaryOperator max = Math::max;
```
메소드 참조도 람다식과 마찬가지로 인터페이스의 익명 구현 객체로 생성되므로, 타겟 타입인 인터페이스 추상 메소드가 어떤 매개 변수를 가지고, 리턴 타입이 무엇인가에 따라 달라진다. 메소드 참조는 ***정적메소드 참조, 인스턴스 메소드를 참조, 생성자 참조가 가능***하다.
```java
클래스::메소드;
참조객체::메소드;
```

메소드는 람다식 외부의 클래스 멤버일 수도 있고, 람다식에서 제공하는 매개 변수의 멤버일 수도 있다. 예를들어, 아래와 같이 a 매개변수의 메소드를 호출해서 b 매개변수를 값으로 사용하는 경우도 있다.
```java
(a, b) -> { a.instanceMethod(b); }
```
이것을 메소드 참조로 표현하면 **a의 클래스 이름 뒤에 :: 기호를 붙이고 메소드 이름을 기술하면 된다.** 정적 메소드 참조와 동일하게 작성하지만, 실제 동작은 a의 인스턴스 메소드가 참조되기 때문에 전혀 다른 코드가 실행된다.
```java
ClassA::instanceMethod;
```

위에서 언급한것 처럼 메소드 참조는 생성자 역시 참조가 가능하다. 단순히 객체를 생성하고 리턴하도록 구성된 람다식은 생성자 참조로 대치가 가능하다. 아래 람다식은 단순히 객체 생성 후 리턴만 한다. 
```java
public class A {
	int a;
	int b;
	
	public A(int a, int b) {
		this.a = a;
		this.b = b;
	}
}

BiFunction<Integer, Integer, A> generator = (a, b) -> new A(a, b);
```

이 경우, 생성자 참조를 사용하면 아래와 같이 수정이 가능하다.
```java
BiFunction<Integer, Integer, A> generator = A::new;
```
생성자 참조는 클래스 이름 뒤에 :: 기호를 붙이고 new 연산자를 기술하면된다. 생성자가 여러개 오버로딩 되어있을 경우, **컴파일러는 함수적 인터페이스의 추상 메소드와 동일한 매개 변수 타입과 개수를 가지고 있는 생성자를 찾아서 실행**하며, 만약 해당 생성자가 없을 경우 컴파일 오류가 발생된다.

-------------------------------
참조
[palpit's log-b](http://palpit.tistory.com/670)
