---
layout: post
title:  "TIL(20240510) JAVA : 클래스 기초 예제 만들어서 익숙해지기 "
date:  2024-05-10
categories: TIL JAVA
---



----------------------------------------------------------


## 📌 Make a sample ①

```java

package ch10.first;

public class Person { 
    private String name;
    private int age;
    private int ageNewyear;
    private String relation;

    // 생성자
    public Person (String name, int age, String relation) {
        this.name = name;
        this.age = age;
        this.ageNewyear = ++age;
        this.relation = relation;
    }

    // 정보 출력 메서드
    public void introduce() {
        System.out.println("안녕하세요 제 이름은 "+ name + " 이고 나이는 " + age + " 입니다.");
        System.out.println("내년에는 "+ ageNewyear +" 입니다 ㅠㅠㅠ");
    }

   public String getRelation () {
        return relation;
   }

   public void tellRelation () {
       System.out.println(name + "님의 배우자는 " + getRelation() + "입니다.");
   }

}

```
```java

package ch10.first;

public class Main {
    public static void main(String[] args) {
        Person person1 = new Person("Hanni",33, "Yoni");
        Person person2 = new Person("Yoni", 35, "Hanni");

        person1.introduce();  // 안녕하세요 제 이름은 Hanni 이고 나이는 33 입니다. 내년에는 34 입니다 ㅠㅠㅠ
        person1.tellRelation(); // Hanni 님의 배우자는 Yoni 입니다.

        person2.introduce(); // 안녕하세요 제 이름은 Yoni 이고 나이는 35 입니다. 내년에는 36 입니다 ㅠㅠㅠ
        person2.tellRelation(); // Yonni 님의 배우자는 Hanni 입니다.

    }
}

```
## 📌 Make a sample ②

```java

package ch10.second;

public class BankAccount {
    private String accountNumber;
    private double balance;

    // 생성자
    public BankAccount(String accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }

    // 입금 메서드
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println(amount + " 원 입금되었습니다. 현재 잔액은 " + balance + " 입니다.");
        } else {
            System.out.println("입금 금액이 없습니다.");
        }
    }

    // 출금 메서드
    public void withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
            System.out.println(amount + "원을 출금했습니다. 남은 잔액은 " + balance + " 입니다.");
        } else {
            System.out.println("출금할 수 없습니다. 잔액이 부족하거나 유효하지 않은 금액입니다.");
        }
    }

    // 잔액조회 메서드
    public double getBalance() {
        return balance;
    }

}

```
```java
package ch10.second;

public class Main {
    public static void main(String[] args) {
        BankAccount account1 = new BankAccount("A1", 0);

        account1.deposit(3000);
        account1.withdraw(200);
        account1.getBalance();
    }
}
```



--------------------------------------------------

