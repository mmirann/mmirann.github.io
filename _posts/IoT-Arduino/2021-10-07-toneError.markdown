---
layout: post
title: "[Arduino] multiple definition of `__vector_7' 에러"
subtitle: ""
categories: [IoT/Arduino]
tags: [Arduino]
comments: true
---

## ISR 중복 사용 에러

펌웨어 개발 중에 다음 코드와 같은 에러가 발생했다. 
```c
Tone.cpp.o (symbol from plugin): In function `timer0_pin_port':
(.text+0x0): multiple definition of `__vector_7'
\arduino_build_600754\sketch\Scratch3Firmware.ino.cpp.o (symbol from plugin):(.text+0x0): first defined here
collect2.exe: error: ld returned 1 exit status
exit status 1
```

아두이노의 피에조 부저 센서를 사용하기 위한 `Tone` 헤더파일의 타이머 함수와 인터롭트 서비스 루틴을 위해 따로 정의한 `ISR(TIMER1_COMPA_vect)`함수를 동시에 사용할 수 없는 문제였다. 사용자 코드에서 이 벡터를 정의해 중복 정의를 하여 발생하는 에러 메세지이다. 두 개 다 프로그램 개발을 위해선 꼭 필요했기 때문에 구글링을 통해 다음과 같은 솔루션을 찾았다. 대부분 Tone 헤더파일과 IRRemote, NewPing 등의 타이머를 사용하는 헤더파일을 중복하여 사용하면 발생하는 에러였다. 

<br>

## SOL1) NewTone 라이브러리 이용

Tone 대신에 [NewTone](https://bitbucket.org/teckel12/arduino-new-tone/wiki/Home) 라이브러리를 사용해봤다. `tone` 함수 대신 `NewTone` 함수를 사용하면 된다. 컴파일하니 다음과 같은 에러 메시지가 출력됐다.

```c
\libraries\NewTone\NewTone.cpp.o (symbol from plugin): In function `NewTone(unsigned char, unsigned long, unsigned long)':
(.text+0x0): multiple definition of `__vector_11'
\libraries\Servo\avr\Servo.cpp.o (symbol from plugin):(.text+0x0): first defined here
collect2.exe: error: ld returned 1 exit status
exit status 1
```

서보모터 라이브러리와 같은 이유로 충돌한다. 사용 불가능하다. 

<br>

## SOL2) TimerFreeTone 라이브러리 이용
Tone 대신에 [TimerFreeTone](https://bitbucket.org/teckel12/arduino-new-ping/wiki/Multiple%20Definition%20of%20%22__vector_7%22%20Error) 라이브러리를 이용한다. `tone`함수 대신에 `TimerFreeTone` 함수를 이용하면 되지만 `noTone`을 대체할 함수는 없다. 프로그램에 필요한 함수는 아니어서 없어도 무방했다. 컴파일 하니  정의한 `ISR` 함수와 충돌나지 않고 정상적으로 컴파일됨을 확인했다. 라이브러리 이름 그대로 타이머를 사용하지 않아서 해결되었다. 예제 프로그램은 다음 코드와 같다.

```c
#include <TimerFreeTone.h>

#define TONE_PIN 6 // Pin you have speaker/piezo connected to (be sure to include a 100 ohm resistor).

// Melody (liberated from the toneMelody Arduino example sketch by Tom Igoe).
int melody[] = { 262, 196, 196, 220, 196, 0, 247, 262 };
int duration[] = { 250, 125, 125, 250, 250, 250, 250, 250 };

void setup() {}

void loop() {
  for (int thisNote = 0; thisNote < 8; thisNote++) { // Loop through the notes in the array.
    TimerFreeTone(TONE_PIN, melody[thisNote], duration[thisNote]); // Play melody[thisNote] for duration[thisNote].
    delay(50); // Short delay between notes.
  }
}
```

<br>

## Reference

- <https://bitbucket.org/teckel12/arduino-new-ping/wiki/Multiple%20Definition%20of%20%22__vector_7%22%20Error>