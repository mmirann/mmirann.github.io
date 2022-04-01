---
layout: post
title: "[데이터통신] 다중접속, 회선할당방식"
subtitle: ""
categories: [Study/OS]
tags:
comments: true
---

## 다중접속(Multiple Access)
[데이터통신 수업](https://mmirann.github.io/study/datacommu/2021/01/28/dc3.html)에서 배웠던 다중접속 방식에 대해서 한번 더 짚고 넘어가도록 하겠다. 다중접속은 주어진 시간, 주파수, 공간, 코드 등을 여러 사용자가 공동으로 사용하게 하는 전송기술이다. 즉, 한정된 전송 자원을 다수의 노드들이 효율적/공평하게 공유할 수 있는가에 대한 문제이다. 

<br>

## 다중접속 vs 다중화(Multiplexing)
- **다중접속**: 사용자의 다중화 측면을 강조한 용어로, 통신시스템에서 사용, 서로 다른 **사용자(혹은 기기)** 들이 같은 통신 채널이나 전달 매체를 공유
  - 보다 많은 사용자에게 양질의 통신서비스를 제공하기 위함
  - 하나의 기지국에 여러 개의 이동통신단말이 접속을 요구하는 것
  - 여러 개의 end-point가 있는 다중 사용자 통신 환경에서 사용
  - 주로 데이터 링크 계층에서 제어
- **다중화**: 설비 효율화 측면을 강조한 보다 포괄적인 용어로 전송에서 사용, 서로 다른 **데이터 스트림이나 신호** 들이 같은 통신 채널이나 전달 매체를 공유
  - 하나의 회선 또는 전송로를 분할하여 개별적으로 독립된 신호를 동시에 송수신할 수 있는 다수의 통신로(채널)을 구성하는 기술
  - 전송장치(전송 프로토콜)이 주체가 되며, Media의 효율적인 사용을 위함
  - end-point가 두 개인 point-to-point 통신 환경에서 사용
  - 주로 물리 계층에서 제어 
  - TDM, FDM, WDM
  
둘이 하나의 통신 채널이나 전달 매체를 공유하는 공통점을 가지지만 위와 같은 차이점을 가지고 있다. CDMA, TDMA, FDMA와 같은 기술은 다중접속과 다중화가 혼합된 기술이라고 볼 수 있다.

<br> 

## 다중접속 기술 분류
### 고정 할당 방식 (Fixed)
- 중앙 집중 제어 위주: 구획(partitioning) 기반
- FDMA, TDMA, CDMA, OFDMA
### 동적 할당 방식 (Dynamic)
- 분산제어 위주: 공용 채널을다수가 함께 사용하기 위한 다중 접속 프로토콜
- 자원 경합 방식/ 경쟁 방식/ 임의 접근 방식
- 자원 비경합 방식/ 무경쟁 방식/ 제어 접근 방식

<br>

  
### FDMA(Frequency Division Multiple Access)
- 각각의 사용자 신호를 서로 다른 작은 주파수대역(부채널)에 실어보내는 방식
- 인접 채널 간 간섭이 생길 수 있으므로 보호대역(Guard Band)이 필수적임  
- 주파수 이용 효율에 한계가 있어 사용자 수가 제한됨
- 효울성이 낮아 더 이상 사용되지 않음

### TDMA(Time Division Multiple Access)
- 각각의 사용자 신호를 서로 다른 시간슬롯에 할당하여 실어보내는 방식
- 보통 TDMA는 오로지 시간 만으로 다른 사용자를 구분하기 보다, FDMA처럼 일단 주파수로 구분하고 동일 주파수대역에서 시간으로 나누어 다중접속
- 2세대 이동통신 GSM 방식에서 사용
### CDMA(Code Division Mulitple Access)
- 각각의 사용자 신호를 서로 다른 코드를 곱하여(직교성 보장) 달리 구분
- 코드의 직교성 활용
- 암호화 되어있어 보안성이 좋음 
- 3세대 이동통신 
### OFDMA(Orthogonal Frequency Division Multiplexing Access)
- FDMA, TDMA, CDMA와 같은 다중접속 방식을 OFDM 기술과 결합
- 주파수의 직교성 활용
- 4세대 이동통신 

<br>

## 회선할당방식
위성통신에서 다중접속은 여러 개의 지구국에 하나의 통신 위성을 거쳐 동시에 필요한 통신로를 설정하는 것이다. 한정된 가용주파수를 많은 지구국들이 사용하여 주어진 시간에 많은 정보를 전달하여 중계기의 활용을 높이는 기술이다. 이렇게 선정된  **다중접속 기술의 재원을 각 지구국에 할당하는 방식이 회선할당방식**으로, PAMA,DAMA, RAMA가 있다.

<br>

### PAMA(Pre Assignment Multiple Access)
- 사전에 할당하여 반영구적으로 주어진 slot을 사용하는 고정할당 방식
- 일정한 트래픽 전송이 계속적으로 요구되는 지구곡에 유용하고, slot할당에 필요한 제어시설에 필요하지 않기 때문에 시스템 구성이 간단함
- 각 지구국의 전송트래픽 변화가 많을 때 부적절하고 망 확장성 등 융퉁성이 없음

### DAMA(Demand Assignmetn Multiple Access)
- 위성 중계기의 자원을 많은 지구국들이 이용할 수 있도록 사용을 원하는 시간에만 사용함
- 사용하지 않는 slot을 비워두어 원하는 다른 지구국이 활용할 수 있도록 함
- PAMA와 달리 slot을 할당받기 위해 신호를 교환하는 slot과 장비가 필요함
- INTELSAT의 SPADE 방식


### RAMA(Random Assignment Multiple Access)
- 혼합 방식으로 전송 정보가 발생한 즉시 임의 송신하는 방식
- ALOHA 방식, slotted ALOHA 방식 등 패킷 전송망에서 많이 활용됨

<br>

## Reference
- <http://www.ktword.co.kr/test/view/view.php?m_temp1=403>
- <https://sanghee-9.tistory.com/entry/%EB%8B%A4%EC%9B%90%EC%A0%91%EC%86%8DMultiple-Access-%EB%B0%A9%EC%8B%9D>
