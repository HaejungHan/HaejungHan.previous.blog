---
layout: post
title:  "TIL(20241003) [공공데이터포털 OPEN_API활용]"
date:  2024-10-03
categories: TIL
---

----------------------------------------------------------------------------

이전에 커리어넷에서 대학교/학과검색 OPEN API를 신청하고 연결했었지만 학과검색 기능을 구현하는 부분에서 해당 학교에 속해있는 학과 리스트만 불러오는 부분이 불가능해서 커리어넷 데이터는 사용하지 않기로 결정했고,

알아보다가 공공데이터 포털에 내가 구현하려는 기능에 부합한 데이터를 찾았고 다시 구현해보려고 한다. 

[한국대학교육협의회_대학알리미 대학별 학과정보](https://www.data.go.kr/iim/api/selectAPIAcountView.do)

일단 커리어넷과 다르게 스웨거를 통해서 데이터를 확인할 수 있었고 문서가 없어서 조금 구현하는데 애를 먹을 것 같다..😂


### 💡 ERROR : 등록되지 않은 인증키 

![image](https://github.com/user-attachments/assets/3102bc35-0866-4888-ac81-dfef85280304)

Postman으로 학교정보 검색기능을 restemplate, UriComponent로 구현해서 요청한 결과 위와 같은 에러가 발생하였다.

#### 문제확인 과정

1) 디버깅을 해보니 문자열을 제대로 불러오지 못하고 있었다. 디코딩한 키를 인코딩 하는 과정에서 문제가 발생하는 듯 했다.

```java
// 서비스키 일부
Tz1OIA%3D%3D -> Tz1OIA%253D%253D
knX6X7+bcj -> knX6X7%2Bbcj
```

2) format을 이용해서 문자열 인코딩 키를 그대로 넘겨주었지만 그래도 해결되지 않았다. 

3) 포스트맨이나 웹브라우저에서는 인코딩 키로 서비스키를 넣어야 하지만 스프링 서버에서 요청을 날릴때는 디코딩 된 키를 넣어야 한다는 것을 알게 되었다.

#### 문제 해결 과정

![image](https://github.com/user-attachments/assets/e33d5606-ffc2-4a5b-b816-e35c6999a02e)

구글 검색하니 등록되지 않은 인증키와 관련된 내용들이 보였고 위의 그림을 참고해서 다시 코드를 수정하였다.

1) 디코딩된 서비스 키를 UTF_8로 인코딩
2) URI 클래스를 사용하여 문자열을 포맷하여 URL을 담은 URI 생성
3) restTemplate의 getForObject나 exchange를 사용

- getForObject: 단순하게 응답 본문만 필요할 때 사용. 상태 코드와 헤더는 처리하지 않음.
- exchange: 더 많은 정보를 얻고 싶을 때 사용. 상태 코드와 헤더를 포함하여 응답을 처리할 수 있음.

![image](https://github.com/user-attachments/assets/ba23a9ce-ba26-4071-875c-5329b0ce6c61)

데이터를 잘 불러오고 있다.


** 📌 스프링 3.0에서부터 지원하는 RestTemplate은 HTTP 통신에 유용하게 쓸 수 있는 템플릿이다. REST 서비스를 호출하도록 설계되어 HTTP 프로토콜의 메서드 (GET, POST, DELETE, PUT)에 맞게 여러 메서드를 제공한다. 