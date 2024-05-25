---
layout: post
title:  "TIL(20240525) Spring:JWT/HttpStatus:406;"
date:  2024-05-25
categories: TIL JAVA 코딩테스트
---

---------------------------------------------------------------------
# 💡 Spring 댓글 기능 구현 1) 댓글 등록

## 🔒 HttpStatus: 406
:  HTTP (HyperText Transfer Protocol) 406 Not Acceptable 클라이언트 오류 응답 코드는 서버가 요청의 사전 콘텐츠 협상 헤더에 정의 된 허용 가능한 값 목록과 일치하는 응답을 생성 할 수 없으며 서버가 기본 표현을 제공하지 않음을 나타냅니다.

```
//postman
{
    "timestamp": "2024-05-25T06:23:04.863+00:00",
    "status": 406,
    "error": "Not Acceptable",
    "path": "/api/schedule/1/comment"
}
```

Accept 등의 헤더에 적혀있는 형식을 생성해 낼 수 없을 경우 발생한다고 하여 3가지 경우로 확인되었다.

1. Jackson라이브러리가 없는 경우, -> 프로젝트의 dependencies -> 기본적으로 spring-boot-starter-web 안에 Jackson라이브러리가 포함되어있음
2. 구글링을 한 결과.. SpringBoot에서 위 에러가 나는 경우는 대부분으 Getter메서드가 없기때문....🤣

## 🔓 해결방법
: CommentResponseDto에 Getter메서드 추가하니 반환이 잘 되는 것을 확인했다..😁

```
{
    "scheduleId": 1,
    "id": 13,
    "contents": "댓글",
    "createdAt": "2024-05-25T15:31:34.3566043"
}
```

## 💡 JWT

- JWT가 무엇인가? JWT는 JSON포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim기반의 Web Token이다. 토큰의 한 종류이며 일반적으로 쿠키저장소에서 JWT를 저장한다. 

- JWT의 장점은? 동시 접속자가 많을 때 서버측 부하를 낮춤 (서버 자체에서 Secret key를 검증하기 때문에)
- JWT의 단점은? 구현의 복잡도 증가, JWT에 담는 내용이 커질수록 네트워크 비용이 증가하게 됨, 기 생성된 JWT를 일부만 만료시킬 방법이 없으며 secret key가 유출될 경우 JWT가 오픈되어 있어서 조작이 가능하기에 보안에 취약함

- JwtUtil ? JWt의 관련된 기능을 가진 클래스 같은 개념

## 💡 패스워드 암호화

1) **양방향 암호 알고리즘** 
- 암호화: 평문 -> 암호화 알고리즘 -> 암호문
- 복호화: 암호문 -> 암호화 알고리즘 -> 평문

2) **단방향 암호 알고리즘**
- 암호화 : 평문 -> 암호화 알고리즘 -> 암호화
- 복호화 : ❌불가!

그렇다면, 사용자가 로그인할 때 암호화된 패스워드를 기억해야 하는가? 
: Password Mathing! boolean타입의 .matches()라는 메서드로 사용자가 입력한 비밀번호 == 암호화된 비밀번호를 구별해준다!

```
boolean matches(CharSequence rawPassword, String encodedPassword);
```
rawPassword : 사용자가 입력한 비밀번호
encodedPassword : 암호화되어 DB에 저장된 비밀번호


---------------------------------------------------------------------

# ⭐ 네트워크 > 기술면접 대비

1) RESTful한 API를 설계하는 장점은?
- Rest : 웹(HTTP)의 장점을 활용한 아키텍쳐
- Rest의 등장배경 : 인터넷과 같이 복잡한 네트워크 통신이 등장함에 따라, 이를 관리하기 위한 지침으로 만들어졌다. 
- Rest의 요소 : method(POST,GET,PUT,DELETE), Resource : URI(자원-명사로 표현), 메시지 포맷이 존재(JSON,XML)
- Rest의 특징 : HTTP 표준에 따라 설계한다면 어떤 기술도 가능한 interface스타일 즉, 특정언어나 기술에 종속 받지 않고 모든 플랫폼에서 사용 가능함/ API메시지만 보고 API를 이해할 수 있는 구조/ HTTP Session과 같은 컨텍스트 저장에 상태 정보를 저장하지 않는다. 
- REST API : REST기반으로 서비스 API를 구현한다라는 의미에서 클라이언트와 서버간의 시스템이 인터넷을 통해 정보를 안전하게 교환하기 위해 사용하는 인터페이스
- REST API의 장점 : 클라이언트와 서버의 역할을 명확히 분리하여 다양한 플랫폼에서 원하는 서비스를 쉽고 빠르게 개발하여 배포할 수 있다.
- RESTful의 목적 : 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것, 즉 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이다.

: restful은 뭔가, restful 의 방법은?, restful의 장점은 이것이다. .... 

