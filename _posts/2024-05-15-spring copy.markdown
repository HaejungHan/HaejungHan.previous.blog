---
layout: post
title:  "TIL(20240515) JAVA Spring 코딩테스트"
date:  2024-05-15
categories: TIL JAVA Spring 코딩테스트
---

---------------------------------------------------------------------

## 🙌 오늘의 공부목록
- Spring : 1주차 강의 정리
- JAVA : 자바의 정석 chapter 7 마무리 chapter 8 강의 끝내기
- ~~코딩테스트 : 프로그래머스 2문제~~
- Spring : 강의 2주차 -8까지 듣기

## 🔄 내일의 공부목록

### ✔ 오늘 하루 느낀점


- 코딩테스트 : 오전 10시-12시
- JAVA-chapter 7(익명클래스),8 : 오후 13시-16시 정리 꼭 할 것!!!!!
- Spring 2주차 : 오후 16시~18시 , 1시간 저녁시간 , 19시~ 22시까지 

혹시나 빨리 끝냈을 경우에는 강의 반복해서 듣기.
꼭 코드 따라서 치기 (자동완성기능 개무시하기!!!!) 😱😱😱


---

# 📌 JAVA    
- 

---------------------------------------------------------------------

# 📌 Spring
- 메소드 키워드 정리 : 
```java
.stream()
.values()
.map()
.toList()
.keySet()
```



---------------------------------------------------------------------

# 📌 코딩테스트 1: 하샤드 수


## 🔒 문제 : 양의 정수 x가 하샤드 수이려면 x의 자릿수의 합으로 x가 나누어져야 합니다. 예를 들어 18의 자릿수 합은 1+8=9이고, 18은 9로 나누어 떨어지므로 18은 하샤드 수입니다. 자연수 x를 입력받아 x가 하샤드 수인지 아닌지 검사하는 함수, solution을 완성해주세요.

## 🚫 조건 : 
- x는 1 이상, 10000 이하인 정수입니다.

# 💡 필요했던 메서드
- Integer.toString() : int타입-> String타입으로 변환
- Integer.parseInt() : String -> int타입으로 변환
- .substring(index,index) -> 문자열 자르기 

# 🔓 문제풀이
```java
class Solution {
    public boolean solution(int x) {
        boolean answer = true;
        String s = Integer.toString(x); 
        int[] arr = new int[s.length()]; 
        int sum =0;
        
        for(int i=0; i<s.length(); i++) {
            arr[i] = Integer.parseInt(s.substring(i,i+1));
            sum += arr[i];
                if(x % sum == 0) {
                    answer = true;
                } else {
                    answer = false;
                }
            }
        
        return answer;
    }
}
```

## 🤷‍♀️ 코딩테스트 1 문제풀이를 하면서 느낀점
: 음.. 못풀거라고 생각했는데, 풀어서.. 놀랐다.. 그리고.. 정말 기뻤다.. 
다른사람들의 풀이를 보니 valueOf()와 split() 메서드를 사용했는데,
저번에 한번 구글링을 해서 어떤 문제를 이 두 가지 메서드로 풀었던 것 같은데.. 또 까먹었다..

```java
String [] tmp = String.valueOf(num).split("");
    int sum=0;
        for(String s:tmp) {
            sum+=Integer.parseInt(s);
        }

```

- String.valueOf()와 Integer.toString과의 차이점 기본int형은 다 잘 반환하지만 wrapper class Integer타입인 경우에는
String.valueOf 값을 잘 반환하지만 Integer.toString은 NullpointException 발생 
- split() : 구분자를 기준으로 문자열을 잘라 배열로 입력할 때 사용하는 메서드 !!!
1) split(String regex) -> 구분자를 바탕으로 배열 형식으로 문자열을 잘라줌
2) split(String regex, int limit) -> 위와 마찬가지로 구분자를 바탕으로 배열 형식으로 문자열을 자르지만 limit 수만큼 잘라줌

.....다음번엔 비슷한 문제가 나온다면 두 메서드를 꼭 사용해서 문제를 풀어보겠다!!


[문자열을 배열로 자르는 메서드 split]: (https://crazykim2.tistory.com/549)

--------------------------------------------------------------

# 📌 코딩테스트 2: 점의 위치 구하기


## 🔒 문제 : 사분면은 한 평면을 x축과 y축을 기준으로 나눈 네 부분입니다. 사분면은 아래와 같이 1부터 4까지 번호를매깁니다. x 좌표 (x, y)를 차례대로 담은 정수 배열 dot이 매개변수로 주어집니다. 좌표 dot이 사분면 중 어디에 속하는지 1, 2, 3, 4 중 하나를 return 하도록 solution 함수를 완성해주세요.

- x 좌표와 y 좌표가 모두 양수이면 제1사분면에 속합니다.
- x 좌표가 음수, y 좌표가 양수이면 제2사분면에 속합니다.
- x 좌표와 y 좌표가 모두 음수이면 제3사분면에 속합니다.
- x 좌표가 양수, y 좌표가 음수이면 제4사분면에 속합니다.

![점의 위치 구하기](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/b58d4788-42fa-44fa-af50-481907e65473/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-07-07%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.27.04%20%E1%84%87%E1%85%A9%E1%86%A8%E1%84%89%E1%85%A1%E1%84%87%E1%85%A9%E1%86%AB.png)

## 🚫 조건 : 
- dot의 길이 = 2
- dot[0]은 x좌표를, dot[1]은 y좌표를 나타냅니다
- 500 ≤ dot의 원소 ≤ 500
- dot의 원소는 0이 아닙니다.

# 💡 필요했던 메서드 : 메서드가 필요하진 않았다..


# 🔓 문제풀이
```java
class Solution {
    public int solution(int[] dot) {
        int answer = 0;
        int num = 0;
        int x = dot[0];
        int y = dot[1];
        
        if (x > num && y > num) {
         return 1;
        } else if (x < num && y > num) {
         return 2;
        } else if (x < num && y < num) {
            return 3;
        } else if (x > num && y < num) {
           return 4;
        }
        return answer;
    }
}

```

## 🤷‍♀️ 코딩테스트 2 문제풀이를 하면서 느낀점
: 다른사람들의 풀이를 보면 변수에 따로 값을 넣지 않고 바로 비교를 하는 것 같았다.
그래서 내가 작성한 풀이에서 num,x,y 변수를 만들지 말고 0,dot[0],dot[1]로 바로 비교했으면 더 좋았을 것 같다.
다음에는 조건에서 명확하게 드러난다면 따로 변수를 넣지 말것.. 👀

코딩테스트2는 딱 ... 내가 스스로 풀고 제출을 할 수 있을 정도라는 걸 알았다..
그것이 Lv.0.. 🤣 천천히 하루에 Lv.0이랑 Lv.1을 각각 한 문제씩 풀어보는 것도 괜찮다라는 생각이 들어서..
내일부터 2문제씩 풀어봐야겠다. 

---------------------------------------------------

