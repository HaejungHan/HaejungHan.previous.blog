---
layout: post
title:  "TIL(20241006) [Maria DB - DB설계와 SQL 연습]"
date:  2024-10-06
categories: TIL
---

----------------------------------------------------------------------------

이전에는 MySQL만 사용해오다가 RDB 종류도 많은터라 다른 DB도 사용해보고 싶은 생각에
workbench 데이터베이스 관리도구를 통해 MariaDB로 간단하게 DB설계와 SQL 연습을 해보려고 한다.

## 📌 주제 : 온라인 쇼핑몰

### 💡 데이터 베이스 설계 요구사항

```
1. 사용자 테이블 (users)
id: INT, PRIMARY KEY, AUTO_INCREMENT
name: VARCHAR(100)
email: VARCHAR(100), UNIQUE
password: VARCHAR(100)
registration_date: DATE

2. 상품 테이블 (products)
id: INT, PRIMARY KEY, AUTO_INCREMENT
name: VARCHAR(100)
category: VARCHAR(50)
price: DECIMAL(10, 2)
stock: INT
rating: DECIMAL(3, 2)

3. 주문 테이블 (orders)
id: INT, PRIMARY KEY, AUTO_INCREMENT
user_id: INT, FOREIGN KEY (users.id)
order_date: DATE
total_amount: DECIMAL(10, 2)

4. 주문 상세 테이블 (order_items)
id: INT, PRIMARY KEY, AUTO_INCREMENT
order_id: INT, FOREIGN KEY (orders.id)
product_id: INT, FOREIGN KEY (products.id)
quantity: INT
price: DECIMAL(10, 2)
```

위 요구사항을 확인하고 아래와 같이 DB설계 하였다.

```
create database shop; // shop이라는 데이터베이스를 만들어줘
use shop; // shop이라는 데이터베이스를 사용할거야.

create table users
(
	id int primary key auto_increment,
    name varchar(100),
    email varchar(100) unique,
    password varchar(100),
    registration_date date
);

create table products 
(
	id int primary key auto_increment,
    name varchar(100),
    category varchar(50),
    price decimal(10,2),
    stock int,
    rating decimal(3,2)
);

create table orders
(
	id int primary key auto_increment,
    user_id int,
    order_date date,
    total_amount decimal(10,2),
    foreign key (user_id) references users (id)
);

create table order_items
(
	id int primary key auto_increment,
    order_id int,
    product_id int,
    quantity int,
    price decimal(10,2),
    foreign key (order_id) references orders (id),
    foreign key (product_id) references products (id)
);

show tables; // 만들어진 테이블 내역이 보인다.
```

### 💡 SQL 연습

그럼 본격적으로 데이터를 다루기 위해 데이터를 생성해보자.

```
insert into users (name, email, password, registration_date) 
values ("Elie", 'aaaa@gmail.com', 'aaaa1234', '2024-10-01'),
("Jina", 'bbbb@gmail.com', 'aaaa1234', '2024-10-01'),
("Suzy", 'cccc@gmail.com', 'aaaa1234', '2024-10-02'),
("John", 'dddd@gmail.com', 'aaaa1234', '2024-10-03'),
("Ria", 'eeee@gmail.com', 'aaaa1234', '2024-10-01'),
("Cristal", 'ffff@gmail.com', 'aaaa1234', '2024-10-03');

select * from users;

insert into products (name, category, price, stock, rating) 
values('coke', 'drinks', 2000, 100, 4.5),
('juice', 'drinks', 3000, 50, 4.2),
('potatochips', 'snack', 1500, 70, 4.1),
('cornchips', 'snack', 1800, 90, 4.5),
('carrot', 'vegi', 500, 1000, 3.2),
('lettuce', 'vegi', 800, 30, 2.2);

select * from products;

insert into orders (user_id, order_date, total_amount)
values (1, '2024-10-03', 2.0),
(2, '2024-10-04', 2.0),
(3, '2024-10-05', 2.0),
(4, '2024-10-03', 3.0),
(5, '2024-10-04', 1.0),
(6, '2024-10-05', 1.0),
(1, '2024-10-06', 1.0),
(2, '2024-10-06', 1.0),
(3, '2024-10-06', 1.0);

select * from orders;

insert into order_items (order_id, product_id, quantity, price)
values (1, 1, 2, 4000),
(2, 2, 2, 6000),
(3, 3, 2, 3000),
(4, 1, 1, 2000),
(5, 4, 1, 800),
(6, 5, 3, 1500),
(1, 6, 1, 800),
(2, 1, 2, 4000),
(3, 4, 1, 1800);

select * from order_items;

```

생성된 데이터는 다음과 같이 확인된다.

일단 너무 연관관계를 신경쓰지 않고 데이터를 넣은 것 같은데.. 하면서 필요할 떄 데이터를 더 넣거나 수정하는 방향으로 하려고 한다..😂

![image](https://github.com/user-attachments/assets/ed11c146-5d3d-4ebe-bbd4-57fa2a84b49c)
![image](https://github.com/user-attachments/assets/8a71bba0-9f81-453b-96ef-2976e089260e)
![image](https://github.com/user-attachments/assets/2f907bb8-be26-48bd-bd67-1c36666214a7)
![image](https://github.com/user-attachments/assets/2ff36120-b74c-42e7-8196-6b58d08ac175)

### 💡 연습문제 (조회)

```
// 연습문제
1. 전체 사용자 조회
모든 사용자의 정보를 조회하세요.

2. 특정 열 조회
사용자 이름과 이메일만 조회하세요.

3. 구매 금액 기준 조회
특정 사용자(Jina)의 모든 주문을 조회하세요.

4. 주문 날짜 기준 정렬
모든 주문을 주문 날짜(order_date) 기준으로 오름차순 정렬하여 조회하세요.

5. 상품별 주문 수
각 상품별로 주문된 수량의 총합을 조회하세요. (JOIN과 GROUP BY 사용)

6. 특정 사용자 주문 내역
특정 사용자(Elie)의 주문 내역을 조회하세요.

8. 사용자별 총 구매 금액
각 사용자의 총 구매 금액을 계산하여 조회하세요. (JOIN과 GROUP BY 사용)
```

```
select * from users;

select u.name, u.email from users u;

select o.id order_id, o.order_date, o.total_amount
from orders o
join users u
on o.user_id = u.id
where u.name = "Jina";

select * from orders order by order_date asc;

select p.name, sum(oi.quantity) 
from order_items oi
join products p on oi.product_id = p.id
group by p.id;

select o.id order_id, o.order_date, o.total_amount
from orders o
join users u on o.user_id = u.id
where u.name = "Elie";

select u.name, sum(o.total_amount)
from users u
join orders o on u.id = o.user_id
group by u.id;
```

어렵당.. 딱 떠올라야 하는 것 같은데. 아직은 익숙하지 않은 것 같다.
QueryDSL을 사용하면서 어느정도 익숙해졌다고 생각했는데, 그게 아니었다.

생성/수정/삭제 구문으로 마지막정리!

```
// 생성
insert into 테이블이름 (컬럼1, 컬럼2, .. ) values (값1, 값2 ..)
// 수정 (where의 컬럼 값을 set구문의 내용으로 변경)
update 테이블이름 set 컬럼이름 = 변경할 값 where 컬럼이름 = "";
// 삭제 
delete from 테이블 이름 where 컬럼이름 = "";
```

조회부분만 더 연습을 해야할 것 같다.