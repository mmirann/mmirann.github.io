---
layout: post
title: "[JS] ES6 문법 정리: 화살표 함수(Arrows)"
subtitle: ""
categories: [Study/Nodejs]
tags: [javascript]
comments: true
---

## 화살표 함수

---

화살표 함수를 사용하면 `function` 키워드 대신 `=>`를 사용해 간단하게 함수를 선언할 수 있다. ES6 이전엔 아래 코드와 같이 함수를 표현했다.

```js
//함수 선언
function foo() {
  console.log("foo 호출");
}

//함수 표현식
const boo = function () {
  console.log("boo 호출");
}; //익명 함수를 변수 foo에 참조
```

함수 선언과 함수 표현식의 차이는 함수 선언으로 선언된 foo 함수는 스코프의 최상단으로 호스팅되어 코드의 처음에 `foo()`로 호출이 가능하다. 하지만 함수 표현식으로 선언된 boo 함수는 함수 표현식을 선언하기 전에는 함수를 호출할 수 없기 때문에 코드의 처음에 `boo()`로 boo 함수 호출이 불가능하다.

<br>

## 화살표 함수

- function 키워드를 생략할 수 있음
- 함수의 매개변수가 1개라면 괄호를 생략할 수 있음
- 매개 변수가 없는 함수는 괄호가 필요함
- 함수 바디가 표현식 하나라면 중괄호와 return 문을 생략할 수 있음

<br>

## 제한 사항

- `this`나 `super`에 대한 바인딩이 없고, `methods`로 사용될 수 없음
- `new.target` 키워드가 없음
- 일반적으로 스코프를 지정할 때 사용하는 `call`, `apply`, `bind` 메소드를 이용할 수 없음
- 생성자로 사용할 수 없음
- `yield`를 화살표 함수 내부에서 사용할 수 없음

<br>

## 화살표 함수 선언

```js
  //매개변수 지정
  //1. 매개변수가 없는 경우
  () => { ... }
  //2. 매개변수가 한개인 경우 소괄호 생략 가능
  x => { ... }
  //3. 매개 변수가 여러 개인 경우 소괄호 생략 불가
  (x,y) => { ... }

  //함수 몸체 지정

  //한줄의 구문인 경우
  x => { return x * x }
  //한줄의 구문인 경우 중괄호를 생략할 수 있고 암묵적으로 return 됨
  x => x * x

  //여러줄 구문인 경우
  () => {
    const x = 10;
    return x * x;
  }
```

<br>

## 화살표 함수 사용

```js
var evens = [2, 4, 6, 8];

// Expression bodies: 표현식의 결과가 반환
var odds = evens.map((v) => v + 1); // [3,5,7,9]
var nums = evens.map((v, i) => v + i); //[2,5,8,11]
var pairs = evens.map((v) => ({ even: v, dd: v + 1 })); //[{even:2, odd:3}, ...]

// Statement bodies: 블럭 내부 실행만 함, 반환을 위해 return 명시
nums.forEach((v) => {
  if (v % 5 === 0) fives.push(v);
});

//Lexical this
var bob = {
  _name: "Bob",
  _friends: ["John, Brian"],
  printFirends() {
    this._firends.forEach((f) => console.log(this._name + "knows " + f));
  },
}; //Bob knows John, Brian
```

<br>

## Reference

- [mozilla](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- <https://jsdev.kr/t/es6/2944>
