---
layout: post
title:  "TIL(20240508) JAVA : 추상클래스"
date:   2024-05-08 14:00:32 +0900
categories: JAVA TIL
---
20240508 JAVA : 추상클래스
===============================

✨내가 생각해봐야 할 것들
1) 자바에서 왜 추상클래스가 있는가?
2) 추상클래스는 어떤 경우에 사용되는가?
-------------------------------

# 💡 추상클래스
- 스스로는 인스턴스를 만들 수 없음
- 자식 클래스로 파생되기 위한 클래스
- 관련된 여러 클래스들의 공통분모를 정의하기 위한 클래스

## 📌 `abstract`클래스
- 인스턴스 생성 불가
- 부모클래스로서는 일반클래스와 같음, 다형성 구현가능

## 📌 `abstract`메서드
- 추상클래스에서만 사용가능
- 부모클래스에서 `abstract`메서드를 선언만 하고 자식클래스에서 반드시 구현해야한다. **구현하지 않을 시 컴파일 에러
- 접근제어자는 의미가 없음
- 클래스 메서드(변수명 앞에 static붙어있는)는 추상 메서드로 작성할 수 없음
- 인스턴스를 생성해서 쓰는 것이 아니므로 맞지 않음

## 💬 이해하기 위해서 만들어본 예제1 
```java
package ch08.ex04;

// 추상 클래스: Animal 
abstract class Animal { // 부모클래스

    protected final String name;

    public Animal (String name) {
        this.name = name;
    }

    // 추상 메서드: 하위 클래스에서 구현해야 함
    abstract void makeSound();

    // 일반 메서드: 모든 동물에 대해 공통적으로 사용 가능
    public void sleep() {
        System.out.println("동물이 잠을 잡니다.");
    }
}
```
```java
package ch08.ex04;

// 구상 클래스: Dog
class Dog extends Animal { // 자식 클래스 dog

    public Dog(String name) {
        super(name);
    }

    @Override
    // 추상 메서드 구현해야함!!!
    public void makeSound() {
        System.out.println("멍멍! 나는 %s 입니다.".formatted(name));
    }
}
```
```java
package ch08.ex04;

// 구상 클래스: Cat
class Cat extends Animal { // 자식 클래스 cat

    public Cat (String name) {
        super(name);
    }

    @Override
    // 추상 메서드 구현해야함!!!
    public void makeSound() {
        System.out.println("야옹! 나는 %s 입니다.".formatted(name));
    }
}

```
```java
package ch08.ex04;

// 메인 클래스
public class Main {
    public static void main(String[] args) {
        // 추상 클래스는 직접 인스턴스화할 수 없지만, 구상 클래스의 객체를 생성할 수 있습니다.
        Dog myDog = new Dog("클레어"); 
        Cat myCat = new Cat("토니");

        myDog.makeSound(); // 멍멍! 나는 클레어 입니다.
        myCat.makeSound(); // 야옹! 나는 토니 입니다.

        myDog.sleep(); // 동물이 잠을 잡니다. - 공통적인 부분
        myCat.sleep(); // 동물이 잠을 잡니다. - 공통적인 부분
    }
}

```

### 예제1을 만들어보면서 느낀점 
: 상위 클래스는 하위 클래스의 공통적인 부분을 추상화하고 그 하위클래스들이 구체화가 필요할 때 추상클래스를 사용하는가 .. 라는 생각이 든다. 
그러니까, 공통분모는 있지만 하위클래스들의 다른 부분이 많을 때 그때 사용하는 건가? 그래서 상위클래스가 하위클래스에게 메서드를 강제할 때
하위 클래스에서 작성하는 코드는 유연해질 수 있고, 또한 확장성도 높아진다. 

## 💬 이해하기 위해서 만들어본 예제2 - chat GPT한테 문제를 달라고 함... 🤣

> 구현해야 할 것은 새로운 음료 클래스입니다. 이 음료 클래스는 Beverage를 상속받아서 구현되어야 합니다. 구현해야 할 메서드는 추상 메서드인 pour()입니다. 이 메서드는 해당 음료를 컵에 따르는 과정을 나타내야 합니다. 새로운 음료를 추가하기 위해서는 다음 단계를 따르면 됩니다. 새로운 음료 클래스 생성: Beverage 클래스를 상속받는 새로운 클래스를 만듭니다. 생성자 구현: 새로운 음료 클래스에 필요한 생성자를 구현합니다. 생성자는 해당 음료의 이름과 용량을 받아와야 합니다. pour() 메서드 구현: pour() 메서드를 오버라이드하여 해당 음료를 컵에 따르는 과정을 구현합니다. 이 과정은 각 음료에 따라 다를 수 있습니다.

```java


```



Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
