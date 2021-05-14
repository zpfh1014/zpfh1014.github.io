---
title: "생성자 함수 (The Constructor Function)와 프로토타입 체인(Prototype chain)"
categories:
  - javascript
tags:
  - Javascript
  - Constructor
  - Prototype chain
toc: true
toc_sticky: true
---

### 1. 생성자 함수 (The Constructor Function)
함수의 이름이 모두 대문자로 시작한다.   
자바스크립트에서 모든 함수는 객체이다. 그러므로 생성자 함수 또한 객체이다.  
자바스크립트에서 객체란 속성(key)과 값(value)를 가지는 존재이다. 즉, 생성자 함수 또한 객체이므로 속성과 값을 가진다.    

생성자 함수란 쉽게 말해서 <code>new 키워드</code>와 함께 쓰이는 함수이다.   
기본적으로 내장된 생성자 함수를 사용할 수도 있다.   

```javascript
new Array();
new Object();
new Function();
```

```javascript
var arr = [];
var arr = new Array();
```

두 코드는 모두 같은 동작을 하며, 모두 빈 배열을 만든다. 
그러나 아래의 코드는 자바스크립트의 기본 내장된 생성자 함수를 이용하여 배열을 만든 것이다.  

모든 생성자 함수에는 기본적으로 prototype이란 속성이 자동으로 생기는데,
콘솔창에 <code>Object.prototype</code> 이라고 타이핑하면 아래와 같이 확인 할 수 있다.

![images 1](/assets/images/javascript1_img_01.gif)