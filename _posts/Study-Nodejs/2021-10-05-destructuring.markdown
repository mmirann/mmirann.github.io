---
layout: post
title: "[JS] ES6 문법 정리: 구조 분해 할당(Destructuring Assignment)"
subtitle: ""
categories: [Study/Nodejs]
tags: [js, javascript]
comments: true
---

## Destructuring Assignment

---

구조 분해 할당 구문은 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 표현식이다. 배열 또는 객체 리터럴에서 필요한 값만 추출해 변수에 할당하거나 반환할 때 유용하다.

<br>

## 배열 구조 분해

```js
// 기본 변수 할당
var foo = ["one", "two", "three"];
var [red, yellow, green] = foo;
console.log(red); //one
console.log(green); //three

// 기본값
// 변수에 기본 값을 할당하면 분해한 값이 undefined일 때 그 값을 대신 사용함
var a, b;
[a = 5, b = 7] = [1];
console.log(a); //1
console.log(b); //7
```

<br>

## 객체 구조 분해

```js
// 기본할당
var o = { p: 42, q: true };
var { p, q } = o;
console.log(p); //42
console.log(q); // true

//중첩된 객체 및 배열의 구조 분해
var metadata = {
  title: "Scratchpad",
  translations:[
    {
      locale: "de",
      localization_tags: [ ],
      last_edit: "2014-04-14T08:43:37",
      url: "/de/docs/Tools/Scratchpad",
      title: "JavaScript-Umgebung"
    }
    url: "/en-US/docs/Tools/Scratchpad"
  ]
};

var { title: englishTitle, translations: [{ title: localeTitle }] } = metadata;

console.log(englishTitle); // "ScratchPad"
console.log(localeTitle); // "JavaScript-Umgebung"
```

위와 같이 구조 분해 문법을 사용하면 객체에서 필요한 정보만 보다 간결하게 가져올 수 있다.
<br>

## Reference

- [mozilla](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
