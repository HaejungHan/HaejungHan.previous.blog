---
layout: post
title:  "TIL(20240515) JAVA: ; Spring; 코딩테스트: 두 정수의 합, 문자 리스트를 문자열로 변환하기;"
date:  2024-05-15
categories: TIL JAVA Spring 코딩테스트
---

---------------------------------------------------------------------

## 🙌 오늘의 공부목록
- Spring 2주차 강의 듣기 
- Spring 개인과제 필수기능구현까지 해보기
- JAVA chapter 9 강의듣기
- JAVA 강의듣고 정리하기

## 🔄 내일의 공부목록

### ✔ 오늘 하루 느낀점

---

# 📌 JAVA    


---------------------------------------------------------------------

# 📌 Spring

```java


```

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

