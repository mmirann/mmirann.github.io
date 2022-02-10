---
layout: post
title: "[Node.js] Socket.IO 에러 해결법"
subtitle: ""
categories: [Study/Nodejs]
tags: [Nodejs]
comments: true
---

## TypeError: io is not a function
```js
const io = require("socket.io-client");
```
sockeit.io-client package를 사용하려고 하니 `Uncaught TypeError: io is not a function` 에러가 떴다. 테스트 코드로 돌렸을 땐 위와 같이 사용해도 잘 작동했는데, 개발 중인 프로그램에 적용하니 같은 코드인데도 작동이 안됐다. 디버깅하면서 type을 확인해보니까 `io`가 `function`이 아닌 `object`로 인식이 되고 있었다. 따라서 다음과 같이 사용하니 에러가 해결되었다.

```js
const { io } = require("socket.io-client");
```

또는 명시적으로 `function`을 호출한다.

```js
const io = require("socket.io-client").io;
```


<BR>

## 'Access-Control-Allow-Origin' error
```
Access to XMLHttpRequest at 'http://127.0.0.1:20111/socket.io/?EIO=4&transport=polling&t=NxXYy3X' from origin 'http://127.0.0.1:8601' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```
통신 환경을 구축하고, 간단한 메세지를 전송하려고 하니 위와 같은 에러가 떴다. 구글링해보니 **SOP(Same-Origin Policy)** 라고 동일한 port, http, host의 자원에만 접근을 허용하기 때문에 **CORS(Cross-Origin Resource Sharing)** policy가 작동하여 지정된 도메인 외부에 있는 자원에 대한 접근을 통제하는 브라우저 메커니즘이 작동한 것으로 보였다. 기본 `transports`는 모든 서버에서의 접근을 허용하지 않으므로 다음과 같이 `transports`를 설정하니 에러가 해결되었다.
```js
// client-side
var socket = io('http://localhost:<your_port_number>', 
                {transports: ['websocket', 'polling']});
```
하지만 위와 같이 설정하면 모든 서버로부터 접근이 허용되므로 안전하지 않다. 특정 서버의 접근만 허용하기 위해선 다음과같이 접근을 허용할 주소를 cors에 넣어 활성화한다.
```js
//server-side
const io = require("socket.io")(httpServer, {
  cors: {
    origin: "https://example.com",
    methods: ["GET", "POST"]
  }
});
```
<br>

## Reference
* <https://stackoverflow.com/questions/44628363/socket-io-access-control-allow-origin-error/64805972>
* <https://socket.io/docs/v4/client-initialization/>
* <https://socket.io/docs/v3/handling-cors/>
