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

![images 1](/assets/images/javascript/01/img_1.gif)

Object.prototype의 값으로 리턴된 객체에는 다시 constructor라는 속성이 들어있다.

![images 2](/assets/images/javascript/01/img_2.gif)

constructor와 prototype은 한 쌍을 이루며 서로가 서로의 정보를 가지고 있다.

### 2. 생성자 함수의 인스턴스 (Instance)

```javascript
var obj = new Object();
```

모든 생성자 함수는 this라는 빈 객체를 리턴한다.     
obj 변수에는 빈 객체가 할당된다. 이 때 생성자 함수에서 리턴하는 결과물을 우리는 <strong>인스턴스(instance)</strong>라고 한다.    
인스턴스는 constructor와 prototype 사이에서 생성된 자식과도 같은 존재이다.

### 3. 프로토타입 체인 (Prototype Chain)

```javascript
var obj = new Object();

console.log(obj.constuctor);
```
obj라는 인스턴스를 생성하고, obj.constructor에 접근하면   
Object.prototype 이 constructor 의 정보를 가지고 있다.

![images 3-1](/assets/images/javascript/01/img_3-1.gif)

Object.prototype 에 type 추가하면 obj.type 로 값을 가져올 수 있다.

![images 3-2](/assets/images/javascript/01/img_3-2.gif)

obj.type 에 1-1 라는 값을 부여하면 Object.prototype 에서 값을 가져오지 않고 obj에 부여된 속성값을 가져온다.
이런 특성이 Prototype Chain 이다.


![images 4](/assets/images/javascript/01/img_4.gif)

obj라는 인스턴스가 부모 프로토타입에 접근하여 정보를 가져다 쓸 수 있다는 특성을 상속이라는 표현보다는 엄밀히 말하면 프로토타입 체인은 상속이 아니라 위임에 가깝다.


```javascript
var arr = new Array();

arr.push(3);
```

arr이라는 인스턴스에서 push 메소드를 사용할 수 있는 이유는 상위 프로토타입인 Array.prototype이 push 메소드를 가지고 있고, 연산을 수행할 수 있는 것이다.

![images 5](/assets/images/javascript/01/img_5.gif)



### 4. Dunder Proto ( __proto__ )

[__proto__](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)


```javascript
function Foo(){}

Foo.prototype.proto_val = 100;    //프로토타입 속성을 설정합니다.

var foo_instance = new Foo();  //Foo의 인스턴스를 생성합니다.

console.log(foo_instance.proto_val); //프로토타입값 100을 출력을 합니다.
console.log(foo_instance.hasOwnProperty('proto_val'));// 인스턴스 내부에 proto_val이라는 속성을 가지고 있는지 확인 prototype체인 순회를 안하는것이 특징
//----------------------------------
foo_instance.proto_val -= 1;  // 인스턴스에서 proto_val을 1줄여봅니다. 
//위표현식은
foo_instance.proto_val = foo_instance.proto_val - 1;// 과 같음
//----------------------------------
console.log(foo_instance.hasOwnProperty('proto_val')); // true 인스턴스 내부에 proto_val이라는 프로퍼티가 할당되었기떄문에 더 이상 프로토타입 체인을 순회하지않습니다.
console.log(foo_instance.proto_val); // 98
console.log(foo_instance.__proto__.proto_val);//프로토 타입 프로퍼티는 존재하지만 foo_instance 내부에 proto_val이라는 동일이름의 프로퍼티가 존재하기때문에 인스턴스 변수로 호출시 인스턴스의 프로퍼티값을 가져옵니다.

// delete 연산자를 이용해서 객체에 foo_instance.proto_val이라는 프로퍼티를 제거 인스턴스 내부에 proto_val이라는 프로퍼티를 가지고있는지 확인
delete foo_instance.proto_val;
console.log(foo_instance.hasOwnProperty('proto_val')); //false 인스턴스내부에 프로퍼티가 지워진걸 알 수 있습니다.
console.log(--foo_instance.proto_val);  // 전위 감소 연산자 사용
console.log(foo_instance.__proto__.proto_val); // 결과값 100
console.log(foo_instance.proto_val); // 99
console.log(foo_instance.hasOwnProperty('proto_val')); //true 전위 연산자를 통해서 다시 proto_val이라는 프로퍼티가 인스턴스에 할당됨.
```