---
layout: post
title:  "TIL(20240523) [JAVA: String클래스]"
date:  2024-05-23
categories: TIL JAVA 코딩테스트
---


---------------------------------------------------------------------


# 📌 JAVA 

## 💡 String 클래스의 생성자와 메서드

- char[] -> String

```java
char[] c = {'H', 'i'};
String s = new String(c); // s = "Hi"
```

- charAt()

```java
String s = "Hi";
char c = s.charAt(1); // c = "i";
```

- concat()

```java
String s = "Hi";
String s2 = s.concat(" Java!"); // s2 = "Hi Java!";
```

- contains()

```java
String s = "Hijava";
boolean b = s.contains("av"); // true
```

- startsWith() / endsWith()

```java
String s = "Hijava";
boolean b1 = s.startsWith("Hi"); // true
boolean b2 = s.endsWith("java"); // true
```

- equals() / equalsIgnoreCase()

```java
String s = "Hello";
boolean b = s.equals("Hello"); // true
boolean b2 = s.equalsIgnoreCase("hello"); // true
```

- indexOf(ch/string) / indexOf(ch, index)

```java
String s = "Hello";
int index = s.indexOf('e'); // index = 1;
int index2 = s.indexOf('j'); // index2 = -1;
int index3 = s.indexOf("ll"); // index3 = 2;

int index4 = s.indexOf('l', 2); // index4 = 1;
int index5 = s.indexOf('e', 3); // index5 = -1; 
```

- lastIndexOf(ch) / lastIndexOf(string)

```java
String s = "java.lang.Object";
int index = s.lastIndexOf('.'); // index = 9;
int index2 = s.indexOf('.'); // index2 = 4;

String s1 = "java.lang.java";
int index3 = s.lastIndexOf("java"); // index3 = 10;
int index4 = s.indexOf("java"); // index4 = 0;
```

- length()

```java
String s = "Hello";
int i = s.length(); // i = 5
```

- split()

```java
String animals = "dog, cat, bear";
String[] arr = animals.split(","); // arr[0] = dog , arr[1] = cat , arr[2] = bear

String[] arr2 = animals.split(",", 2); // 2 -> 두개로 분리한다. arr[0] = dog, arr[1] = cat,bear
```

- substring()

```java
String s = "java.lang.Object";

String s1 = s.substring(10); // s1 = Object
String s2 = s.substring(3, 6); // s2 = a.l
```

- toLowerCase() / toUpperCase()

```java
String s = "Hello";

String s1 = s.toLowerCase(); // s1 = hello
String s2 = s.toUpperCase(); // s2 = HELLO
```

- trim()

```java 
String s = "   Hello java    ";
// 왼쪽 오른쪽 공백 제거
String s1 = "Hello java";
```

-  valueOf()  /  참조변수의 경우 toString()

```java
String s = String.valueOf(123); // s = "123"
String s1 = String.valueOf('a'); // s1 = "a"
String s2 = String.valueOf(123L); // s2 = "123"
String s3 = String.valueOf(1.23); // s3 = "1.23"
```

---------------------------------------------------------------------

# 📌 Spring

- WAS - 소켓 - 톰캣 
- JAR , WAR
- 스프링부트> 톰캣 : 개발자들이 주로 사용하는 옵션
- 스프링 : 틀




---------------------------------------------------------------------

# 📌 코딩테스트1️⃣ : 핸드폰 번호 가리기

## 🔒 문제 : 프로그래머스 모바일은 개인정보 보호를 위해 고지서를 보낼 때 고객들의 전화번호의 일부를 가립니다. 전화번호가 문자열 phone_number로 주어졌을 때, 전화번호의 뒷 4자리를 제외한 나머지 숫자를 전부 *으로 가린 문자열을 리턴하는 함수, solution을 완성해주세요.

## 🚫 조건 : phone_number는 길이 4 이상, 20이하인 문자열입니다.


# 💡 필요했던 메서드
- setCharAt() 

# 🔓 문제풀이

```java

class Solution {
    public String solution(String phone_number) {
        String answer = "";
        StringBuilder sb = new StringBuilder(phone_number);
        sb.replace(0,phone_number.length()-4, "*");
        answer = String.valueOf(sb);
        return answer;
    }
}

실행한 결괏값 "*4444"이 기댓값 "*******4444"과 다릅니다.
실행한 결괏값 "*8888"이 기댓값 "*****8888"과 다릅니다.

// 재시도

class Solution {
    public String solution(String phone_number) {
        String answer = "";
        StringBuilder sb = new StringBuilder(phone_number);
        for(int i=0; i<phone_number.length()-4; i++) {
            sb.setCharAt(i, '*');
        }
        answer = String.valueOf(sb);
        return answer;
    }
}

```

## 🤷‍♀️ 코딩테스트1️⃣ 문제풀이를 하면서 느낀점
: 처음에는 builder를 생각도 못하다가.. 곰곰히 생각해보니 phone_number는 변경가능한 문자열이어야 한다는 생각에
Stringbuilder,StringBuffer를 생각햇다. 구지 멀티쓰레드를 쓰면서 할 필요없는 단순한 문자열이기에
builder를 선택했고 처음에는 replace로 문자를 대체해야지 했는데.. 테스트를 통과하지 못한 결과에 대해서 보니, 뒤에 4자리 빼고 앞자리들은 모두 "*"하나로 대체된것.. 바보같았다. 다시 for문을 돌리면서 문자열의 길이-4 즉, 뒤에 4자리 빼고 *을 sb.setCharAt 메서드를 사용하여 하나씩 넣어줬다. 마지막으로 Stringbuilder를 문자열로 변환해서 return..! 
builder가 없엇다면 참 문제풀이가 복잡해졌을거 같다라는 생각이 든다... 이번 기회에  builder를 활용하여
문제를 풀게되어 다음번에도 활용할 수 있을 거 같은 자신감이 생김!  

## 🎈 코딩테스트1️⃣ 다른사람의 풀이! 

```java

class Solution {
  public String solution(String phone_number) {
     char[] ch = phone_number.toCharArray();
     for(int i = 0; i < ch.length - 4; i ++){
         ch[i] = '*';
     }
     return String.valueOf(ch);
  }
}

class Solution {
  public String solution(String phone_number) {
      String answer = "";

        for (int i = 0; i < phone_number.length() - 4; i++)
            answer += "*";

        answer += phone_number.substring(phone_number.length() - 4);

        return answer;
  }
}


```
- .. 정말 쉽게 푼 사람들이 많구나.. 음.. ? 정말 깔끔하게 풀었다.. substring을 이용해서! ...



