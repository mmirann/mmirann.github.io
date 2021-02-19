---
layout: post
title: "[Arduino] 시리얼 통신 USB vs pin(RX,TX)"
subtitle: ""
categories: [IoT/Arduino]
tags: [IoT, Arduino]
comments: true
---

이전 포스팅에서 시리얼 통신은 아두이노 보드의 0번, 1번 핀 또는 USB 연결을 통해 시리얼 통신을 할 수 있다고 언급했는데 이 두 방법의 차이점은 무엇인지 대표님이 잘 설명해주셔서 간단하게 정리하고자 한다.

![image](https://www.researchgate.net/profile/Samarjith_Biswas/publication/325115625/figure/fig3/AS:625887674380289@1526234658056/Arduino-UNO-The-Arduino-program-contains-two-main-parts-setup-and-loop-The-name.png)

[출처](https://www.researchgate.net/publication/325115625_Assessment_of_Surface_Roughness_Using_LVDT_A_Convenient_and_Inexpensive_Way_of_Measuring_Surface_Irregularities)

## RESET 버튼 사용 여부

우선 가장 큰 차이점은 **업로드 시**에 RESET 버튼을 눌러야하는지 아닌지의 차이다. USB를 이용해 업로드를 하는 경우 아두이노 우노 보드의 Reset 버튼을 누르지 않아도 된다. 이유를 알기 위해선 아두이노 우노 보드의 회로도를 살펴봐야 한다. 아두이노 보드에 있는 USB와 아두이노의 연결 역할을 하는 `USB TTL 컨버터`인 `CH340`가 있다. 이 곳에는 Reset 버튼을 누른 것과 동일한 역할을 하는 Reset 기능이 내장되어있다. 따라서 단순히 USB 포트를 이용해 코드를 업로드 하는 경우 따로 과정이 필요없다.

하지만 0번, 1번(RX, TX) 핀을 통해 코드를 업로드 하는 경우는 `ATmega라는 avr칩`을 거치지 않기 때문에 컴파일->업로드로 넘어가는 과정에서 타이밍 맞춰 Reset 버튼을 눌러주어야 한다. 만약 Reset버튼을 누르지 않은 경우 무한업로딩 현상이 발생할 수 있다. 이와 같이 핀을 통해 업로드하는 것은 유선보다는 불완전하고 USB 연결과 겹칠 수 있기 때문에 핀을 통한 업로드는 권장하지 않는다.
