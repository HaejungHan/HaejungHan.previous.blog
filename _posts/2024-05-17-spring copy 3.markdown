---
layout: post
title:  "TIL(20240517) JAVA:래퍼(wrapper)클래스, 문자열을 숫자로 변환, 오토박싱과 언박싱; 코딩테스트: 콜라츠 추측;"
date:  2024-05-17
categories: TIL JAVA Spring 코딩테스트
---

---------------------------------------------------------------------


## 🙌 오늘의 공부목록
- JAVA chapter 9 강의듣기
- JAVA 강의듣고 정리하기
- ~~코딩테스트 문제풀기~~

---

# 📌 JAVA

## 💡 래퍼(wrapper)클래스
- 8개의 기본형을 객체로 다뤄야 할 때 사용하는 클래스
- boolean, char, byte, short, int, long, float, double -> Boolean, Character, Byte, Short, Integer, Long, Float, Double

```java
Integer i = new Integer(100);
Integer i2 = new Integer(100);

        System.out.println(i == i2); // false
        System.out.println(i.equals(i2)); // true 
        System.out.println(i.compareTo(i2)); // 0  -> 같으면 0 오른쪽 값이 작으면 양수 오른쪽 값이 크면 음수  
        System.out.println(i.toString()); // 100
        System.out.println(Integer.TYPE); // int
```

## 💡 Number클래스
- 모든 숫자의 래퍼 클래스의 조상
- Integer -> intValue(), Doube-> doubleValue(), Float-> floatValue(), Long-> longValue()

![래퍼클래스의 조상](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTn4atu3VT2U6gV_zQERtnbM0rwCPHO_ecoLTMKT51s&s)

## 💡 문자열을 숫자로 변환하기
- 문자열-> 기본형 : parseInt()
- 문자열-> 래퍼클래스 : valueOf()

```java
        int num = Integer.parseInt("1000");
        System.out.println(num); // 1000
        int num2 = Integer.valueOf("2000");
        System.out.println(num2); // 2000
        int num3 = new Integer(101).intValue();
        System.out.println(num3); // 101

        // n진법 변환
        int num = Integer.parseInt("1000",2); // 2진수로 변환
        System.out.println(num); // 8
```
- 래퍼클래스 -> 문자열 : toString()

## 💡 오토박싱 & 언박싱
- 컴파일러가 자동변환
- int-> integer : 오토박싱
- integer -> int : 언박싱

```java
        int num = 8;
        Integer i = new Integer(11);

        int sum = num + i; // i를 기본형으로 변환 integer-> int

        System.out.println(sum); // 19
    
```

```java
    ArrayList<Integer> list = new ArrayList<Integer>();
        list.add(10); // = list.add(new Interger(10)) 오토박싱 int -> Integer 
        System.out.println(list.toString()); // [10]


        int i = list.get(0); // int i = list.get(0).intValue() 언박싱 Integer -> int
        System.out.println(i); // 10
```

```java
        int num = 10;

        Integer i = (Integer)num; // 언박싱 integer -> int
        System.out.println(i); //  10
        Object o = (Object)num; // (Object)Integer.valueOf(num)
        System.out.println(o); //  10        
        
        Long l =  100L; // new Long(100L) Long-> long 언박싱
        System.out.println(l); //  100
        
        int sum = num + i; // 
        System.out.println(sum); //  20
        long lsum = i + l; // 참조형 간의 덧셈 가능
        System.out.println(lsum); //  110 

        Integer i2 = new Integer(30);
        int num2 = (int)i2; // 오토박싱 int -> integer
        System.out.println(num2); //  30
```



---------------------------------------------------------------------


# 📌 Spring

## ⭐ 나만의 일정관리 앱 서버 만들기!

# 📌 MySQL


---------------------------------------------------------------------

# 📌 코딩테스트 1: 콜라츠 추측

## 🔒 문제 : 1937년 Collatz란 사람에 의해 제기된 이 추측은, 주어진 수가 1이 될 때까지 다음 작업을 반복하면, 모든 수를 1로 만들 수 있다는 추측입니다. 작업은 다음과 같습니다.

- 1-1. 입력된 수가 짝수라면 2로 나눕니다. 
- 1-2. 입력된 수가 홀수라면 3을 곱하고 1을 더합니다. 
- 2. 결과로 나온 수에 같은 작업을 1이 될 때까지 반복합니다. 예를 들어, 주어진 수가 6이라면 6 → 3 → 10 → 5 → 16 → 8 → 4 → 2 → 1 이 되어 총 8번 만에 1이 됩니다. 위 작업을 몇 번이나 반복해야 하는지 반환하는 함수, solution을 완성해 주세요. 단, 주어진 수가 1인 경우에는 0을, 작업을 500번 반복할 때까지 1이 되지 않는다면 –1을 반환해 주세요.

## 🚫 조건 : 입력된 수, num은 1 이상 8,000,000 미만인 정수입니다.


# 💡 필요했던 메서드
**없음**

# 🔓 문제풀이
```java
class Solution {
    public int solution(int num) {
        int answer = 0;
        
        while(num!=1) {
            
            if (num % 2 == 0) {
                num = num / 2;
            } else if (num % 2 == 1) {
                num = (num * 3) +1;
            }
            answer++;
            
            if (answer == 500) {
                answer = -1;
                return answer;
             }
        } 
        return answer; 
    }              
}
```

## 🤷‍♀️ 코딩테스트 1 문제풀이를 하면서 느낀점
: 마지막 3번째 테스트에서 통과를 못해서 고민을 많이 했는데,
while의 조건을 answer <=500 으로 잡고 있어서 -1이 반환되야 할 상황에서는
어디서 return문을 넣어야 할지 막막해지면서 조건문이 잘못되었다라는 것을 인지했다.
조건을 num !=1로 바꾸면서 while안의 블록에서 처리해야 할 방법에 대해서 풀렸던 것 같다.
정말 간단한 문제 같은데.. 1시간이 걸렸다.. 후우.. 풀리니까 재밌는 코딩테스트인데..
안풀리면 내가 멍청이 같아보여서 묘한 매력이 있다..😁

## 🎈 코딩테스트 1 다른사람의 풀이! 

```java
class Collatz {
    public int collatz(int num) {
    long n = (long)num;
    for(int i=0; i<500; i++){
        if(n==1) return i; 
      n = (n%2==0) ? n/2 : n*3+1;
    }
        return -1;
    }
```

TMI) 삼항연산자는 정말 쉬운 듯 한데 응용이 안된다.. 하하하하하하핳 🤣
오늘은 과제를 제출해야 하므로 1문제로 족하자..ㅠ_ㅠ...

--------------------------------------------------------------

