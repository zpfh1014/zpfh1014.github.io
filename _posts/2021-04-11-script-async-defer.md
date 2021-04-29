---
title: "async 와 defer의 차이"
categories:
  - HTML
tags:
  - html
  - async
  - defer
  - study
---

브라우저는 html 파일을 한줄씩 파싱한 후 렌더링하는 과정을 거쳐 화면을 사용자에게 보여줍니다. 이때 html 파일에서 외부 script 파일을 연결하는 경우 <code>&lt;script&gt;</code> 를 사용하게 되는데, <code>&lt;script&gt;</code>에는 2가지 속성 **async, defer** 가 있습니다. 우리가 html 파일안에서 <code>&lt;script&gt;</code>를 작성하는 경우 그 위치와 속성에 따라 브라우저에서 다르게 동작이 이루어지게 되는데 각각의 경우에 따라 발생할 수 있는 장단점들을 알아보겠습니다.

## 1. <code>&lt;head&gt;</code> 안에 <code>&lt;script&gt;</code>가 위치하는 경우
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="test.js"></script>
</head>
<body></body>
</html>
```
브라우저는 파싱하는 과정 중 <code>&lt;script&gt;</code> 를 만나게 되면 파싱하는 것을 멈추고 파일을 서버에서 다운받아 실행 한 후 다시 파싱하는 과정을 가집니다. 그로인해 렌더링 시간을 길어지고 사용자는 제대로 된 화면을 보지 못한채 파일을 다운 받고 실행하는 시간 동안을 기다려야 됩니다.


## 2. <code>&lt;body&gt;</code> 안 제일 끝부분에 <code>&lt;script&gt;</code>가 위치하는 경우
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div></div>
    <script src="test.js"></script>
</body>
</html>
```
이 경우에는 모든 html 파싱을 마친 후 <code>&lt;script&gt;</code>를 만나 script를 실행하기 때문에 렌더링 시간이 짧아지게 되고 기본적인 화면을 사용자에게 먼저 제공할 수 있습니다. 또한 DOM 요소가 생성되지 않은 상태에서 script가 실행 되는 경우 요소를 탐색하지 못하게 되는데 끝에 작성할 경우에는 DOM 요소가 생성 된 후에 script를 실행하기 때문에 DOM 요소에 접근할 수 있게 됩니다. 이러한 이유로 <code>&lt;body&gt;</code> 안 제일 끝부분에 <code>&lt;script&gt;</code> 작성하는 것을 추천합니다. 하지만 이경우에도 script에 의존적인 사이트(데이터 통신, DOM 요소를 변경하는 경우)에서는 사용자가 잘못된 컨텐츠를 보게 될 수 있다는 문제점이 존재 할 수 있습니다.

## 3. <code>&lt;head&gt;</code> 안에 <code>&lt;script&gt;</code>가 위치하고 async 속성을 하는 경우

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script async src="test.js"></script>
</head>
<body></body>
</html>
```
async는 브라우저가 파싱하는 도중 <code>&lt;script&gt;</code>를 만나게 되었을 때 파싱하는 것을 멈추지 않고 파일을 다운받기 시작합니다. 이후 다운로드가 완료 되면 파싱을 멈추고 script를 실행하게 됩니다. 파일을 다운로드 받는 동안 파싱이 계속 이루어 지기 때문에 렌더링 시간은 줄어들겠지만 script가 실행 될 때 DOM 요소가 생성 되어 있지 않을 경우 오류가 발생하게 될 수 있습니다. 또한 async는 다운로드 된 순서대로 파일을 실행하기 때문에 순서에 의존적일 경우에는 적합하지 않습니다.

## 4. <code>&lt;head&gt;</code> 안에 <code>&lt;script&gt;</code>가 위치하고 defer 속성을 하는 경우

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script defer src="test.js"></script>
</head>
<body></body>
</html>
```
defer는 async와 마찬가지로 브라우저가 파싱하는 도중 <code>&lt;script&gt;</code>를 만나게 되었을 때 파싱하는 것을 멈추지 않고 파일을 다운받기 시작한 후 파싱을 끝까지 실행합니다. 모든 파싱을 마친 후 다운로드 받은 script 파일을 실행합니다. 이로인해 렌더링 시간을 가장 단축 시킬 수 있어 이 <code>&lt;script&gt;</code>에 defer 속성을 사용하는 것을 가장 추천하지만 IE 하위 브라우저에서는 이 속성을 지원하지 않는 경우가 있어 하위브라우저를 지원해야 되는 경우에는 2번째 경우를 사용하시는 것을 추천드립니다.

Reference
[드림코딩 - 자바스크립트 기초 강의](https://www.youtube.com/watch?v=tJieVCgGzhs&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=2)