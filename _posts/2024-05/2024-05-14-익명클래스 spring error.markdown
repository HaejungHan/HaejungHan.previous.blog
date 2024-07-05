---
layout: post
title:  "TIL(20240514) JAVA: 익명클래스"
date:  2024-05-14
categories: TIL JAVA Spring
---

---------------------------------------------------------------------

## 🙌 오늘의 공부목록
- ~~JAVA 자바의 정석 chapter 7 강의 듣기~~
- ~~JAVA 강의 듣고 정리(chapter6,7)!!!!!~~
- ~~코딩테스트 1문제 (30분 소요)!!!!!~~
- ~~Spring 강의 1주차 끝내기!!!!!~~
- Spring 강의 1주차 강의 정리!!!

## 🔄 내일의 공부목록
- Spring : 1주차 강의 정리
- JAVA : 자바의 정석 chapter 7 마무리 chapter 8 강의 듣기
- 코딩테스트 : 프로그래머스 2문제
- Spring : 강의 2주차 -8까지 듣기

### ✔ 오늘 하루 느낀점
: Spring 수업에 따라가느라.. 자바 공부하느라... 계속해서
정리는 못하고 강의만 듣고 있는데.. 정말 어떻게 효율적으로 공부해야할지
방향을 못잡고 있는 것인가.. 이거하다가 저거하다가.. 정신이 없다.
내일은 시간을 효율적으로 쓸 수 있도록 정확한 시간을 정해야겠다.

- 코딩테스트 : 오전 10시-12시
- JAVA-chapter 7(익명클래스),8 : 오후 13시-16시 정리 꼭 할 것!!!!!
- Spring 2주차 : 오후 16시~18시 , 1시간 저녁시간 , 19시~ 22시까지 

혹시나 빨리 끝냈을 경우에는 강의 반복해서 듣기.
꼭 코드 따라서 치기 (자동완성기능 개무시하기!!!!) 😱😱😱

---

# 🚩 JAVA    

## 💡익명클래스
- 프로그램에서 일시적으로 한번만 사용되고 버려지는 객체
- 익명 클래스는 클래스 정의와 동시에 객체를 생성할 수 있기에 단 한번만 사용됨(일회용)
- 익명클래스는 어떤 상황에서 사용되는가 ? 익명 클래스는 부모 클래스의 자원을 일회성으로 재정하여 사용하기 위한 용도
- 오버라이딩 한 메소드 사용만 가능, 새로 정의한 메소드는 외부에서 사용 불가능
- 익명클래스도 내부 클래스의 일종이기 때문에, 외부의 지역변수를 이용하려고 하면 내부의 클래스의 제약을 받게 됨.

### 👀 이해하기 위한 예제(코드 따라서 작성하기)

```java
class Animal {
    public String bark() {
        return "동물이 짖습니다.";
    }
}

public class Main{
    public static void main(String[] args) {
        Animal dog = new Animal() {
            // @Override 메소드
            public String bark() {
                return "개가 짖습니다";
            }

            public String run() {
                return "개가 달리기를 합니다.";
            }
        }; //<- 꼭 세미콜론 붙여야함

        dog.bark(); // 개가 짖습니다.
        dog.run(); // error - 외부에서 호출 불가능!
    } 
}
```


---------------------------------------------------------------------

# 🚩 Spring
- 메소드 키워드 정리 : 

```java
.stream()
.values()
.map()
.toList()
.keySet()
```

```java
🔒 error 1: Error starting ApplicationContext. To display the condition evaluation report re-run your application with 'debug' enabled.
🔓 error 1 해결방법 : Run -> edit configurations-> modify options -> enable debug output (오류 내역을 상세히 알려줌..error2로 인해서 발생된 오류 같음..)

🔒 error 2: Exception encountered during context initialization - cancelling refresh attempt: org.springframework.context.ApplicationContextException: Failed to start bean 'webServerStartStop' (이미 사용중 포트이기 때문에 포트 번호 바꿔야함)
🔓 error 2 해결방법: resources -> application.properties -> server.port=8090(포트번호 변경) 
```

---------------------------------------------------------------------