2) 관심사 분리
- 장점: 코드의 유지보수성, 재사용성, 테스트 가능성을 향상시킬 수 있다.
- 하나의 관심사가 하나의 기능만 수행하도록 코드를 구성하는 것을 말한다. 한 객체(클래스)안에서 다양한 관심사를 수행하게되면 코드가
복잡해져서 코드에 대한 이해도가 떨어질 수 있다. 그렇기에 유지보수적인 측면에서 악영향을 줄 수 있다. 

: 예를 들어서 설명하여 유지보수,확장성이 용이하다. + 가독성, 재사용성 2개이상의 논리를 추가한다. 

3) 무분별하게 setter를 사용하지 않아야 한다고 하는데 그 이유는? 
피드백 : 기술적인 역량, 기술의 견해, 문제를 어떻게 논리적으로 해결할 수 있는가? 증명해야한다. 
논리적으로 설명해야한다.  

모든 setter데이터를 무분별하게 사용하게 된다. .. 데이터 무결성 손상, 분별성 위반, 보완적 측면에서도 좋지않다. 

3-1) setter사용하지 않고 하는방법?
- 생성자 , 특별한 메서드 사용 (데이터 주입)

4) 데이터베이스 어떤것인가
- 관계형데이터베이스, 비관계형데이터베이스, ACID 

이해-> 정리-> 답변

5) 객체지향 프로그램은 어떤것인가?  (주관적인 관점에서 얘기해보자)
- 객체, 캡슐화, 다형성.... 어떤것이 장점? 
- 절차적인 프로그램 어떻게 보완한 페러다임 ? 

서로 대화하는 형식.. 티키타카의 형식으로!  

# 📌 Spring

# 📌 DB

- 데이터조회

```
select 
    p.id, // product 테이블에서 id
    p.title as product_title, // product 테이블에서 title 
    pf.product_id as product_id, // Product_Folder테이블의 Product_id
    pf.folder_id as folder_id // Product_Folder테이블의 folder_id 들을 보여주면 되
from product p // product 테이블에서
         left join product_folder pf on p.id = pf.product_id // product_folder에서의 product_id와 product테이블의 id와 같으니까 합쳐서
where p.user_id = 1 // user_id 1번이 가진 
and pf.folder_id = 4 // 4번 폴더 에서
order by p.id desc /// Product_id가 내림차순이 되어서
limit 20; // 20개씩만 보이도록 

```

---------------------------------------------------------------------

# 📌 코딩테스트1️⃣ : 없는 숫자 더하기

## 🔒 문제 : 0부터 9까지의 숫자 중 일부가 들어있는 정수 배열 numbers가 매개변수로 주어집니다. numbers에서 찾을 수 없는 0부터 9까지의 숫자를 모두 찾아 더한 수를 return 하도록 solution 함수를 완성해주세요.

## 🚫 조건 : 
- 1 ≤ numbers의 길이 ≤ 9
- 0 ≤ numbers의 모든 원소 ≤ 9
- numbers의 모든 원소는 서로 다릅니다.


# 💡 필요했던 메서드


# 🔓 문제풀이

```java
// 1차시도
class Solution {
    public int solution(int[] numbers) {
        int answer = 0;
        for(int i=0; i<10; i++){
          for(int j=0; j<numbers.length; i++){
              if(!(i == numbers[j])) {
                  answer+= i;
              }
          }

        }
        return answer;
    }
}

실행 시간이 10.0초를 초과하여 실행이 중단되었습니다. 실행 시간이 더 짧은 다른 방법을 찾아보세요. => 🤣

class Solution {
    public int solution(int[] numbers) {
        int answer = 0;
        
        for(int i=0; i<10; i++){
            answer += i;
        }
        
        for(int i=0; i<numbers.length; i++){
            answer -= numbers[i];
        }
        return answer;
    }
}


```

## 🤷‍♀️ 코딩테스트1️⃣ 문제풀이를 하면서 느낀점
: 수학적으로 풀어야하는 문제여서, 제한사항을 살펴보니 numbers에서 0~9까지 없는 숫자만 빼서 더하는거나
0~9 숫자의 합에서 numbers에만 있는 숫자의 합을 뺀 것과 같다라는 생각이 들었다.
그래서 answer에 for문을 돌려서 0~9 숫자의 합을 넣고 numbers배열의 있는 값을 for문을 돌려서 answer에서 빼주니..
numbers에서 없던 숫자의 합이 나왔다!


## 🎈 코딩테스트1️⃣ 다른사람의 풀이! 

```java

class Solution {
    public int solution(int[] numbers) {
        int sum = 45;
        for (int i : numbers) {
            sum -= i;
        }
        return sum;
    }
}


```
- 향상된 for문에도 익숙해질 수 있도록 노력해야겟다.. ㅎㅎㅎ;



