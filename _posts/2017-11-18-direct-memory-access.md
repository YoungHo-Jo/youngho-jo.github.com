---
layout: single
published: true
date: '2017-11-18 16:04 +0900'
modified: '2017-11-18 16:04 +0900'
category: base
imagefeature: sitelogo.png
show_meta: true
comments: true
mathjax: false
gistembed: false
noindex: false
hide_printmsg: false
sitemap: true
summaryfeed: false
title: Direct Memory Access
tags:
  - Cortext M3
---

Cortex m3에서 DMA는 2개의 Controller에 의해 작동한다.
이 두 Controller는 총 12 채널을 가진다.
DMA1이 7개, DMA2가 5개를 관리한다.
내부에는 DMA 요청을 우선순위대로 처리하는 관리자가 존재한다. 
우선순위는 총 4단계로, very high, high, medium, low이다. 
이는 소프트웨어적으로 설정하는 값이며, 만약 같은 우선순위의 요청일 경우 하드웨어적으로 미리 정해진 우선순위에 따라서 처리한다. 
또한, 한 번 보내는 사이즈를 특정 시킬 수 있는데, 이는 byte, half word, word중 하나로 정할 수 있다. 따라서 사이즈에 맞게 시작과 도착 주소를 알맞게 설정해야 한다. 
DMA과정 중 총 3 번의 이벤트가 발생한다. Half Transfer, Trasfer complete, Transfer Error가 그것이다. 이는 각 채널마다 인터럽트를 이용하여 표시한다. 
Register에서 Memory 로의 이동 뿐만 아니라, Memory 에서 Memory로의 이동도 지원한다. 

![DMA block diagram.png]({{site.baseurl}}/images/media/DMA block diagram.png)

이번 실험에서 사용된 주요 부분을 마킹하여 표시하였다. 
DMA가 일어날때, Controller에 의해서 요청자는 ACK을 DMA에 보낸다. ACK을 받은 Controller는 처리가 끝날때 까지 가지고 있다가 처리가 끝나는 즉시 ACK을 요청자에게 돌려보낸다. 즉, DMA는 세 가지 주요 기능으로 작동한다. 보낼 데이터를 DMA_CPARx 또는 DMA_CMARx에 설정되어있는 시작주소를 가리키게 한다. 
보낸 데이터를 저장하는 것 또한 DMA_CPARx 또는 DMA_CMARx에 설정되어있는 시작 주소를 사용한다. 
연속 수행을 위한 DMA_CNDTRx는 후위감소로 작동하며, 앞으로 일어나야 할 전달 횟수를 가지고 있다. 따라서, 사용할때 보낼 최종 횟수를 이 레지스터를 통해 설정하면 된다. 
우선순위와 보내느 데이터 크기 설정은 DMA_CCRx 를 통해 가능하다. 
그리고 설정을 다 마친 후 DMA_CCRx의 ENABLE 을 설정하면 즉시 동작하게 된다. 
DMA 동작 모드에는 Circular mode가 있다. 이는 circular buffer와 연속되는 데이터 흐름을 보낼 때 사용한다. 예를 들어 ADC Scan 모드가 있다. 이 모드로 설정되면 보낼 데이터의 갯수는 자동으로 조정되고 계속해서 DMA 요청이 이루어진다. 
전송 중 에러 발생 시 앞서 기술했던 것 처럼 Error flag를 interrupt를 통해서 나타내는데, 이때 해당 채널을 DISABLE한다. 그러고 나서 DMA_CCR에 있는 TEIE(Transfer Error Interrupt Enable) bit를 set한다. 

![DMA_State_Flag_interrupt.png]({{site.baseurl}}/images/media/DMA_State_Flag_interrupt.png)

아래는 DMA의 채널과 H/W 우선순위를 나타내고 있다.

![DMA1_request_mux.png]({{site.baseurl}}/images/media/DMA1_request_mux.png)
![DMA1_requeset_by_channel.png]({{site.baseurl}}/images/media/DMA1_requeset_by_channel.png)
![DMA2_request_byChannel.png]({{site.baseurl}}/images/media/DMA2_request_byChannel.png)
![DMA2_request_mux.png]({{site.baseurl}}/images/media/DMA2_request_mux.png)

 

 

 
 

