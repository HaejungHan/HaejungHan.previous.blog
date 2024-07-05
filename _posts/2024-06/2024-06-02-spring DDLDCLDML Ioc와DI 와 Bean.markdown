---
layout: post
title:  "TIL(20240602) [Spring:DDL,DCL,DML/IoC와 DI/ Bean]"
date:  2024-06-02
categories: TIL Spring
---

---------------------------------------------------------------------


# 📌 Spring 

오늘은 팀 프로젝트에 들어가기 앞서서 기본 1주차를 복습하기 위해
핵심 키워드를 정리한다는 마음으로 공부를 하려고 한다. 

## 💡 Spring 입문1주차 복습 키워드 정리

- 정적페이지(static)와 동적페이지(templates)

- thymeleaf : 템플릿 엔진, 동적인 웹페이지를 만드는 html파일을 만드는 라이브러리(templates폴더 내부에서 찾음)

- StatusCode[3XX]: redirect관련 요청 

- @Controller : 애너테이션 Controller와 RestController의 차이는
Controller의 경우 html을 반환, RestController는 Json형태로 데이터를 반환하기 위해 사용한다.  

### 💡 SQL 

- SQL은 'Structured Query Language'의 약자로 RDBMS(관계형 데이터베이스)에서 사용되는 언어

- DDL(Data Definition Language) : 테이블이나 관계의 구조를 생성하는데 사용함 
 1) CREATE : 새로운 데이터베이스 및 테이블을 생성해줌
 2) ALTER : 데이터베이스와 테이블의 내용 수정을 할 수 있음
 3) DROP : 데이터베이스와 테이블을 삭제할 수 있음
 4) TRUNCATE : 데이터베이스와 테이블을 삭제할 수 있고, 최초 테이블이 만들어졌던 상태 (컬럼값만 남김)로 돌아감

- DCL(Data Control Language) : 데이터의 사용권한을 관리하는 데 사용
1) GRANT : 사용자 또는 ROLE에 대한 권한 부여
2) REVOKE : 사용자 또는 ROLE에 부여한 권한을 회수

- DML(Dta Manipulation Language) : 테이블에 데이터를 검색,삽입,수정,삭제하는데 사용
1) INSERT : 테이블에 새로운 row를 추가
2) SELECT : 테이블의 row를 선택
3) UPDATE : 테이블의 row의 내용을 수정
4) DELETE : 테이블의 row를 삭제


## 💡 Spring 입문2주차 복습 키워드 정리

- 3 Layer Architecture 
1) Controller : client와 소통하는 창구 즉, 클라이언트의 요청을 받고 요청에 따른 로직처리를 service에게 넘기고 받은(결과) 해당 요청에 대한 응답을 client에게 반환하는 역할을 한다. 
2) Service : 비지니스 로직, client의 요구사항을 처리하는 역할
3) Repository : DB관리, DB CRUD작업을 처리함

### 💡 IoC(제어의 역전)와 DI(의존성 주입)

제어의 역전과 의존성 주입을 설명해보자 

```java

public class Cooker{
    void cook () {
        Chicken chicken = new Chicken();
        chicken.cook(); 
    }

    public static void main(String[] args){
        Cooker cooker = new cooker();
        cooker.cook();
    }
}

class Chicken {
    public void cook() {
        System.out.println("치킨을 요리한다.");
    }
}

```

만약에 요리사가 피자를 요리하고 싶다면? Cooker클래스에서 cook메서드의 chicken객체를 pizza객체로 코드 수정해야한다. 또, 마라탕을 먹고싶다고 하면? 이하 같다. 이러한 상황을 **강한 결합**이라고 한다.

이러한 강한결합의 단점은 **코드의 유지보수 측면**에서 좋지 않다. 보다시피 1) 코드의 변경에 영향을 많이 받고 있으며 2) 객체의 중복생성이 발생할 수 있다. 

interface Food를 이용해 약한 결합으로 바꾸어 보자.

