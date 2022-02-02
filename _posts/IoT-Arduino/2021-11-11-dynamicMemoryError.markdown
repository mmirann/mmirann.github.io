---
layout: post
title: "[Arduino] 아두이노 동적 메모리 용량 부족 에러"
subtitle: ""
categories: [IoT/Arduino]
tags: [Arduino]
comments: true
---

## Not enough memory

개발 중인 쉴드가 피에조 부저로 동요를 연주하는 기능을 필요로 했다. 노래는 10개 정도로, 각각 길이가 20초 남짓이어서 전역변수로 선언한 변수 배열이 길어져 컴파일 도중 아래와 같은 에러가 출력됐다. 
```
Sketch uses 7838 bytes (24%) of program storage space. Maximum is 32256 bytes.
Global variables use 3816 bytes (186%) of dynamic memory, leaving -1768 bytes for local variables. Maximum is 2048 bytes.
Not enough memory; see https://support.arduino.cc/hc/en-us/articles/360013825179 for tips on reducing your footprint.
```
아두이노 측에서 제공한 링크로 들어가서 얻을 수 있는 건 딱히 없었다. 배열이나 스트링을 사용 중이면 길이를 줄이라던가, 중복되는 코드는 루프나 함수로 구현하라는 추상적인 얘기였다. 구글링을 해보니 방법은 **SRAM 대신 FLASH 메모리를 직접 사용**하도록 하는 것이었다.

<br>

## 아두이노 메모리

아두이노가 사용하는 메모리의 종류는 3가지로, 다음과 같다.
* **Flash Memory**(Program space): 32Kbyte, 아두이노 스케치와 부트로더가 저장되는 메모리로, 비휘발성 메모리이다.
* **SRAM**: 아두이노 스케치 실행 시 2Kbyte, 필요한 각종 변수와 버퍼 등이 저장되는 메모리로, 휘발성 메모리이다.
* **EEPROM**: 1Kbyte,길이가 긴 시간 자료 등을 저장할 때 사용하는 메모리로, 비휘발성 메모리이다.

아두이노를 컴파일하면 아래와 같은 멘트를 볼 수 있다.
```
스케치는 프로그램 저장 공간 239654 바이트(18%)를 사용. 최대 1310720 바이트.
전역 변수는 동적 메모리 14984바이트(4%)를 사용, 312696바이트의 지역변수가 남음. 
```
위의 프로그램 저장공간은 Flash Memory를 의미하고, 전역 변수는 SRAM을 의미한다. Flash Memory는 프로그램을 업로드할 때만 접근할 수 있고, 프로그램 실행 시에는 값을 변경하지 못한다. 따라서 크기가 큰 배열 변수들을 FLash Memory에 저장하기 위해서 `PROGEM` 키워드를 사용한다.

<br>

## PROGMEM 키워드

```c
const dataType variableName[] PROGMEM = {};
const PROGMEM dataType variableName[] = {};
```
`PROGMEM` 키워드를 사용하면 Flash Mememory에 고정이 되므로 값을 바꿀 수 없어 `const` 와 함께 사용한다. `PROGMEM`를 사용해 선언한 변수는 다음 함수를 사용해 하나씩 읽어온다.
* `pgm_read_byte_near(variableAddress)` : 8bit 변수(char, byte, uint8_t)
* `pgm_read_word_near(variableAddress)`: 16bit 변수(int, uint16_t)

