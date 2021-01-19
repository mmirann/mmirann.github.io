---
layout: post
title: "[Arduino] Arduino port"
subtitle: ""
categories: [IoT/Arduino]
tags: [IoT, Arduino]
comments: true
---

> 아두이노 포트별 역할

---

아두이노 커스텀 쉴드에 탑재되는 펌웨어의 코드 중에서 다음과 같은 코드를 종종 볼 수 있다.

```c
 for (pinNumber = 4; pinNumber < 14; pinNumber++)
    {
        if (digitals[pinNumber] == 0)
        {
            sendDigitalValue(pinNumber);
            callOK();
        }
    }
```

위의 코드는 핀에 연결되어 있는 센서의 값을 보내는 코드의 일부이다.
위와 같이 반복문의 시작이 `pinNumber = 4`와 같이 4부터 값을 보내는 코드가 있는 반면에

```c
for (pinNumber = 0; pinNumber < 14; pinNumber++)
 {
    if(digitals[pinNumber] == 0)
    {
      sendDigitalValue(pinNumber);
      callOK();
    }
 }
```

위와 같이 `pinNumber = 0`부터 시작하는 코드도 있다.
첫번째 코드와 같이 핀 값을 보내면 digital 2,3 번째 포트는 사용할 수 없다. 왜 pin을 4부터 초기화하는지 알기 위해선 아두이노의 포트에 대해 알아야한다.

## 아두이노의 시리얼 통신핀

아두이노 보드의 0번 핀과 1번 핀은 RX, TX로 시리얼 통신을 하는 포트이다. 시리얼 통신은 1대 1 통신이고, 아두이노와 컴퓨터가 연결될 때 사용된다. 따라서 0번과 1번 핀을 다른 용도로 사용하는 경우 아두이노 스케치를 업로드하는 문제가 발견할 수 있기 때문에 사용에서 제외시키는 것이다.

## 아두이노의 인터롭트와 시리얼 통신핀

만약 추가적인 시리얼 포트가 필요한 경우 외부 인터롭트가 하드웨어적으로 구현이 되어있는 2,3번 핀과 SoftwareSerial 라이브러리를 사용할 수 있다.SoftwareSerial 라이브러리는 일반적으로 동작하는 2,3번 핀을 0,1 번핀과 같은 RX,TX핀처럼 동작하게 해준다. 따라서 첫번째 코드와 같이 4번핀부터 사용하는 경우는 블루투스와 같은 추가적인 Serialport를 제공하는 펌웨어에서 볼 수 있다. 0,1번 핀을 사용하지 않는 이유와 동일하다.

정리하면 다음과 같다.

- 아두이노의 0,1 번핀은 시리얼 통신 핀이다.
- 아두이노의 2,3 번핀은 추가적인 시리얼 포트 사용을 위한 핀이다.

개발 중인 펌웨어는 SoftwareSerial과 Bluetooth를 사용하지 않으므로 범용적인 사용을 위해 2번 핀부터 사용한다.
