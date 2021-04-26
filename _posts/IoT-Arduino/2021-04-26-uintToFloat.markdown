---
layout: post
title: "[Arduino] Uint8Array를 float으로, float을 uint8_t로 변환"
subtitle: ""
categories: [IoT/Arduino]
tags: [arduino]
comments: true
---

## Arduino, node.js

DHT센서의 결과 값의 자료형은 `float`인데, 블루투스 송수신을 위한 `BLECharacteristic` 라이브러리의 `setValue`는 `uint_8_t`만 지원한다. 따라서 보드에서 블루투스를 통해 스크래치 인터페이스(`node.js`기반)로 받아오기 위해선 전송시에 `uint8_t`로 변환해야한다. 또한 스크래치에서 지원하는 블루투스 모듈 `BLE`를 통해 `Uint8Array`로 읽어온 것을 다시 `float`으로 변환하는 코드를 다뤄보도록 하겠다.

- uint8_t 크기: 1 Byte (0-255)
- float 크기: 4 Byte

## Arduino에서 float -> uint8_t

아두이노에서 float을 uint8_t으로 변환하는 방법은 두가지가 있다. 첫번째 방법은 비트 연산을 이용하는 것이고, 두번째 방법은 union을 이용하는 것이다.

## 1. 비트 연산

```c
   float data32;
   uint8_t temp[4];
	temp[0] = data32;
	temp[1] = data32 >> 8;
	temp[2] = data32 >> 16;
	temp[3] = data32 >> 24;
```

4비트씩 밀면서 `uint8_t` 배열에 저장하는 간단한 방법이다.

## 2. Union 이용

```c
   union
   {
      uint8_t intVal[4];
      float floatVal;
   } val;
```

위와 같이 `union`을 선언한다.

```c

   float data32;
   val.floatVal=data32;
   memcpy(&value_sensor_all_data[3], val.intVal, 4);

```

그리고 위와 같이 `union`을 이용하여 `uint8_t` 자료형의 `intVal` 배열로 할당된 `data32`를 `uint8_t` 배열인 `value_sensor_all_data`에 4바이트 복사해주었다.

<br>

## node.js에서 uint8Array -> float

node.js에서 `uint8Array`를 `float`으로 변환하기 위해선 `typescript`의 모듈인 `Dataview`를 사용한다.

```js
   float float_value;
   const data = Base64Util.base64ToUint8Array(base64);
   const buffer = new Uint8Array([data[3], data[4], data[5], data[6]])
            .buffer;
   const view = new DataView(buffer);
   float_value = view.getFloat32(0, true).toFixed(2);
```

위와 같이 `uint8Array` 자료형의 `data`의 변환하고자 하는 4바이트를 `buffer`에 읽어온다. `DataView`의 매개변수는 `buffer`만 올 수 있기 때문에 이와 같이 버퍼로 만드는 과정이 필요하다. 그리고 만든 버퍼를 매개변수로 가지는 `DataView`를 생성한다. 그리고 `getFolat32`를 이용해 float 변수에 할당한다. 첫번째 매개변수는 `byteOffset`, 두번째 매개변수는 `LittleEndian`으로 읽어오겠다는 뜻이다. 소숫점 둘째자리까지 읽어오기 위해 `toFixed(2)`를 사용하였다.

### Reference

-<https://github.com/nkolban/esp32-snippets/blob/master/cpp_utils/BLECharacteristic.cpp> -<https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/DataView/DataView>
