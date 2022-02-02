---
layout: post
title: "[Node.js, Arduino] Node.js CRC-16 계산, 하드웨어 CRC-16 에러 검증"
subtitle: ""
categories: [Study/Nodejs]
tags: [Arduino,Nodejs]
comments: true
---

## 순환 중복 검사 (CRC, Cyclic Redundancy Check)
데이터 전송할 때 데이터에 오류가 있는지 확인하기 위한 오류 검증 기법이다. 송신 측에선 CRC 값을 데이터에 붙인 코드 워드를 전송하고, 수신 측에선 수신된 코드워드에서 CRC 값을 이용해 에러를 발견한다. 수학적 연산 과정은 복잡하지만, CRC 패키지를 활용하면 간단한다.  

 CRC는 CRC-16과 CRC-32 두 종류가 많이 사용되는데 원칩 마이크로 프로세서와 같이 간단한 용도에선 CRC-16이 사용되고, 정확한 에러 검증에는 CRC0-32가 사용된다. 아두이노, ESP32와 같은 하드웨어와의 통신을 목적으로 사용할 것이기 때문에 CRC-16을 사용하도록 한다. 

<br>

## Node.js CRC-16 계산

우선 `crc-full` 패키지를 설치한다.
```
npm install crc-full
```

CRC를 계산하는 방법은 아래와 같다. `dataArr`의 송신하고자 하는 데이터 배열에 Start Flag(255) 및 길이를 붙인 배열을 만들어 CRC 값을 계산한 후 코드워드로 만든다. `crc.compute(Array)`함수를 이용하면 계산된 crc 값이 나오고, 시리얼 통신은 바이트 단위로 이루어지므로 비트연산을 이용해 바이트 단위로 나눈다.
```js

const CRC = require('crc-full').CRC;
let crc = new CRC("CRC16_XMODEM", 16, 0x1021, 0x0000, 0x0000, false, false);

sendWsData(datArr){
    let sendArr = [255, 255, dataArr.length];
    sendArr = sendArr.concat(dataArr);
    // crc 계산 
    let computed_crc = crc.compute(sendArr);
    let crcArr = [computed_crc >> 8, computed_crc & 0xFF];
    sendArr = sendArr.concat(crcArr); // 최종 코드워드
    this._serialport.write(sendArr); // 전송 
}
```
<br>

## 하드웨어 CRC-16 에러 검증
하드웨어에서 수신한 코드워드를 수신한 CRC 값과, 계산한 CRC과 비교를 통해 에러를 검증한다. 우선 코드워드에 첨부된 CRC값을 제외한 에러를 검사할 데이터 부분을 `calcrc`함수를 통해 CRC 값을 계산한다. 

```c
uint16_t crc;

/**
* ... 데이터 수신 
*/

crc = calcrc(receivedArr, arrLength);

uint16_t calcrc(uint8_t *ptr, int count)
{
  uint16_t crc;
  uint8_t i;
  crc = 0;
  while (--count >= 0)
  {
    crc = crc ^ (int)*ptr++ << 8;
    i = 8;
    do
    {
      if (crc & 0x8000)
        crc = crc << 1 ^ 0x1021;
      else
        crc = crc << 1;
    } while (--i);
  }
  return (crc);
}

```

계산하여 나온 CRC 값과 코드워드에 같이 첨부되어 온 CRC 값이 일치하는지 검사한다. 값이 일치하면 데이터에 이상이 없는 것이고, 일치하지 않으면 에러가 발생한 것이다. 
```c
uint16_t crcRx;

 crcRx = (CRCH << 8) | CRCL; //수신한 CRC 값 

 if (crcRx == crc){
     // 데이터 파싱 
 }else{
     // 데이터 에러 
 }

```

<br>

## Reference
* <https://www.npmjs.com/package/crc-full>