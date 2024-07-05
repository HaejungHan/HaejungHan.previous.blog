---
layout: post
title:  "TIL(20240603) [Spring:Jpa]"
date:  2024-06-03
categories: TIL Spring 코딩테스트
---

---------------------------------------------------------------------


# 📌 Spring 

## 💡 Spring 입문2주차 복습 키워드 정리

어제 공부했던 Jpa부터 다시 정리하고 넘어가도록 하자.

## 💡 Jpa가 왜 필요한가? 
- Jdbc를 사용하게 되면 객체에 필드를 하나 추가한다고 가정했을 때
SQL문의 DDL이나 DML의 수정작업이 필요하게 되는데 즉, 코드를 변경해야될 부분이 너무나도 많아진다는 사실이다. 그렇기 때문에 유지보수측면에서 좋지 않고 확장성도 좋지 않다. 
- 이를 보완하기 위해 나온 것이 ORM이라는 기술인데, ORM은 object-relational-mapping 객체와 데이터베이스를 자동연결 시켜주는 기술이다. java를 위한 ORM의 대표적인 기술이 **Jpa**(실제 구현체는 Hibernate)이다. 
- **장점 :** Jpa를 사용하게 되면 데이터베이스를 직접 연결하지 않아도 자동으로 처리해주며, 자바의 클래스를 통해서 간접적으로 데이터베이스에 접근하기에 쉽게 데이터베이스 (CRUD)작업을 처리할 수 있다!
- TMI) Jpa를 사용하면 코드 4줄이 -> 1줄로 바뀌는 매직매직✨

## 💡 영속성 컨텍스트
- 영속성 컨텍스트가 무엇인가? Entity 객체를 효율적으로 관리하기 위한 만들어진 공간
- 영속성 컨텍스트 환경에서는 꼭 트랜잭션이 적용되어야 한다. 트랜잭션이 적용되지 않을 경우 persist call(영속성 컨텍스트에서 해당 객체를 넣어주는 개념)이 불가능하다. 

## 💡 Jpa Auditing 
- 일시 자동생성 같은 개념
- Timestamped 추상클래스로 설정하는 이유? 객체로 사용하지 않을 것이기 때문에 구지 일반클래스로 설정할 필요가 없다.
- TImestamped 를 사용하게 된다면 main에서 @EnableJpaAuditing 애너테이션을 달아서 spring에게 알려야 한다.
- @Temporal(TemporalType.TIMESTAMP) 3가지 DATE,TIME,TIMESTAMP가 있다. 상황에 맞게 변경해서 사용할 수 있다.
- reponse에도 해당 필드와 생성자 만들어주기
- createAt에 updatable = false 인 이유? 최초 생성했던 일시를 이후에 변경할 필요가 없고 modifiedAt에서 이후 변경사항에 대한 일시 알려주면 되기 때문

## 💡 Query Method 
- 형식에 맞게 SQL 쿼리메서드를 선언하기만 하면 따로 구현하지 않아도 해당 메서드를 분석해서 (simple)JpaRepository가 대신 구현을 해주게 된다.
- 예시) findAllByOrderByModifiedAtDesc() : modifiedAt 필드를 기준으로 정렬해서 해당 모든 데이터를 내보낼 건데 그 기준은 내림차순입니다.
- 예시 2) findAllByContentsContainsOrderByModifiedAtDesc() : 해당 키워드 입력시 조회기능!

<br>

---------------------------------------------------------------------

# 📌 코딩테스트1️⃣ : 문자열 내림차순으로 배치하기

### 🔒 문제 : 문자열 s에 나타나는 문자를 큰것부터 작은 순으로 정렬해 새로운 문자열을 리턴하는 함수, solution을 완성해주세요. s는 영문 대소문자로만 구성되어 있으며, 대문자는 소문자보다 작은 것으로 간주합니다.

### 🚫 조건 : str은 길이 1 이상인 문자열입니다.


##
 🔓 문제풀이

```java
import java.util.Arrays;
import java.util.Collections;

class Solution {
    public String solution(String s) {
        String answer = "";
        String[] sArr = s.split(""); // s 문자열 자르기
        Arrays.sort(sArr,Collections.reverseOrder()); // 오름차순 -> 내림차순

        answer = new String();

        for (int i=0; i<sArr.length; i++){
            answer += sArr[i]; // 배열을 문자열로 
        }
        
        return answer;
    }
}
```
