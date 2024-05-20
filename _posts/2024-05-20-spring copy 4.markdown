---
layout: post
title:  "TIL(20240520) JAVA:제네릭스, 스트림; Spring; 코딩테스트: 서울에서 김서방 찾기;"
date:  2024-05-20
categories: TIL JAVA Spring 코딩테스트
---

---------------------------------------------------------------------

## 🙌 오늘의 공부목록
- JAVA 9강~10강 강의듣기
- ~~과제 피드백 수정하기~~
- Spring 1주차 복습 강의듣기
- JAVA, Spring 강의 정리

---

# 📌 JAVA  

## 💡
- 


---------------------------------------------------------------------


# 📌 Spring
- gradle이란? 버전을 설정하고 라이브러리를 가져오는 것 !

# 📌 MySQL



---------------------------------------------------------------------

# 📌 코딩테스트 : 서울에서 김서방 찾기

## 🔒 문제 : String형 배열 seoul의 element중 "Kim"의 위치 x를 찾아, "김서방은 x에 있다"는 String을 반환하는 함수, solution을 완성하세요. seoul에 "Kim"은 오직 한 번만 나타나며 잘못된 값이 입력되는 경우는 없습니다.

## 🚫 조건 : 
- seoul은 길이 1 이상, 1000 이하인 배열입니다.
- seoul의 원소는 길이 1 이상, 20 이하인 문자열입니다.
- "Kim"은 반드시 seoul 안에 포함되어 있습니다.

# 💡 필요했던 메서드
- format() : 리턴되는 문자열의 형태를 지정하는 메소드
- equals() : 문자열 값 비교

# 🔓 문제풀이
```java

class Solution {
    public String solution(String[] seoul) {
        String answer = "Kim";
        int x = 0;
        for(int i=0; i<seoul.length; i++) {
            if (seoul[i].equals(answer)){
                x = i;
            }
        }
        return String.format("김서방은 %d에 있다", x);
    }
}              

```

## 🤷‍♀️ 코딩테스트 1 문제풀이를 하면서 느낀점
: 반환타입이 String인데 계속 index를 반환하려고 했던 나의 조급함.. 
그리고 처음부터 indexOf를 배열에서 잘 활용하지 못해 아쉽다..
Arrays.asList()는 배열을 리스트형식으로 변환해준다는 것을 꼭 기억했다가 다음번에 
비슷한 문제를 만나면 활용할 것 ! 

## 🎈 코딩테스트 1 다른사람의 풀이! 

```java
import java.util.Arrays;
public class FindKim {
    public String findKim(String[] seoul){
        //x에 김서방의 위치를 저장하세요.
        int x = Arrays.asList(seoul).indexOf("Kim");        
        return "김서방은 "+ x + "에 있다";
    }
}
```
- asList() : 배열을 - 리스트로 변환
- indexOf() : 해당 문자열 인덱스번호 찾기 



--------------------------------------------------------------

