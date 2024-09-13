---
layout: post
title:  "TIL(20240821) [Java version?]"
date:  2024-08-21
categories: TIL 
---

----------------------------------------------------------------------------

## 최종프로젝트를 마무리하며..

- 개선해야할 부분? 
1) 백엔드와 프론트 서버 분리하기 : 백엔드에 변경사항이 발생하면 프론트까지 다시 띄워지는 상황이 발생하여 서버 분리해줄 것
2) 스테이징서버도 테스트 환경만들기 :
DB같은 경우는 실제 배보환경의 DB를 사용하기 때문에 insert나 update가 쉽지 않을 것이다. 그래서 테스트 환경 DB를 따로 구축할 것

개선사항과 내가 공부해야 할 부분들이 많다. 이번주는 목에 담이 오고 피로가 너무 쌓여서인지 집중과 몰입이 힘들다.
그래도 마무리는 잘해야 하니 정신을 잡는 중! 😂

## 💡 Java Version?

Java 17버전은 2021년 9월 새로 공개한 LTS(Long-Term Support) 버전으로, Oracle JDK 기준 2029년 9월까지 지원한다. Java 11과 비교해서 70가지 이상의 JEP가 더 추가 되었는데, 그 부분을 살펴보도록 하겠다. 

```
JEP: (JDK 개선 제안, JDK Enhancement Proposals)
```

## 💡 Java 8

JetBrains에서 발표한 조사를 보면 Java언어를 이용하는 개발자분들께서 주로 사용하는 버전은 JDK 8이다.

JDK8버전을 사용하는 이유는?

 1. 발표된 LTS 버전 중 가장 오랜 Support을 보장
 - Oracle은 JDK 8이 현재까지 나온 버전 중 가장 오랜 기간 지원될 버전이라고 발표하였습니다.

2. 기존 서비스와의 호환
- 현재 국내에서 개발된 프로젝트는 대다수 Java 8로 개발되어 운영하고있는 상황입니다. 그렇기에 기존 프레임워크 또는 제품들과의 호환성을 유지하고 안정적으로 운영하기 위해 이후 연관된 프로젝트들 또한 JDK 8에서 벗어나지 않고 있습니다.


1. Java 8 (was released on March 18, 2014)
Oracle이 Sun Microsystems 인수 후 출시한 첫 번째 LTS 버전의 자바
32bit를 지원하는 마지막 공식 Java 버전
Oracle사에서 지원하는 유료 버전인 Oracle JDK와 오픈소스 기반의 무료 버전인 Open JDK로 나뉨
new Date and Time API(LocalDateTime 등)
Lambda, Stream API
PermGen Area 삭제
Static Link JNI Library
Unsigned Integer 계산
Annotation on Java Types
Interface Default Method
Optional class
Nashorn JavaScript engine 탑재
 

1-1. Lambda
```java
int max(int a, int b) {
    return a > b ? a : b;
}
```
//람다식으로 변환
(a, b) -> a > b ? a : b;
'람다식(Lambda Expression)'이란 함수를 간단한 식으로 표현하는 방법을 말하는데요.

메서드의 이름과 반환값(return)이 생략된다는 점에서 '익명 함수(anonymous function)'라고도 불립니다.

 
1-2. Stream API
```java
List<String> lowercase = Arrays.asList("a", "b", "c", "d", "e");
lowercase.stream()
        .map(String::toUpperCase)
        .forEach(System.out::println);
```
Steam은 컬렉션의 저장 요소를 하나씩 순회하면서 처리할 수 있는 코드 패턴입니다.

람다식을 지원한다는 점과 내부 반복자를 사용하기 때문에 병렬 처리가 쉽다는 특징이 있습니다.

 

 

1-3. interface default method
```java
public interface TestInterface {
    void doSomething();
    default void doSomethingDefault() {
        System.out.println("doing something default");
    }
}

//implements the interface
public class TestClass implements TestInterface {
    @Override
    public void doSomething() {
        System.out.println("doing something");
    }
}

TestClass testClass = new TestClass();
testClass.doSomething();          // Output: "doing something"
testClass.doSomethingDefault();   // Output: "doing something default"
```
java 8 이전의 인터페이스는 메서드 정의만 할 수 있었고 구현은 할 수 없었는데요.

