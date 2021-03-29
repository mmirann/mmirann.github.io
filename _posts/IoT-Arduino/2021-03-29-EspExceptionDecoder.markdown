---
layout: post
title: "[ESP32] Esp Exception Decoder - 스택 오버플로우 디버깅"
subtitle: ""
categories: [IoT/Arduino]
tags: [esp32]
comments: true
---

블루투스 통신을 위한 ESP32 보드를 개발하고 있는데, 스택 오버플로우 에러가 났다. 시리얼 창에 출력되는 에러는 철저하게 컴퓨터 기준이어서 어디서 난 건지 감도 안잡혔다. 코드를 살펴봐도 안보인다. 스택 오버플로우 에러가 발생하면 ESP32는 계속해서 리부팅된다. 시리얼 프린트 디버깅으로도 해결불가. 그래서 구글링하다가 `EspExceptionDecoder`을 발견하였다.

<br>

## Stack overflow 에러

```
guru meditation error core 1 panic'ed (loadprohibited). exception was unhandled.
```

![image](https://user-images.githubusercontent.com/48276682/112835275-04078400-90d4-11eb-9cbe-4facd881c11c.png)

컴퓨터가 아닌 이상 해독할 수 없는 오류.. 이 오류를 해석해주는 게 `EspExceptionDecoder`의 역할이다.

<br>

## Esp Exception Decoder 설치

> <https://github.com/me-no-dev/EspExceptionDecoder>

위의 깃허브로 들어가면 설치 방법이 잘 나와 있다.

1. Arduino IDE에 ESP32 보드가 설치되어 있어야 한다.
2. [다운로드 페이지](https://github.com/me-no-dev/EspExceptionDecoder/releases/tag/1.1.0)에 접속해서 `EspExceptionDecoder-1.1.0.zip`을 다운 받는다.
3. 압축을 풀고 `<home_dir>/Arduino/tools/EspExceptionDecoder/tool/EspExceptionDecoder.jar` 에 `jar`파일을 집어 넣는다.

   - 나는 `Window10`환경 기준으로 다음 경로에 넣어주었다.
   - `C:\Users\<UserName>\Documents\Arduino\tools\EspExceptionDecoder\tool`
   - `Arduino`폴더 안에 `tools`가 없어도 그냥 생성하면 된다.
     ![image](https://user-images.githubusercontent.com/48276682/112836722-e9cea580-90d5-11eb-89f1-5d3c50fa2b3a.png)

4. 아두이노 IDE 껐다 키기

![image](https://user-images.githubusercontent.com/48276682/112836333-6a40d680-90d5-11eb-9383-af083bbf2a4c.png)

짠! Esp Exception Decoder가 툴 메뉴에 추가된 걸 확인할 수 있다.

<br>

## Esp Exception Decoder 사용

사용 방법은 굉장히 간단하다.

1. 스케치를 컴파일하고 업로드한다.
   - 컴파일, 업로드를 안하는 경우 `elf`파일을 선택하라고 뜨기 때문에 바로 시리얼 창 키지 말고 컴파일 업로드를 먼저 한다.
2. 시리얼 창에 뜬 스택 오버플로우 오류를 복사한다.
3. 툴 > ESP Exception Decoder를 열어 오류를 붙여 넣는다.
4. 에러가 발생한 곳을 확인한다.

![image](https://user-images.githubusercontent.com/48276682/112836806-066add80-90d6-11eb-80d6-dcca78cca825.png)

짠! 위와 같이 오류를 복사하면 어떤 코드의 몇 번째 줄에서 발생한 오류인지 알려준다. 역시나 컴퓨터는 거짓말을 하지 않고 틀린 건 나였다 :( `BLECharacteristic` 객체를 선언만 해놓고 `createCharacteristic()`를 생략하고 냅다 `setValue()`를 해서 스택 오버플로우가 발생했던 것....

### Reference

- <https://github.com/me-no-dev/EspExceptionDecoder>