```java
public class Cooker{

    void cook(Food food){
        food.cook();
    }

    public static void main(String[] args){
        Cooker cooker = new cooker();
        cooker.cook(new Chicken())
        cooker.cook(new Pizza());
    }
}

interface Food() {
    void cook();
}

class Chicken implements Food{
    @Override
    public void cook() {
        System.out.println("치킨을 요리한다.");
    }
}

class Pizza implements Food{
    @Override
    public void cook() {
        System.out.println("피자를 요리한다.");
    }
}
```

이처럼 Cooker클래스의 메서드에 매개변수를 Food타입으로 받아서
치킨이나 피자를 요리할 수 있다. 그런데 여기서 살펴볼 점은 요리를 하는 제어권이 Cooker가 아닌 Food로 바뀐것을 볼 수 있다. 

그렇다면 DI를 통해서 약한 결합을 하게 된다면 어떻게 될까? DI(의존성 주입)디자인 패턴의 경우에는 3가지  필드주입,setter주입,생성자 주입이 있다. 여기서 나는 **생성자 주입**으로 약한 결합의 경우를 살펴보도록 하겠다.
(객체의 불변성을 위해 생성자 주입을 많이 사용한다.)

```java
public class Cooker {
    Food food; 

    public Cooker (Food food){
        this.food = food;
    }

    void cook() {
        this.food.eat();
    }
    
    public static void main(String[] args){
        Cooker cooker = new cooker(new Chicken());
        cooker.cook(); // 치킨을 요리한다.
        cooker.cook(); // 피자를 요리한다.
    }
}

interface Food(){ // Food interface 생성
    void cook(); 
}

class Chicken implements Food{
    @Override
    public void cook(){
        System.out.println("치킨을 요리한다.");
    }
}

class Pizza implements Food{
    @Override
    public void cook(){
        System.out.println("피자를 요리한다.");
    }
}
```

이처럼 생성자 주입을 통해서 제어의 흐름이 Food타입으로 음식을 만들다가 다시 Cooker를 통해 음식을 만드는 것을 확인할 수 있다.

이것이 IoC 의 제어의 역전이다. Cooker -> Food , Food -> Cooker로 다시 제어의 흐름이 바뀌는 것! DI(의존성 주입)를 통해서 제어의 역전을 이루어낸 것이다. 

### 💡 IoC컨테이너 와 Bean
1) Bean : Spring이 관리하는 객체
2) Spring Ioc 컨테이너 : 'Bean'을 모아둔 컨테이너

### 💡 @Component : 일반클래스를 애너테이션 @Component로 설정해주게 되면 Spring이 실행될 때 알아서 찾아 Bean으로 자동 등록해준다. 

그렇다면 여기서 생각해볼 것이 있다. @Controller(Rest포함), @Service, @Repository에 @Component 애너테이션을 같이 붙이지 않는다. 그 이유는 ? Controller,Service,Repository 안에 @Component 애너테이션이 있기 때문에(포함되어 있기 때문)! 따로 애너테이션을 붙여주지 않아도 자동으로 Component 설정이 되는 것이다. 

### 💡 ORM(Object-Relational Mapping) 
- 객체와 데이터베이스를 매핑해주는 것
- ORM이 등장한 이유는 ? 예를 들어 객체에 필드 하나를 추가해야 한다고 가정했을 때, SQL 수정에 더 많은 시간을 투자해야한다. 이러한 작업을 줄여주기 위해서 등장한다.

### 💡 Jpa : java ORM기술의  대표적 명세
- 웹 어플리케이션 서버와 JDBC 사이에서 동작이 되고 있는데, Jpa를 사용하게 되면 데이터베이스에 직접 연결하지 않아도 자동으로 매핑이 된다. 
- Jpa는 표준 명세, 이를 실제로 구현한 프레임워크 중 사실상 표준이 하이버네이트(Hibernate) 이다. 


---------------------------------------------------------------------
