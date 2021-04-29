---
title: "이벤트 루프 알아보기"
categories:
  - Javascript
tags:
  - Javascript
  - Study
---

자바스크립트의 **이벤트 루프(Event loop)**란 개념에 대해 알아 보겠습니다.

먼저 자바스크립트는 단일 스레드 언어입니다. 단일 스레드라는 말은 여러개의 요청을 받았을 때 동시에 여러개의 요청을 처리하는 것이 아닌

요청을 하나씩만 처리 하는것을 말합니다. 하지만 실제 자바스크립트가 실행되는 환경을 확인해보면 동시에 여러 작업을 수행하고 있는 것을 볼 수 있는데요.

이렇게 단일 스레드 언어인 자바스크립트가 동시성을 가지게 해주는 과정 속에 바로 이벤트 루프라는 개념이 들어 있습니다.

이벤트 루프를 이해하기 전에 먼저 **호출스택(Call Stack)**에 대해 알아야 합니다.

호출 스택은 실행 컨텍스트와 비슷한 형태를 가지고 있습니다.

```javascript
function first(){
    second();
    console.log("1");
};

function second(){
    third();
    console.log("2");
};

function third(){
    console.log("3");
};

first();
```

위 코드를 보면

1. 함수 first 를 호출
2. 함수 first 안의 함수 second 호출
3. 함수 second 안의 함수 third 호출

![이벤트 루프1](/assets/images/eventloop_img01.png)

호출된 순서에 따라 아래부터 호출스택에 쌓이게 되고 모든 함수가 호출 되고 난 후 위에서 부터 처리하게 됩니다.

![이벤트 루프2](/assets/images/eventloop_img02.png)

여기서 자바스크립트는 자바스크립트 엔진에 의해 호출스택에 하나의 스택만을 쌓아 처리하고 처리 하는 과정동안 다른 어떠한 함수도 실행 할 수 없습니다. 이러한것을 Blocking 이라고 부릅니다. Blocking 된 상태에서는 다른 함수를 실행할 수 없을 뿐만 아니라 렌더링도 진행되지 않습니다.

동시에 여러 요청을 처리할 수 있도록 해주는 역활은 브라우저 환경에서 이루어 집니다. 브라우저가 가지고 있는 Web API(DOM Events, AJAX, setTimeout()) 를 통해 동시 처리가 가능해 집니다.

```javascript
console.log("시작");

function run(){
    console.log("run");
};

setTimeout(run, 1000);
console.log("끝");
```

위 코드를 실행하면 시작, 끝이 출력되고 난 후 1초 후에 run 이 출력 됩니다.

여기서 의문이 드실텐데 자바스크립트는 스택을 하나씩 순차적으로 처리하는데 setTimeout이 처리 되기 전에 끝이 먼저 출력 됩니다.

이 과정이 이벤트 루프가 이루어지는 과정인데요.

1. 호출스택에 console.log("시작") 이 쌓였다 실행되어 사라지고

2. setTimeout(run,1000) 이 호출스택에 쌓였다 1초 후 실행 되고나서 사라진다고 생각할 수 있지만 호출스택에 들어와 Web API 에 의해 타이머가 실행되고 setTimeout 은 스택에서 바로 사라진 후 백그라운드 환경에서 실행 됩니다. 실행이 완료된 결과는 태스크 큐로 들어가게 됩니다.

3. 그 사이 빈 호출스택에 console.log("끝") 이 들어와 실행되어 사라지고

4. 태스크 큐에서 대기하던 Callback(setTimeout 실행값) 이 빈 호출스택에 들어와 실행 됩니다. 바로 이 4번째 과정을 이벤트 루프 라고 합니다.

여기서 setTimeout의 실행된 결과가 대기하는 곳을 **태스크 큐(Task Queue)**라고 부르며,

호출스택이 비어질때마다 태스큐 큐에 있는 콜백 함수를 호출스택으로 이동시켜 실행하는것을 **이벤트 루프(Event loop)**라고 합니다.

[http://meetup.toast.com/posts/89](http://meetup.toast.com/posts/89)
[제로초님 이벤트 루트 관련 글](https://www.zerocho.com/category/JavaScript/post/597f34bbb428530018e8e6e2)
[이벤트 루프 직접 테스트 하기](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)