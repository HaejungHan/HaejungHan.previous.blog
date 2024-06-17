---
layout: post
title:  "TIL(20240615) [Controller MockMvc 테스트 코드]"
date:  2024-06-15
categories: TIL Spring
---

---------------------------------------------------------------------


# 📌 Spring

## 💡 Controller MockMvc 테스트 코드 

Controller 테스트 코드를 작성하기 앞서서 Controller테스트에서는 무엇을 체크해야 할까? 라는 생각이 들었고, 무작정 테스트코드를 작성하기 보다는 Controller의 책임범위, 어떤것을 테스트 하면 좋을지에 대해서 정리를 하고 시작한다면 효율적으로 진행할 수 있을 것 같아 개념정리를 하고 시작해보았다.

3 Layer Architecture 중 클라이언트의 가장 가까이에서 요청과 응답을 처리하는 Controller에 대해
어떤 테스트를 진행하면 좋을지 개념정리!

**Controller의 책임범위**

    1. http 요청을 듣고, http 응답을 책임진다.
    2. http 요청을 파싱하여 들어온 데이터를 역직렬화 한다.
    3. http 요청 구믄을 분석하고, url/body/param 등의 변수에서 java 객체를 생성한다.
    4. 들어온 데이터가 유효한지 검증한다. 
    5. 비지니스 로직(service)를 호출한다.
    6. 비지니스 로직의 출력을 http 응답으로 직렬화 한다.
    7. 처리 도중에 예외가 발생하면 사용자에게 오류메세지와 http 상태응답을 한다.

이렇게 정리를 한번 하고 나니 Controller가 많은 일들을 하고 있구나 라고 생각했던 게 다시 한번 되새김질이 되었고, Controller에는 비지니스로직을 두지 말라고 하는 이유도 바로 위의 책임범위가 충분히 많기 때문에
역할 분리가 필요하다는 것을 알 수 있다. 

그렇다면 어떤걸 Controller에서 테스트 해야하는가?

    1. Controller가 해당 url을 잘 읽어들이는 지 확인
    2. 들어온 데이터가 원하는 java object로 직렬화가 되는지 확인
    3. 들어온 데이터의 validation 확인이 잘 되는지
    4. 비지니스로직을 잘 호출 했는지
    5. http 응답 본문에 json형태로 잘 들어갔는지 
    6. 예외처리가 잘되었는지

스프링부트에서 제공하는 Controller의 대표적인 @SpringBootTest, @WebMvcTest 어노테이션 중
@WebMvcTest 를 사용하여 테스트를 하려고 한다. 


**@WebMvcTest vs @SpringBootTest**
  - @SpringBootTest 는 프로젝트의 전체 컨텍스트를 로드하여 빈을 주입하기 때문에 속도가 느리고, 통합 테스트를 할 때 많이 사용한다.
  - 필요한 빈만을 등록하여 테스트를 진행하고자 한다면 슬라이스 테스트 어노테이션인 @WebMvcTest를 사용하는 것이 효율적이다.

**테스트 코드 작성 순서**

1) MockMvc를 통해서 API 호출, 해당 컨트롤러에서 의존하고 있는 객체는 Mock객체로 만들어 주입(@MockBean)사용하기
2) Mock 객체는 "가짜 객체" 라 리턴 값이 없기에 given, when 등으로 원하는 값을 리턴하도록 조작해줘야 함
3) 로직이 진행된 후 verify를 통해 검증하기 (나는 service테스트에서 해당 부분을 처리하도록 바꾸었다.)


