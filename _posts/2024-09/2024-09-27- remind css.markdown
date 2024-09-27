---
layout: post
title:  "TIL(20240927) [다시 CSS 기초]"
date:  2024-09-27
categories: TIL
---

----------------------------------------------------------------------------

https://github.com/user-attachments/assets/3b2f0ba6-a141-401a-860a-705556f8e1a9

## 💡 CSS 기초  

### 🚩 . / # 차이
- (.) : 클래스 선택자
- (#) : id 선택자

### 🚩 유용한 단축키
- 한줄 복사 : shift + alt + 화살표 위,아래
- 한줄 이동 : alt + 화살표 위,아래
- 한줄 삭제 : ctrl + shift + K

### 🚩 유용한 태그
- ex) .doors>.container

```
<body>
<div class="doors">
  <div class="container"></div>
</div>
</body>
```

```
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Select Doors!</title>
  <link href="https://cdn.jsdelivr.net/npm/reset-css@5.0.2/reset.min.css" rel="stylesheet">
  <link href="./main.css" rel="stylesheet">
</head>
<body>

<div class="doors">
  <div class="container"></div>
</div>
<div class="container">
  <div class="doors">
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
    <div class="door">
      <div class="image"></div>
    </div>
  </div>
  <h1 class="title"> Doors. </h1>
</div>
</body>
</html>
```

```
body {
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/bg.jpg");
  background-size: cover;
  background-repeat: no-repeat;
}

.container {
  padding: 50px 0;
}

.doors{
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  max-width: 660px;
  /* background-color: orange; */
  margin: 0 auto;
  padding: 40px 20px;
}

.door{
  width: 80px;
  height: 84px;
  margin: 4px;
  background-color: #555;
  border: 3px solid #fff;
  transform: skewX(-14deg);
  border-radius: 10px;
  box-sizing: border-box;
  transition: transform .1s, background-color .6s;
  overflow: hidden;
}

.door:hover {
  background-color: orange;
  transform: skewX(-14deg) scale(1.3);
  z-index: 1;
}

.door .image{
  width: 130%;
  height: 100%;
  background-size: cover;
  transform: translateX(-11px) skewX(14deg);
  background-position: center;
}

.door:nth-child(1) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img1.jpg");
}
.door:nth-child(2) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img2.jpg");
}
.door:nth-child(3) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img3.jpg");
}
.door:nth-child(4) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img4.jpg");
}
.door:nth-child(5) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img5.jpg");
}
.door:nth-child(6) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img6.jpg");
}
.door:nth-child(7) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img7.jpg");
}
.door:nth-child(8) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img8.jpg");
}
.door:nth-child(9) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img9.jpg");
}
.door:nth-child(10) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img10.jpg");
}
.door:nth-child(11) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img11.jpg");
}
.door:nth-child(12) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img12.jpg");
}
.door:nth-child(13) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img13.jpg");
}
.door:nth-child(14) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img14.jpg");
}
.door:nth-child(15) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img15.jpg");
}
.door:nth-child(16) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img16.jpg");
}
.door:nth-child(17) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img17.jpg");
}
.door:nth-child(18) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img18.jpg");
}
.door:nth-child(19) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img19.jpg");
}
.door:nth-child(20) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img20.jpg");
}
.door:nth-child(21) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img21.jpg");
}
.door:nth-child(22) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img22.jpg");
}
.door:nth-child(23) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img23.jpg");
}
.door:nth-child(24) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img24.jpg");
}
.door:nth-child(25) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img25.jpg");
}
.door:nth-child(26) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img26.jpg");
}
.door:nth-child(27) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img27.jpg");
}
.door:nth-child(28) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img28.jpg");
}
.door:nth-child(29) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img29.jpg");
}
.door:nth-child(30) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img30.jpg");
}
.door:nth-child(31) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img31.jpg");
}
.door:nth-child(32) .image{
  background-image: url("https://raw.githubusercontent.com/ParkYoungWoong/door-selector/main/images/img32.jpg");
}

h1.title {
  font-size: 30px;
  font-weight: bold;
  color: #333;
  text-align: center;
}
```

클론 코딩이지만 오랫만에 해보니 감회가 새롭다.. 

