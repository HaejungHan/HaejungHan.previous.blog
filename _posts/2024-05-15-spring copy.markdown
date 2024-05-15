---
layout: post
title:  "TIL(20240515) JAVA: 예외처리; Spring; 코딩테스트: 하샤드 수;"
date:  2024-05-15
categories: TIL JAVA Spring 코딩테스트
---

---------------------------------------------------------------------

## 🙌 오늘의 공부목록
- Spring : 1주차 강의 정리
- ~~JAVA : 자바의 정석 chapter 7 마무리 chapter 8 강의 듣기~~
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

## 💡 프로그램 오류
### 컴파일에러
1) 구문체크
2) 번역
3) 최적화

## 런타임에러 : 실행중 발생하는 에러 -> 프로그램 종료
1) 에러(error) : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류 (OOME :out of Memory 메모리부족)
2) 예외(exception) : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류
- 
3) 예외 처리(Exception handling) : 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 코드를 작성하는 것, 프로그램의 비정상 종료를 막고 정상적인 실행상태를 유지하는 것!

#### Exception (checked Exception) : 사용자가 발생시키는 예외
(⭐예외처리 필수-> try-catch문 필수! 컴파일러가 체크함)
- IOException : 입출력예외 (Input/Output)
- ClassNotFoundException : 클래스가 존재하지 않음
...
##### Exception- RuntimeException(Unchecked Exception) : 프로그래머의 실수로 발생하는 예외
(⭐예외처리 선택-> try-catch문 선택!)
- ArithmeticException : 산술계산 예외 (계산이 잘못된것)
- ClasCastException : 형변환 잘못됨
- IndexOutBoundException : 배열범위 벗어날 경우
- NullPointException : 
```java
// NullPointException 예시
String str = null;

System.out.println(str.length()); // NullPointException 발생

//해결방법
// 1) null이 발생할 것 같은 값에 대해서는 미리 예측해서 null체크를 해주기
if(str!=null){
System.out.println(str.length());
}

// 2) toString() 대신 String.valueOf() 사용하기
Integer i = null;
System.out.println(String.valueOf(i));
```

### 논리적에러 : 작성 의도와 다르게 동작 (프로그램 종료X)

## 💡 예외 처리하기 - try-catch문
```java
// 사용방법 
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣음
} catch (Exception1 e1) { // 예외가 발생할 경우 실행
    // Exception1이 발생했을 경우, 이를 처리할 문장
} catch (Exception2 e2) { 
    // EXception2가 발생했을 경우, 이를 처리할 문장
} ...
```
### 👀 이해하기 위한 예제(코드 따라서 작성하기)
```java
class Main {
    public static void main (String[] args){
        System.out.println(1);
        System.out.println(2);
        try {
        System.out.println(0/0); // ArithmeticException 에러 예상
        } catch (ArithmeticException ae) {
            if(ae instanceof ArithmeticException) {
            System.out.println("ArithmeticException");
            System.out.println("4");
            } 
        } catch (Exception e) { // Exception에 포함된 자식(?)예외상황에 하나라도 일치하는지 확인
            System.out.println("Exception 예외처리");
        }
        System.out.println("5");
    }   
}
```
```
// 출력 : 1 , 2 , ArithmeticException , 4 , 5
```
🎈 Exception은 가장 마지막 catch문에서 사용해야한다. 다른 예외사항에 해당하지 않을 경우 Exception에서 모든 예외사항을 처리하기 때문에!

## 💡 printStackTrac()와 getMessage()
- printStackTrac() : 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외메세지를 화면에 출력한다.
- getMessage() : 발생한 예외클래스의 인스턴스에 저장된 메세지를 얻을 수 있다.
```java
class Main {
    public static void main (String[] args){
        System.out.println(1);
        System.out.println(2);
        try {
        System.out.println(0/0); // ArithmeticException 에러 예상
        } catch (ArithmeticException ae) { // ArithmeticException 예외객체 생성
            ae.printStackTrace(); 
            System.out.println("예외메세지 : " + ae.getMessage()); // 참조변수 ae 의 getMessage메서드 호출로 예외메세지 String타입으로 출력
        } catch (Exception e) { 
            System.out.println("Exception 예외처리");
        }
        System.out.println("5");
    }   
}
```

## 💡 멀티 catch블럭
- 내용이 같은 catch블럭을 하나로 합친 것
```java
try{

} catch(ExceptionA e) { 
    e.printStackTrace();
} catch(ExceptionB e2) { // ExceptionA 와 내용동일
    e2.printStackTrace();
}

// 멀티 catch블럭
try{

} catch(ExceptionA | ExceptionB e) { // 내용 합친 것
    e.printStackTrace();
}
```
※ 주의사항 : 
1) 부모자식관계의 Exception 클래스 사용X
2) ExceptionA와 ExceptionB의 객체의 공통된 메서드만 사용가능(필요할 경우 형변환해서 사용가능)

## 💡 예외 발생시키기
```java
class Main {
    public static void main (String[] args){
        try {
            Exception e = new Exception("고의로 발생시킴");
            throw e; // catch 블럭 참조변수 e로 넘어감
            //위 구문을 throw new Exception("고의로 발생시킴")으로 줄여서 쓸 수 있음
        } catch (Exception e) {
            System.out.println("에러메세지 : " + e.getMessage()); // 에러메세지 : 고의로 발생시킴 출력
        }
    }   
}
```

