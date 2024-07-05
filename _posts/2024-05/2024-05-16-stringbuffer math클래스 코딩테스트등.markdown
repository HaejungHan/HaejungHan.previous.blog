---
layout: post
title:  "TIL(20240515) [JAVA:StringBuilder/StringBuffer,Math클래스]"
date:  2024-05-15
categories: TIL JAVA MySQL 코딩테스트
---



---

# 📌 JAVA    

### ✨ 들어가기전 : StringBuilder와 StringBuffer는 문자열을 수정할 수 있는 객체!

## 💡 StringBuilder
- 싱글 쓰레드로 작성된 것(1개당 하나의 작업)

```java

// 메서드 사용 시 공백도 인덱스번호에 포함되므로 주의할 것!
StringBuilder sb = new StringBuilder();

        // append
        sb.append("Hello Java!");
        System.out.println("append : "+ sb); // Hello Java!

        // insert(인덱스번호, "삽입문자")
        sb.insert(5, "!!!"); // Hello!!! Java!
        System.out.println("insert : " + sb);

        // replace(인덱스번호1,인덱스번호2, "대체문자")
        sb.replace(0,8, "I love");
        System.out.println("replace : " + sb); // I love Java!

        // delete(인덱스번호1,인덱스번호2)
        sb.delete(0,6); 
        System.out.println("delete : " + sb); // Java!

        // reverse 
        sb.reverse();
        System.out.println("reverse : " + sb); // !avaJ
        
        sb.reverse();
        sb.insert(6, " developer");  
        System.out.println("insert2 : " + sb); // Java! developer

        // setLength (길이를 줄이면 그 뒤의 문자들은 제거됨)
        sb.setLength(5);
        System.out.println("setLength : " + sb); // Java

        // charAt 특정인덱스 문자 가져오기
        char ch = sb.charAt(1);
        System.out.println("charAt : " + ch); // J

        // setCharAt (특정 인덱스 문자설정하기)
        sb.setCharAt(1,'j'); 
        System.out.println("setcharAt : " + sb); // java
        
        // substring
        String substring = sb.substring(1,3);  
        System.out.println("substring : " + substring);  // ja 
        
        //정렬함수
        .reverse().toString();
```

## 💡 StringBuffer
- 멀티 쓰레드로 작성된 것(동시에 여러 작업)
- 동기화되어 있으나 멀티쓰레드 프로그램이 아닌 경우에 buffer사용할 경우 성능저하

## 💡 Math클래스
- 수학관련 static 메서드의 집합
- round() : 원하는 소수점 아래 첫 번째 자리에서 반올림하기

```java

   double d = 90.7552;
        System.out.println(d *=100); // 9075.52
        System.out.println((double)Math.round(d)); // 9076

```
### 💡 Math클래스의 메서드

- abs() : 주어진 절대값을 반환함 (양수->양수, 음수->양수)
- double ceil() : 주어진 값을 올림하여 반환 (음수는 주의할 것 - 음수에서 큰 숫자는 반대..)
- double floor() : 주어진 값을 버림하여 반환 (음수 주의 - 음수에서 작은 숫자)
- max(a,b) : 주어진 두 값을 비교하여 큰 쪽을 반환함
- min(a,b) : 주어진 두 값을 비교하여 작은 쪽을 반환함 
- double random() : 0~1범위의 값을 반환함 0<x<1 (1은 미포함)
- round() : 소수점 첫째자리 반올림 

```java

// Math.abs()
        double d1 = 11.0;
        double d2 = -11.0;
        System.out.println(Math.abs(d1)); // 11.0
        System.out.println(Math.abs(d2)); // 11.0

// Math.ceil() 올림하여 큰 수 반환
        double d3 = 11.3; 
        double d4 = -11.3;
        System.out.println(Math.ceil(d3)); // 12.0;
        System.out.println(Math.ceil(d4)); // -11.0; 더 큰 수 반환 음수에서는 -11.3보다 -11.0이 더 큼...

// Math.floor() 버림하여 작은 수 반환
        System.out.println(Math.floor(d3)); // 11.0;
        System.out.println(Math.floor(d4)); // -12.0; // 더 작은 수 반환 음수에서는 -11.3보다 -12.0이 더 작음.. 

// Math.max()
        int num = Math.max(2,8);
        System.out.println("num의 더 큰 값은 : " + num); // 8
        double d = Math.max(1.4, 0.9);
        System.out.println("d의 더 큰 값은 : "+ d); // 1.4
        
// Math.min()
        int num1 = Math.min(2,8);
        double d1 = Math.min(1.4, 0.9);
        System.out.println("num1의 더 작은 값은 : "+ num1); // 2
        System.out.println("d1의 더 작은 갑은: "+ d1); // 0.9

// Math.random
        double d = Math.random();
        System.out.println(d); // 0.0< d < 1.0사이의 수 중 random

// 로또번호 뽑기
        
        Set<Integer> roto = new HashSet<Integer>();
        
        for(int i=0; roto.size()<6; i++){
            int num = (int)(Math.random() * 45) + 1; // 1 <= num < 46
            roto.add(num);
        }

        System.out.println(roto.toString());  // [16, 37, 6, 24, 10, 27] 랜덤 숫자 6개     

// Math.round
        double d = 88.767;
        System.out.println(Math.round(d)); // 89

```