8 버전부터 default method라는 개념이 생기면서 구현 내용도 인터페이스에 포함시킬 수 있게 되었습니다.


1-4. Optional class
```java
//create an optional that contains a value
Optional<String> optional = Optional.of("Hello, world!");
//check if a value is present
if (optional.isPresent()) {
	String value = optional.get();
	System.out.println(value);    // Output: "Hello, world!"
}
		
//create an empty optional
Optional<String> emptyOptional = Optional.empty();
//get a default value if the optional is empty
String value = emptyOptional.orElse("default value");
System.out.println(value);    // Output: "default value"
		
//throw exception
emptyOptional.orElseThrow(() -> new RuntimeException("throw Exception"));
```
Optional<T>는 null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 NPE(Null Pointer Exception)가 발생하지 않도록 도와주는 역할을 하는데요.

따라서 예상치 못한 Null Pointer Exception이 발생될만한 상황에서도 예시와 같이 제공되는 메서드를 통해 간단하게 예외 처리를 할 수 있습니다.

 ## 💡 Java 17

3. Java 17 (was released on September 14, 2021)
봉인 클래스(Seald Class) 정식 추가
패턴 매칭 프리뷰 단계
Incubator (Foreign Function & Memory API)
애플 M1 및 이후 프로세서 탑재 제품군에 대한 정식 지원
의사난수 생성기를 통해 예측하기 어려운 난수를 생성하는 API 추가
컨텐츠 기반의 역직렬화 필터링
Record Data Class 추가
 

3-1. Seald Class
```java
public sealed class Shape permits Circle, Square {
    // common fields and methods
}

public final class Circle extends Shape {
    // circle-specific fields and methods
}

public final class Square extends Shape {
    // square-specific fields and methods
}
```
17 버전에서 추가된 Seald Class, Interface는 상속하거나(extends), 구현(implements) 할 클래스를 지정해 두고, 해당 클래스들만 상속 또는 구현을 허용하는 키워드입니다.

개발자는 seald 키워드를 통해 어떤 클래스가 해당 클래스를 상속 또는 구현하는지를 쉽게 알 수 있고, 또 제한할 수 있습니다.

 
3-2. Record Data Class

```java
//Lombok 사용 예시
@EqualsAndHashCode
@ToString
@AllArgsConstructor
@Getter
public class Person {
    private final String name;
    private final String address;
}

//record class 예시
public record Person (String name, String address) { }
```
Record 키워드는 14 버전에서 프리뷰 기능으로 추가되었고, 16 버전에서 공식 기능이 되었는데요.

 

Record 클래스는 불변 데이터를 객체 간에 전달하는 작업을 간단하게 만들어주며, record를 사용함으로써 불필요한 코드(boilerplate code)를 제거할 수 있고, 적은 코드로도 명확한 의도(data carrier)를 표현할 수 있다는 특징이 있습니다.

 

 
3-3. 텍스트 블록(Text Blocks)

```java
String html1 = "<html>\n" +
        "           <body>\n" +
        "               <p>Hello, world</p>\n" +
        "           </body>\n" +
        "       </html>\n";

// Text Blocks
String html2 = """
        <html>
            <body>
                <p>Hello, world</p>
            </body>
        </html>
        """;
```
텍스트 블록은 java 13, 14 버전에서 프리뷰로 추가되었고 15 버전에서 정식으로 발표되었는데요.

멀티 라인의 문자열을 에스케이프 시퀀스 없이 사용감으로 소스 코드 작성을 편리하게 하고, 코드의 가독성을 높이는데 주된 목적을 가지고 있다고 합니다.

 [Java 8, 11, 17 버전별 추가된 기능](https://wildeveloperetrain.tistory.com/287)