기존에 사용하던 코드는 아래와 같다.
```c
const int pacmanMelody[] = {
    // Pacman
    // Score available at https://musescore.com/user/85429/scores/107109
    NOTE_B4, 16, NOTE_B5, 16, NOTE_FS5, 16, NOTE_DS5, 16, //1
    NOTE_B5, 32, NOTE_FS5, -16, NOTE_DS5, 8, NOTE_C5, 16,
    NOTE_C6, 16, NOTE_G6, 16, NOTE_E6, 16, NOTE_C6, 32, NOTE_G6, -16, NOTE_E6, 8,
    // 중략
};

void playMelody(int melody[], int melodySize, int tempo, int sec)
{
    // sizeof gives the number of bytes, each int value is composed of two bytes (16 bits)
    // there are two values per note (pitch and duration), so for each note there are four bytes
    int notes = melodySize / sizeof(melody[0]) / 2;

    // this calculates the duration of a whole note in ms (60s/tempo)*4 beats
    int wholenote = (60000 * 4) / tempo;

    int divider = 0, noteDuration = 0;
    long interval = sec * 1000;

    int buzzer = 6;

    for (int thisNote = 0; thisNote < notes * 2; thisNote = thisNote + 2)
    {
        unsigned long currentMillis = millis();
        if (currentMillis - previousMillis > interval)
        {  
            previousMillis = currentMillis;
            noTone(buzzer);
            break;
        }
        // calculates the duration of each note
        divider = melody[thisNote + 1];
        if (divider > 0)
        {
            // regular note, just proceed
            noteDuration = (wholenote) / divider;
        }
        else if (divider < 0)
        {
            // dotted notes are represented with negative durations!!
            noteDuration = (wholenote) / abs(divider);
            noteDuration *= 1.5; // increases the duration in half for dotted notes
        }
        // we only play the note for 90% of the duration, leaving 10% as a pause
        tone(buzzer, melody[thisNote], noteDuration * 0.9);

        // Wait for the specief duration before playing the next note.
        delay(noteDuration);

        // stop the waveform generation before the next note.
        noTone(buzzer);
    }
}
```
`playMelody`함수에서 `melody[]` 매개변수로 멜로디 배열을 받아온다. 이 melody에 들어가는 배열들이 `PROGMEM`으로 선언되어 있으므로 `pgm_read_word_near()`을 이용해 불러온다. melody를 사용하는 divider 선언부분과 tone 함수 부분을 수정하였다. 코드는 다음과 같다.

```c
const int pacmanMelody[] PROGMEM = {
    // Pacman
    // Score available at https://musescore.com/user/85429/scores/107109
    NOTE_B4, 16, NOTE_B5, 16, NOTE_FS5, 16, NOTE_DS5, 16, //1
    NOTE_B5, 32, NOTE_FS5, -16, NOTE_DS5, 8, NOTE_C5, 16,
    NOTE_C6, 16, NOTE_G6, 16, NOTE_E6, 16, NOTE_C6, 32, NOTE_G6, -16, NOTE_E6, 8,
    // 중략
};

void playMelody(int melody[], int melodySize, int tempo, int sec)
{
    // sizeof gives the number of bytes, each int value is composed of two bytes (16 bits)
    // there are two values per note (pitch and duration), so for each note there are four bytes
    int notes = melodySize / sizeof(melody[0]) / 2;

    // this calculates the duration of a whole note in ms (60s/tempo)*4 beats
    int wholenote = (60000 * 4) / tempo;

    int divider = 0, noteDuration = 0;
    long interval = sec * 1000;

    int buzzer = 6;

    for (int thisNote = 0; thisNote < notes * 2; thisNote = thisNote + 2)
    {
        unsigned long currentMillis = millis();
        if (currentMillis - previousMillis > interval)
        {  
            previousMillis = currentMillis;
            noTone(buzzer);
            break;
        }
        // calculates the duration of each note
        divider = pgm_read_word_near(&(melody[thisNote + 1]));
        if (divider > 0)
        {
            // regular note, just proceed
            noteDuration = (wholenote) / divider;
        }
        else if (divider < 0)
        {
            // dotted notes are represented with negative durations!!
            noteDuration = (wholenote) / abs(divider);
            noteDuration *= 1.5; // increases the duration in half for dotted notes
        }
        // we only play the note for 90% of the duration, leaving 10% as a pause
        tone(buzzer, pgm_read_word_near(&(melody[thisNote])), noteDuration * 0.9);

        // Wait for the specief duration before playing the next note.
        delay(noteDuration);

        // stop the waveform generation before the next note.
        noTone(buzzer);
    }
}
```

<br>

## Reference

- <https://www.arduino.cc/reference/en/language/variables/utilities/progmem/>
- <https://kocoafab.cc/fboard/104>