## 💡 메서드 예외 선언하기
- 예외를 처리하는 방법 : try-catch문(직접처리), 예외 선언하기(예외 떠넘기기), (은폐..)덮기
```java
// 예외 떠넘기기
class Main {
    public static void main (String[] args){
        try {
            File f = createFile(""); // createfile메서드 호출 -> if문에서 true ->
            System.out.println(f.getName() + "파일이 성공적으로 생성되었습니다"); // 실행 안됨
        } catch (Exception e) {
            System.out.println("다시 입력해주세요 -> " + e.getMessage()); // e.getMessage : 파일이름이 유효하지 않습니다.
        }
    }
    static File createFile (String fileName) throws Exception { // 직접처리 하지 않겠다!
        if (fileName == null || fileName.equals("")) {
            throw new Exception("파일이름이 유효하지 않습니다.");
        }
        File f = new File(fileName);
        f.createNewFile();
        return f;
    }
}

// 직접처리
class Main {
    public static void main (String[] args){

        File f = createFile("");
        System.out.println(f.getName() + "파일이 성공적으로 생성되었습니다");

    }
    static File createFile (String fileName) { // 직접처리
        try {
            if (fileName == null || fileName.equals("")) {
                throw new Exception("파일이름이 유효하지 않습니다.");
            } else { File f = new File(fileName); }
        } catch (Exception e) {
            fileName = "제목없음";
        }
        File f = new File(fileName);
        return f;
    }
}

```
🎈 상황에 맞게 판단하여 사용하자!
- 예외가 발생한 곳에서 예외를 처리하거나 (직접처리)
- 메서드를 호출한 곳에서 예외처리하거나 (떠넘기기)

## 💡 finally 블럭
- 예외 발생여부와 관계없이 수행되어야 하는 코드를 넣는다. 
```java
class Main { 
    public static void main (String[] args){

        File f = createFile("");

    }
    static File createFile (String fileName) { // 직접처리
        try {
            if (fileName == null || fileName.equals("")) {
                throw new Exception("파일이름이 유효하지 않습니다.");
            } else { File f = new File(fileName); }
        } catch (Exception e) {
            fileName = "제목없음";
        } finally {
            System.out.println(fileName + " 파일을 생성하였습니다.");
        }
        File f = new File(fileName);
        return f;
    }
}

```

## 💡 사용자 정의 예외 만들기
- 직접 예외 클래스를 정의할 수 있다.
- Exception과 RuntimeException중에서 선택
```java
//사용방법
clas MyException extends Exception {
    MyException(String msg) {
        super(msg);
    }
}
```

## 💡 예외 되던지기 (Exception re-throwing)
- 예외를 처리한 후에 다시 예외를 발생시키는 것
```java
class Main {
    public static void main (String[] args){

        try {
            sometingToDo();
        } catch (Exception e) {
            System.out.println("Main메서드에서 예외를 처리합니다.");
        }

    }
    static void sometingToDo() throws Exception { // 직접처리
        try {
           throw new Exception();
        } catch (Exception e) {
            System.out.println("somethingToDo메서드에서 예외를 처리합니다.");
            throw e; // 다시 메인메서드의 Exception 던짐
        }
    }
}
```
🎈 팀프로젝트를 하면서 이런 경우가 있었는데, somethingToDo같은 메서드는 B클래스에서 
1차적으로 RuntimeException으로 걸러주고, 혹시나 다른 예외가 발생시에는 Main클래스에서
2차적으로 Exception으로 걸러주게 처리했던 적이 있다! 이런 형식으로 사용하면 되는 게 아닐까.. 생각이 든다. 

## 💡 연결된 예외(chained exception)
- 어떤 경우에 사용되는가? 
1) 여러 예외를 하나로 묶어서 다루기 위해(너무 많은 catch블럭이 생길때 유용)
```java
// 설치 중에 발생할 예외들이 너무 많을 경우
try {
    install(); 
} catch (SpaceException e) {
    e.printStackTrace();
} catch (MemoryException e) {
    e.printStackTrace();
} catch (Exception e) {
    e.printStackTrace();
}

// 여러 예외를 하나의 예외로 묶기
try {
    install(); 
} catch (InstallException e) { // SpaceException과 MemoryException을 InstallException에 넣어버림
    e.printStackTrace();
} catch (Exception e) {
    e.printStackTrace();
}

// 위와 같이 하려면 install 메서드에서 처리해줘야 할 것들
void install() throws InstallException {
    try {
        startInstall(); // 설치를 시작하는 도중에 Error발생시
    } catch (SpaceException e) {
        InstallException ie = new InstallException("설치 중 예외 발생");
        ie.initCause(e); // InstallException 객체에 SpaceException 넣기
        throw ie;
    } catch (MemoryException m) {
        InstallException ie = new InstallException("설치 중 예외 발생");
        ie.initCause(m); // InstallException 객체에 MemoryException 넣기
        throw ie;
    }
}
```
🎈 설치 중에 발생하는 예외들을 한 번에 묶어 처리할 수 있다.

2) checked 예외를 unchecked 예외로 변경하려 할 때
- 예를들어 "MemoryException m"을 선택예외로 변경하고 싶을 때
```java
static void startInstall() throws SpaceException, MemoryException {
    if (!enoughSpace()) {
        throw new SpaceException(" 공간이 부족합니다.");
    }

    if (!enoughMemory()) {
        throw new MemoryException("메모리가 부족합니다."); // 필수예외->선택예외로 변경 하고 싶다면?
    }
}

static void startInstall() throws SpaceException { // 필수예외만 throws
    if (!enoughSpace()) {
        throw new SpaceException("공간이 부족합니다.");
    }

    if (!enoughMemory()) {
        // RuntimeException은 선택예외이기에 MemoryExceptio을 RuntimeException에 넣어 처리
        throw new RuntimeException(new MemoryException("메모리가 부족합니다.")); 
    }
}

```
🎈 필수 예외를 선택 예외로 바꿀 수 있다.
---------------------------------------------------------------------

# 📌 Spring

```java
My SQL error 1: Access denied for user ''@'localhost' (using password: YES)

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

