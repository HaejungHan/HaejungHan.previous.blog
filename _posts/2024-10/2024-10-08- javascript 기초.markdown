---
layout: post
title:  "TIL(20241008) [Javascript 기초]"
date:  2024-10-08
categories: TIL JavaScript
---

----------------------------------------------------------------------------

## 📌 PL(Programming Language) 

1) Complier: 전체 소스 코드를 한 번에 읽어 기계어(바이너리 코드)로 변환합니다. (번역?)<br>
특징)
- 성능: 컴파일된 코드는 실행할 때 빠릅니다. 모든 코드가 미리 기계어로 변환되기 때문입니다.
- 오류 검사: 컴파일 시 모든 문법 오류를 찾아내기 때문에, 실행 전에 많은 오류를 잡을 수 있습니다.
- 결과물: 컴파일 후 실행 파일이 생성되며, 이는 다른 컴퓨터에서도 실행될 수 있습니다(호환성 고려).
- 예시: C, C++, Rust 등

2) Interpreter: 소스 코드를 한 줄씩 읽고 즉시 실행합니다. (해석?)<br>
특징)
- 즉각적인 실행: 코드 변경 후 즉시 결과를 볼 수 있어 개발 과정에서 유용합니다.
- 디버깅 용이: 실행 중 오류가 발생하면 해당 코드 줄에서 중단되므로, 오류를 쉽게 추적할 수 있습니다.
- 속도: 실행 속도가 느릴 수 있습니다. 매번 코드를 해석해야 하므로, 반복 실행 시 성능이 저하될 수 있습니다.
- 예시: Python, Ruby, JavaScript 등

3) 혼합형(Complier + Interpreter)
- 일부 언어는 두 가지 방식의 장점을 결합합니다. 예를 들어, Java는 소스 코드를 먼저 바이트코드로 컴파일한 후, JVM(Java Virtual Machine)에서 이 바이트코드를 인터프리트하여 실행합니다.

## 💡 DOM API (Document Object Model, Application Programming Interface)
- HTML 및 XML 문서의 구조화된 표현을 다루기 위한 프로그래밍 인터페이스입니다. **DOM은 문서의 각 요소를 객체로 표현**하며, JavaScript를 통해 웹 페이지의 내용과 구조를 동적으로 수정할 수 있게 해줍니다.
    
정리: DOM API는 웹 페이지의 구조와 내용을 프로그래밍적으로 조작할 수 있는 강력한 도구입니다. JavaScript와 함께 사용하여 동적인 웹 애플리케이션을 개발할 수 있게 해줍니다.


### 💡 기초 개념 알기

- var, let : 선언된 변수 재할당 가능, const : 상수 

#### 1. object

```javascript
const obj = { abc: 123 }; // {} 안 객체 리터럴 key-value 형태
```

**리터럴이란 ? 리터럴(literal)은 프로그래밍에서 직접적으로 값을 표현하는 고정된 데이터로 바로 사용되는 값입니다. 

```
const user = {
  nameis : 'Heropy object',
  age : 11,
  isVaild : true
}

console.log(user.nameis); // Heropy object
console.log(user.age); // 11
console.log(user.isVaild); // true
```

#### 2. array

```javascript

const fruits = ['Apple', 'Banana', 'Cherry'];

console.log(fruits[0]); // Apple
console.log(fruits[1]); // Banana
console.log(fruits[2]); // Cherry

for(var key in fruits) {
  console.log(key, fruits[key]); // 0 Apple 1 Banana 2 Cherry
}

```

### 3. function

#### 익명함수

```javascript
const hello = function helloFunc() {
  console.log('익명함수 확인');
}

hello(); // 익명함수 확인 변수로 호출
```

#### addEventListener

- addEventListener는 JavaScript에서 DOM 요소에 이벤트 리스너를 추가하는 메서드입니다. 이 메서드를 사용하면 특정 이벤트가 발생했을 때 실행될 함수를 지정할 수 있습니다.
- element.addEventListener(event, handler);
1) element: 이벤트를 감지할 DOM 요소입니다.
2) event: 감지할 이벤트의 종류를 문자열로 지정합니다. 예: 'click', 'mouseover', 'keyup' 등.
3) handler: 이벤트가 발생했을 때 실행될 함수입니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script src="./main.js"></script>
</head>
<body>
  <button id="clickButton">Click Me!</button> // 버튼 id 지정
</body>
</html>
```

```javascript
// HTML 버튼 요소를 선택
const button = document.getElementById('clickButton');

// 클릭 이벤트에 대한 리스너 추가
button.addEventListener('click', function() {
    console.log('버튼을 클릭!!!');
});
```

#### this

```javascript
const hero = {
  nickName : '영웅',
  age : 11,
  getName : function() {
    return this.nickName; //getName은 객체의 nickName을 리턴해라
  }
};

const result = hero.getName(); 
console.log(result); // 영웅 
```

#### getter, setter

```javascript
const cafeOrders = {
  menu : '아메리카노',
  quantity : 3,
  getMenu: function() {
    return this.menu;
  },
  setMenu: function(newMenu) {
    this.menu = newMenu;
  }
}

const resultSet = cafeOrders.getMenu();
console.log(resultSet); // 아메리카노

const changeMenu = cafeOrders.setMenu('카페모카');
console.log(cafeOrders.getMenu()); // 카페모카
```

#### class 태그의 경우? querySelector('.name');

```html

<body>
  <div class = "box"> BOX !?</div>
</body>

```

```javascript
const box = document.querySelector('.box'); 
```







