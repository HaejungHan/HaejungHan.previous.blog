---
layout: post
title:  "TIL(20240520) JAVA: 제네릭; Spring; 코딩테스트: 서울에서 김서방 찾기;"
date:  2024-05-20
categories: TIL JAVA Spring 코딩테스트
---

---------------------------------------------------------------------

## 🙌 오늘의 공부목록
- JAVA 제네릭,스트림 강의 듣기
- ~~과제 피드백 수정하기~~
- Spring 1주차 복습 강의듣기
- JAVA, Spring 강의 정리

---

# 📌 JAVA  

## 💡 제네릭스(Generics)
- 컴파일시 타입을 체크해 주는 기능, 다양한 데이터 타입을 처리할 수 있도록 하는 기능
- 제네릭을 사용하면 타입 안정성과 코드 재사용성을 높일 수 있음-> 객체 타입의 안정성을 높이고, 형변환의 번거로움을 줄여줌

![제네릭](https://user-images.githubusercontent.com/33862991/109501510-07125300-7adb-11eb-9d55-7d9efb2ff2d0.png)

- 형변환 에러 : ClassCastException

## 💡 제네릭 타입과 다형성
- 참조변수와 생성자의 대입된 타입은 일치해야 한다.

```java
ArrayList<Person> list = new ArrayList<Person>(); // 가능 대입된 타입 일치
ArrayList<Person> list = new ArrayList<Box>(); // error 대입된 타입 불일치
```

- 제네릭 클래스간의 다형성은 성립 (대입된 타입은 일치해야함)

```java
List<Person> list = new ArrayList<Person>(); // 가능, 다형성 ArrayList가 List를 구현
List<Person> list = new LinkedList<Person>(); // 가능, 다형성 LinkedList가 List를 구현
```

-  매개변수의 다형성도 성립 

```java
class Product{}
class Tv extends Product{}
class Audio extends Product{}

        ArrayList<Product> list = new ArrayList<Product>(); 
        list.add(new Product()); // 가능
        list.add(new Tv()); // 가능 
        list.add(new Audio()); // 가능
        printAll(list);  // Product@3fee733d Tv@5acf9800 Audio@4617c264

        
    public static void printAll(ArrayList<Product> list)
            for(Product p : list) {
                System.out.println(p);
            }
```

```java
 ArrayList<Integer> list = new ArrayList<Integer>();

        list.add(12); // 오토박싱
        list.add(13);
        
//        list.add("14"); // error String 타입 불가

        System.out.println(list); // [12, 13]
```

### 🎈 String 타입을 넣으려면?

```java
    ArrayList<Object> list = new ArrayList<Object>(); // Object타입으로 변환

        list.add(12); // 오토박싱
        list.add(13);       
        list.add("14"); 

        String str = (String)list.get(2);
        System.out.println(str); // 14
```

### 👀 예제 만들어서 이해하기


```java
class Person {
    private String name;
    private int age;

    public Person (String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String toString() {
        return "나의 이름은 "+name+ " 나이는 "+age+" 입니다";
    } 
}
class Main {
    public static void main(String[] args) {
    ArrayList<Person> list = new ArrayList<Person>();
        Person person = new Person("hanni", 33);
        list.add(person);
        System.out.println(list.get(0));
        
    }
}
```

```java
class Box<T> {
    private T contents;

    public void setContents(T contents) {
        this.contents = contents;
    }
    public T getContents() {
        return contents;
    }

    public void displayContents() {
        System.out.println("contents :"+ contents);
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String toString(){
        return "사람의 이름은 : "+name+" 나이는 : "+age+" 입니다";
    }
}
class Main {
    public static void main(String[] args) {
    // 정수형 box 생성
        Box<Integer> integerBox = new Box();
        integerBox.setContents(100);
        integerBox.displayContents(); // contents :100

    // 문자열형 box생성
        Box<String> stringBox = new Box();
        stringBox.setContents("apple");
        stringBox.displayContents(); // contents :apple

    // 사용자 정의 타입 Box 생성
        Box<Person> person = new Box();
        person.setContents(new Person("hanni", 33));
        person.displayContents(); // contents :사람의 이름은 : hanni 나이는 : 33 입니다

        
        
    }
}
```


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

