---
layout: post
title: "[Java Script, HTML] HTML DOM Video Object 생성, 해제"
subtitle: ""
categories: [Study/Nodejs]
tags: [js, javascript, HTML]
comments: true
---

## Video 객체 생성, 해제

---

웹과 데스크탑 어플리케이션에서 동시에 웹카메라를 사용해야 하는 경우가 생겼다. 웹에서도 HTML의 Video Object를 사용하고 데스크탑 어플리케이션에서도 HTML의 Video Object를 사용하기 때문에 한 군데에서 웹 카메라를 사용하고 있는 경우 다른 곳에서 웹 카메라 사용이 불가능하다. 프로그램을 종료해야하는 번거로움을 없애기 위해서 데스크탑 어플리케이션에서 비디오 객체를 생성/ 해제하는 기능을 추가했다. 단순히 `pause()`를 이용해 비디오 출력을 중지하면 여전히 웹에선 데스크탑 어플리케이션에서 웹 카메라를 사용하고 있다고 인식하기 때문에 video 객체의 모든 요소를 정지해야 한다. 

<br>

## Video 객체 생성

```js
 
  this.video = document.createElement("video");
  this.video.width = 480;
  this.video.height = 360;
  this.video.autoplay = true;
  this.video.style.display = "none";

  const media = navigator.mediaDevices.getUserMedia({
    video: true,
    audio: false,
    });

  media.then((stream) => {
    this.video.srcObject = stream;
    });

```

`createElement("video")`를 호출해 HTML 비디오 객체를 생성한다. `getUserMedia()`로 받아온 사용자의 비디오`stream`를 `this.video.srcObject`에 할당한다.

## Video 객체 해제

```js

this.video.pause();
this.video.srcObject.getTracks()[0].stop();
this.video.srcObject = null;

```

`MediaStreamTrack`의 `stop()`을 호출하여 비디오 스트림을 정지한다. `this.video.srcObject`를 `null`로 설정하여 객체를 해제한다. 

<br>

## Reference

- [mozilla](https://developer.mozilla.org/ko/docs/Web/API/MediaStreamTrack/stop)
- [stack overflow](https://stackoverflow.com/questions/11642926/stop-close-webcam-stream-which-is-opened-by-navigator-mediadevices-getusermedia)
