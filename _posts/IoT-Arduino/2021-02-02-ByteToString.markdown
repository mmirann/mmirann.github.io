---
layout: post
title: "[Arduino] 아두이노 HEX 배열을 String으로 바꾸기"
subtitle: ""
categories: [IoT/Arduino]
tags: [IoT, Arduino]
comments: true
---

> 아두이노 HEX 배열을 String으로 바꾸기

---

아두이노에서 RFID 센서를 이용하면 RFID 센서가 카드나 키를 인식해서 카드번호를 알 수 있다. MRFC522 자체 함수를 이용하면 카드 번호가 크기가 4인 바이트 배열에 담기게 된다. 카드 번호는 아래와 같이 DEC, HEX 형식으로 출력 가능하다.

```c
#include <SPI.h>
#include <MFRC522.h>

#define SS_PIN 10
#define RST_PIN 9

MFRC522 rfid(SS_PIN, RST_PIN); // Instance of the class

void setup() {
  Serial.begin(9600);
  SPI.begin(); // Init SPI bus
  rfid.PCD_Init(); // Init MFRC522
  }
}

void loop() {

  // Reset the loop if no new card present on the sensor/reader. This saves the entire process when idle.
  if ( ! rfid.PICC_IsNewCardPresent())
    return;

  // Verify if the NUID has been readed
  if ( ! rfid.PICC_ReadCardSerial())
    return;

    // Store NUID into nuidPICC array
    for (byte i = 0; i < 4; i++) {
      nuidPICC[i] = rfid.uid.uidByte[i];
    }

    Serial.println(F("The NUID tag is:"));
    Serial.print(F("In hex: "));
    printHex(rfid.uid.uidByte, rfid.uid.size);
    Serial.println();
    Serial.print(F("In dec: "));
    printDec(rfid.uid.uidByte, rfid.uid.size);
    Serial.println();
}


/**
 * Helper routine to dump a byte array as hex values to Serial.
 */
void printHex(byte *buffer, byte bufferSize) {
  for (byte i = 0; i < bufferSize; i++) {
    Serial.print(buffer[i] < 0x10 ? " 0" : " ");
    Serial.print(buffer[i], HEX);
  }
}

/**
 * Helper routine to dump a byte array as dec values to Serial.
 */
void printDec(byte *buffer, byte bufferSize) {
  for (byte i = 0; i < bufferSize; i++) {
    Serial.print(buffer[i] < 0x10 ? " 0" : " ");
    Serial.print(buffer[i], DEC);
  }
}
```

위와 같이 크기가 4인 바이트 배열에 카드 번호가 저장되는데 엔트리와 아두이노가 카드 값을 주고 받기 위해선 배열도 가능하지만, 사용자가 블록을 이용해서 카드값을 비교하기 위해선 배열보다 스트링이 적합했다. 따라서 바이트 배열을 하나의 스트링으로 만드는 코드는 아래와 같다.

```c
    String hexstring="";
    for (byte i = 0; i < 4; i++) {
        hexstring += String(rfid.uid.uidByte[i], HEX);
      }
```

아두이노에서 사용되는 String은 c/c++과는 달라서 기억하기 위해 기록!  
만약 DEC을 저장하고 싶다면 HEX를 DEC으로 바꾸거나 아예 아무것도 안쓰면 된다. HEX가 길이가 더 짧기때문에 HEX를 이용하였다.
