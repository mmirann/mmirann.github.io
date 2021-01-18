---
layout: post
title: "[Arduino] Hex to Binary"
subtitle: ""
categories: [IoT/Arduino]
tags: [IoT,Arduino]
comments: true
---

> 16진수를 2진수로 바꾸기

***

Entry 블록 중에 도트 매트릭스 센서를 구현을 위해선 16진수를 2진수로 변환해야한다. 사용자가 도트 매트릭스의 행마다 블록에 바이너리 값을 입력하는 건 불편하다고 느낄 것이기 때문에, 16진수 한줄을 입력하면 펌웨어에서 2진수로 변경하여 
매트릭스에 출력하는 방식을 생각했다. 도트매트릭스 변환 사이트는 [여기](https://xantorohara.github.io/led-matrix-editor/#1824428181815a24)를 이용하였다. 16진수와 2진수 결과 값 모두를 보여주어 참고하기에 좋다. 

***
처음엔 스위치 문을 이용해 케이스를 나누는 방법을 생각해보았다. 
```c
switch (hexdec[i]) {
      case '0':
        if (i % 2 == 0) { //짝수인 경우
          m[index] = B0000;
        } else { //홀수인 경우(붙이기)
          m[index] = m[index] << 4;
          m[index] += B0000;
        }
        break;
```
이런 식으로 16가지 케이스를 나누어 shift연산으로 자리수를 옮기고 덧붙이는 방법을 생각했었는데 shift연산을 사용하면 더 쉽게 해결될 문제였다. 

```c
  uint64_t image = 0xff001c183ce39dff;

  for (int i = 0; i < 8; i++) {
    byte row = (image >> i * 8)&0xFF;
    for (int j = 0; j < 8; j++)
    {
      lcjikko->setLed(0, i, j, bitRead(row, j));
    }
  }
```

`uint64_t`는 자료형이 양수인 64비트를 뜻한다. `0x`가 앞에 붙으면 16진수를 뜻한다. 참고로 `B`가 앞에 붙으면 2진수이다. `byte`는 0-255의 8비트 부호없는 숫자값을 나타내는 자료형이다.


따라서 `byte row=(image >> i *8);`에서는 8자리씩 비트 shift 연산을 통해 row에 저장되며, row의 크기는 8비트이기 때문에 도트 매트릭스에 한 줄씩 출력할 수 있는 것이다. 잘 잘렸는지 확인하고 싶으면 `Serial.print(row,BIN)`을 통해 확인할 수 있다. `&0xFF`연산을 하는 이유는 뒤의 8비트만 남기기 위함인데 이미 byte의 자료형 때문에 8비트만 짤리기 때문에 없어도 문제는 없다. `row`의 자료형이 `int`라면 `&0xFF`연산이 필요하다. 