---
layout: post
title: 고음질 KBS Classic FM 청취를 위한 여정
tags: [KBS, Tuner]
---

업그레이드와 튜닝의 종착지는 순정이라고 하듯이 오디오의 종착지는 단촐한 라디오라는 말이 있다. 하이엔드 오디오 매니아까지는 아니지만 FM 라디오의 매력에 빠져 아직도 진행중인 고음질 KBS Classic FM 청취를 위한 여정을 기록해 본다.

### 아날로그와 디지털

라디오의 원리는 기본적으로 음성신호를 전파에 담아서 전달하고 수신하는 쪽에서 음성신호를 추출하는 과정이다. 음성신호를 디지털로 보낼 수 있지만 이럴경우 디지털TV처럼 디지털 수신기가 필요한데 FM방송이 재난방송 의무송신 매체로 지정된 만큼 당분간은 아날로그 송신이 지속되리라 생각된다.

### 일반안테나와 공청안테나

라디오의 기본원리를 이해하면 고음질의 FM방송청취를 위해 우선적으로 확보해야 하는것이 고품질의 전파임을 알 수 있다. 튜너성능의 반은 수신력이라고 할 만큼 매우 중요해서 무전기가 전신인 리복스, 켄우드 등이 좋은 튜너에 속하는 이유이기도 하다.  
10년 이내의 아파트에 거주한다면 [방송 공동수신설비의 설치기준에 관한 고시](http://www.law.go.kr/%ED%96%89%EC%A0%95%EA%B7%9C%EC%B9%99/%EB%B0%A9%EC%86%A1%EA%B3%B5%EB%8F%99%EC%88%98%EC%8B%A0%EC%84%A4%EB%B9%84%EC%9D%98%EC%84%A4%EC%B9%98%EA%B8%B0%EC%A4%80%EC%97%90%EA%B4%80%ED%95%9C%EA%B3%A0%EC%8B%9C)에 따라 공청안테나 사용이 가능하므로 벽체 FM 콘센트로 고음질 FM 청취가 가능하다. 공청안테나가 없다면 일반안테나로 전파를 수신해야 하는데 시간과 비용은 복불복이다.

### KBS 수신안내지도

![kbs-fm-map](/assets/images/kbs_fm_map.png)  
아날로그 튜너를 고려한다면 가장 먼저 확인해야 하는 것이 수신위치에서의 전파경로 및 수신상태인데, [KBS 수신안내지도](https://map.kbs.co.kr/map.jsp)에서 확인 가능하다. 만약 노란색(잡음발생)영역에서 FM 방송을 들어야 한다면 아날로그 튜너는 깨끗이 접고 스트리밍 서비스로 전환해야 한다. 수신불량 지역에서 최고급의 튜너, 안테나, 증폭기, 필터 등 장비빨(?)로 극복했다는 뉴스나 블로그를 아직 본 적이 없다. 사용법은 구글링하면 자세히 나와있지만 '주소입력>FM방송선택>FM채널선택>FM주파수선택' 으로 수신위치에서 전파경로분석과 수신상태를 대략적으로 확인 할 수 있다.

- KBS kong: [Andoird](https://play.google.com/store/apps/details?id=kr.co.kbs.kong), [iOS](https://apps.apple.com/kr/app/kbs-kong/id928368733)

### 튜너랭킹

구글링에서 나오는 튜너 랭킹은 대부분 [Tuner Information Center](ttps://www.fmtunerinfo.com/standings.html)에서 참조하는 듯 한데, 최종버전이 2006년 2월 12일 이지만 기본적으로 빈티지 튜너에 대한 정보를 제공하는 곳이라 감안해서 참고하면 될 듯 하다. 튜너도 전자제품이라 수명이 있으므로 관리(콘덴서 교체 등)에 스트레스가 있으면 너무 오래된 것은 피하는게 맞을 것 같고, 기본적으로 스펙도 확인하고 선택하는게 현명해 보인다.

- 튜너랭킹에 사진첨부: [http://blog.daum.net/adallo/7678280](http://blog.daum.net/adallo/7678280)
- 스펙 보고 좋은 튜너 고르기: [https://adultkid.tistory.com/621](https://adultkid.tistory.com/621)

### 프리엠퍼시스, 디엠퍼시스

FM 주파수 특성상 주파수를 변조하기 전에 증폭을 해서 송신하고 수신쪽에서 다시 원래의 신호를 재생하는데 이를 제어하하는 회로를 프리엠퍼시스, 디엠퍼시스라고 한다.  
문제는 적용 시정수를 미국과 한국은 75uS, 유럽과 일본은 50uS를 사용하므로 국내에서 50uS를 사용하는 튜너는 고음이 과장될 수 밖에 없다. 과장된 고음을 좋아하면 상관없지만 아니라면 75uS로 설정된 튜너나 시정수를 선택할 수 있는 튜너를 사용해야 한다.
![sdr-radio-rds](/assets/images/sdr_radio_rds.png)  
(SDR-Radio.com의 설정화면)

### 디지털 튜너의 가능성

소프트웨어의 발달(?)로 튜너에서도 새로운 패러다임이 도입되고 있는데 쉽게 예기하자면 '전파만 받아주면 나머지는 소프트웨어로 처리할께'라는 의미이다. FPGA(Field Programmable Gate Array)로 접근하는 papa FM tuner와 SDR(Software-defined radio)를 활용한 소프트웨어 튜너가 대표적이다.  
물론 둘 다 고품질 전파를 필요로하며 평가도 엄청 좋은 편이다. 편하게 한방(?)에 가려면 papa FM tuner를 추천하고 PC와 친하고 저렴하게 가려면 SDR도 좋은 선택이 될 것이다. SDR도 저렴한 것부터 고가까지 있으니 적당한 수준(Airspy HF+ 추천)에서 선택하고 세팅만 잘하면 아날로그 튜너보다 훨씬 고음질로 청취할 수 있다.  
![hfplus-discovery](/assets/images/hfplus_discovery.png)  
(Airspy.com의 Airspy HF+ Discovery)

- papa FM tuner: [http://www.paparadio.net/](http://www.paparadio.net/)
- SDR hardware: [http://www.hdsdr.de/hardware.html](http://www.hdsdr.de/hardware.html)
- SDR software: [https://www.sdr-radio.com/](https://www.sdr-radio.com/)

멋진 목소리의 진행자와 의외의 선곡을 고음질로 즐기는 튜너의 세계에 빠져보시길...
