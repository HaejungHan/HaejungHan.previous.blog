---
layout: post
title:  "TIL(20240522) Spring:인증과 인가; 코딩테스트:나누어 떨어지는 숫자 배열;"
date:  2024-05-22
categories: TIL JAVA Spring 코딩테스트
---

---------------------------------------------------------------------

## 🙌 오늘의 공부목록

- ❎ JAVA 제네릭 강의 듣기 (1시간)
- 🔺(진행중) Spring 기본 1주차 복습 
- 🔺(진행중) Spring 숙련 2주차 강의
- 🔺(진행중) JAVA, Spring 강의 정리
- ✅ ~~코딩테스트 문제풀기(1/2)~~ 
---------------------------------------------------------------------

# 📌 Spring


---------------------------------------------------------------------

# 📌 코딩테스트1️⃣ : 음양 더하기

## 🔒 문제 : 어떤 정수들이 있습니다. 이 정수들의 절댓값을 차례대로 담은 정수 배열 absolutes와 이 정수들의 부호를 차례대로 담은 불리언 배열 signs가 매개변수로 주어집니다. 실제 정수들의 합을 구하여 return 하도록 solution 함수를 완성해주세요.

## 🚫 조건 : 
- absolutes의 길이는 1 이상 1,000 이하입니다.
- absolutes의 모든 수는 각각 1 이상 1,000 이하입니다.
- signs의 길이는 absolutes의 길이와 같습니다.
- signs[i] 가 참이면 absolutes[i] 의 실제 정수가 양수임을, 그렇지 않으면 음수임을 의미합니다.

# 💡 필요했던 메서드
- length() : 배열의 길이

# 🔓 문제풀이

```java
class Solution {
    public int solution(int[] absolutes, boolean[] signs) {
        int answer = 0;
        
        for(int i=0; i<absolutes.length; i++) {
            if(!signs[i]) {
                absolutes[i] *= -1;
            } 
            answer += absolutes[i];
        }
        return answer;
    }
}

```

## 🤷‍♀️ 코딩테스트1️⃣ 문제풀이를 하면서 느낀점
: 개인적으로 이 문제는 어렵다고 느껴지진 않았다... signs 배열에서 true인지 false인지만 구분 확인하면 absolutes의 배열에 담긴
절대값들이 양수인지 음수인지 확인이 되니 거기서 음수만 구분해주어 더해주면 끝이었다. 


## 🎈 코딩테스트1️⃣ 다른사람의 풀이! 

```java
class Solution {
    public int solution(int[] absolutes, boolean[] signs) {
        int answer = 0;
        for (int i=0; i<signs.length; i++)
            answer += absolutes[i] * (signs[i]? 1: -1);
        return answer;
    }
}
```
- 아.. 나도 삼항연산자로 3줄짜리를 한 줄로 바꾸고싶다..  항상 다른 사람들의 풀이를 볼 때 삼항연산자 응용해봐야지..해봐야지..
속으로만 생각하고 문제를 풀 때는 항상 머리속에서 리셋이 되는거지..🤣🤣
뭔지 모를.. 코드를 조금이라도 줄이고 싶은 욕심이 스멀스멀 생기고 있군..... 신기하넹..🙄


# 📌 코딩테스트2️⃣ : 문자열 붙여서 출력하기

## 🔒 문제 : 두 개의 문자열 str1, str2가 공백으로 구분되어 입력으로 주어집니다. 입출력 예와 같이 str1과 str2을 이어서 출력하는 코드를 작성해 보세요.

# 🔓 문제풀이

```java

import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String a = sc.next();
        String b = sc.next();
         System.out.println(a+b);
    }
}
```
## 🤷‍♀️ 코딩테스트2️⃣ 문제풀이를 하면서 느낀점
: 음.. 기본 ... 이당.. 😁


--------------------------------------------------------------
