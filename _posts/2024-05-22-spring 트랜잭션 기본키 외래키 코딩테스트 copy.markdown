---
layout: post
title:  "TIL(20240522) [Spring:트랜잭션] [DB:기본키(PK)와 외래키(FK)]"
date:  2024-05-22
categories: TIL DB Spring 코딩테스트
---


---------------------------------------------------------------------


# 📌 Spring

## 💡 Autowired
- 필요한 의존 객체의 "타입"에 해당하는 빈을 찾아 주입한다.(생성자/setter/필드)

## 💡 Transactional 트랜잭션

[트랜잭션과 어노테이션](https://tecoble.techcourse.co.kr/post/2021-05-25-transactional/)

### 💡 COMMIT과 ROLLBACK 
- COMMIT : 모든 작업을 정상적으로 처리하겠다고 확정하는 명령어, 트랜잭션의 처리과정을 데이터베이스에 반영하기 위해서, 변경된 내용을 영구저장함
- ROLLBACK : 작업 중 문제가 발생했을 때, 트랜잭션의 처리 과정에서 발생한 변경사항을 취소하고 트랜잭션 과정을 종료시킨다. 이전 COMMIT한 곳까지만 복구함

[COMMIT과 ROLLBACK](https://wikidocs.net/4096)

# 📌 DB

## 💡 기본키와 외래키

- `PK` : (기본키)primary key는 Entity를 식별하는 대표키, unique하며 null 허용X, 하나의 테이블에는 반드시 하나의 기본키만 존재해야함
1) 유일성과 최소성, 해당 레코드를 식별할 때 기준이 되는 반드시 필요한 키
- `FK` : (외래키)foreign key는 다른 테이블의 PK를 참조하는 키, PK와 동일한 domain을 갖게되고, 즉, 테이블간의 관계를 나타낼 때 사용해 다른 테이블의 기본키를 참조해 외래키를 사용함
 1) 중복 데이터 제거를 위해 테이블을 분리할 때, 반드시 필요한 개념
 2) '한 테이블에 존재하는 다른 테이블의 정보'이기 때문에 외래키라고 부름

[기본키와 외래키](https://velog.io/@kon6443/DB-%EA%B8%B0%EB%B3%B8%ED%82%A4-%EC%99%B8%EB%9E%98%ED%82%A4-%ED%9B%84%EB%B3%B4%ED%82%A4-%EB%B3%B5%ED%95%A9%ED%82%A4-%EA%B0%9C%EB%85%90-4x1bgz5w)

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