---------------------------------------------------------------------

# 📌 MySQL
## 💡 DB 선택 -> 테이블 조회

```
//mysql - client
show tables;
show databases;

use 데이터베이스명;
select database();
show tables;

select * from 데이터베이스명;
```


```java
My SQL error1 : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed: org.springframework.jdbc.BadSqlGrammarException: StatementCallback; bad SQL grammar [SELECT * FROM memo]] with root cause

```

https://velog.io/@hyeddo/MySQL-%EC%99%84%EC%A0%84-%EC%82%AD%EC%A0%9C-%ED%9B%84-%EC%9E%AC%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0

winver

---------------------------------------------------------------------

# 📌 코딩테스트 1: 두 정수의 합

## 🔒 문제 : 두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요. 예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다.

## 🚫 조건 : 
- a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.
- a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.
- a와 b의 대소관계는 정해져있지 않습니다.

# 💡 필요했던 메서드
**없음**

# 🔓 문제풀이

```java
class Solution {
    public long solution(int a, int b) {
        long answer = 0;
        if (a == b) {
            answer = a;
        } else if (a < b) {
            for (int i=a; i<=b; i++) {
                answer += i;
            }
        } else if (a > b) {
            for (int i=b; i<=a; i++) {
                answer += i;
            }
        }
        return answer;
    }
}
```

## 🤷‍♀️ 코딩테스트 1 문제풀이를 하면서 느낀점
: 아직 코드를 줄이거나.. 간단하게 작성할 레벨은 아니라..
다른 사람들의 풀이를 보면서 아 이렇게 풀 수 있구나.. 하면서
또 알아가는 기분이다. 아래 다른 사람의 풀이를 보면서 
와 이렇게도 할 수 있구나.. 나의 코드에서 반으로 줄어든 느낌이다..
Math.min()/Math.max() 머리 속에 킵...!

## 🎈 코딩테스트 1 다른사람의 풀이! 

```java
class Solution {
  public long solution(int a, int b) {
      long answer = 0;

        int start = Math.min(a, b);
        int end = Math.max(a, b);

        for (int i = start; i <= end; i++) {
            answer += i;
        }

        return answer;
  }
}
```

- 메서드 :
1) Math.min(a, b); -> 두 인자 값 중 가장 작은 값을 리턴 
2) Math.max(a, b); -> 두 인자 값 중 가장 큰 값을 리턴
--------------------------------------------------------------

# 📌 코딩테스트 2: 문자 리스트를 문자열로 변환하기

## 🔒 문제 : 문자들이 담겨있는 배열 arr가 주어집니다. arr의 원소들을 순서대로 이어 붙인 문자열을 return 하는 solution함수를 작성해 주세요.

## 🚫 조건 : 
- 1 ≤ arr의 길이 ≤ 200
- arr의 원소는 전부 알파벳 소문자로 이루어진 길이가 1인 문자열입니다.

# 💡 필요했던 메서드 : 


# 🔓 문제풀이

```java
class Solution {
    public String solution(String[] arr) {
        String answer = "";
        for(int i=0; i<arr.length; i++) {
            answer += arr[i];
        }
        return answer;
    }
}
```

## 🎈 코딩테스트 2 다른사람의 풀이! 
- StringBuffer는 멀티 스레드 환경에서 안전하고, StringBuilder는 StringBuffer보다 성능이 우수하다는 장점이 있다. 따라서 동기화를 고려할 필요가 없는 상황에서는 StringBuffer보다 StringBuilder를 사용하는 것이 유리하다.
- StringBuilder는 변경 가능한 문자열을 만들어 주기 때문에, String을 합치는 작업 시 하나의 대안이 될 수 있다.
- new StringBuffer()로 객체를 생성하면 String을 사용할 때보다 메모리 사용량도 많고 속도도 느리다. 따라서 문자열을 추가하거나 변경하는 작업이 많으면 StringBuffer를, 적으면 String을 사용하는 것이 유리하다.

```java
//StringBuilder
class Solution {
    public String solution(String[] arr) {
        StringBuilder sb = new StringBuilder();
        for (String s: arr) {
            sb.append(s);
        }
        return sb.toString();
    }
}
//StringBuffer
class Solution {
    public String solution(String[] arr) {
        StringBuffer sb = new StringBuffer();
        for (String a : arr)
            sb.append(a);
        return sb.toString();
    }
}
```

- String.Join(구분자, 배열의변수명)
- 오버로드: String.Join(구분자, 배열의변수명,indexA,indexB)

```java
//String.join()
class Solution {
    public String solution(String[] arr) {
        return String.join("", arr);
    }
}

```

## 🤷‍♀️ 코딩테스트 2 문제풀이를 하면서 느낀점
: 문제를 풀면서 다른사람들의 풀이를 보며 공부를 해야겠다는 생각이
들었다. 메서드를 활용하면 정말 다양하게 풀 수 있다는 것을 많이 느낀다. StringBuilder와 StringBuffer는 변경가능한 문자열을 만들어 준다는 것! String.join으로 배열과 구분자를 연결해주는다는 것! 다음번에 꼭 잊지말고 써먹는 날이 있길 바래!!! 

---------------------------------------------------

