---
layout: post
title: Volumio에 Tidal Connect 설치
tags: [Volumio, Tidal]
---

Volumio에서 Tidal Connect를 UI에서 간단히 활성화 할 수 있지만 유료서비스로 바뀌어 무료(?)로 이용하려면 커맨드로 설치를 해야 한다. 커맨드로 설치하면 UI에서 시스템 업데이트가 안되는 단점이 있지만 30분 정도만 시간내서 최신버전으로 재설치하면 되니까 부담도 거의 없는 편이다.

## Volumio 설치

Volumio 공식홈에서 최신버전을 다운받아 설치한다. (현재 3.198)  
설치방법은 공식홈이나 구글링하면 많은 포스트가 있으니 참조한다.

## SSH 활성화 및 접속

http://{Volumio Local IP}/dev 로 들어가서 SSH를 활성화 한다.  
![volumio-ssh-enable](/assets/images/volumio-ssh-enable.png)

SSH 클라이언트(ex.PuTTY)로 접속한다. 기본 계정과 비밀번호는 volumio 이다.  
![volumio-ssh-login](/assets/images/volumio-ssh-login.png)

## Tidal Connect 설치

구글링하면 라이브러리 부터 하나씩 자세히 설치하는 방법이 있는데 이를 한방에 설치할 수 있게 쉘로 작성해 주신 분이 있다.

```
$ curl -sSL https://raw.githubusercontent.com/shawaj/HiTide/main/install.sh | sudo bash
```

(출처. [https://github.com/shawaj/HiTide](https://github.com/shawaj/HiTide))

## MPD 재설치

Tidal Connect 설치가 예전버전(?)이라 그런지 MPD(Music Player Daemon)가 삭제되어 다시 설치해야 한다.

```
$ sudo apt install mpd
```

## Tidal Connect 연결 디바이스 찾기

```
$ sudo /usr/ifi/ifi-tidal-release/pa_devs/run.sh
```

실행 후

```
$ cat /usr/ifi/ifi-tidal-release/pa_devs/devices
```

로 현재 사용하는 DAC의 디바이스명을 확인 할 수 있으며 설정을 위해 메모장 등에 복사해 둔다.  
![volumio-tidal-devices](/assets/images/volumio-tidal-devices.png)

사용중인 AP-1 앰프의 DAC 이름이며, 참고로 LG 하이파이 플러스 모듈은 "HM: USB Audio (hw:5,0)" 이다.

## Tidal Connect 서비스 설정

```
$ sudo nano /lib/systemd/system/ifi-streamer-tidal-connect.service
```

메모장에 복사해둔 디바이스명으로 설정(추가)한다.  
형식은 --playback-device "디바이스명" \ 이다.  
DAC이 MQA를 지원하면 --enable-mqa-passthrough true \ 로 설정하면 된다.

![volumio-tidal-service](/assets/images/volumio-tidal-service.png)

## 자동시작 서비스 등록

```
$ sudo nano /etc/rc.local
```

라즈베리파이 부팅시 백그라운드 서비스로 실행할 수 있도록 설정(추가)한다.

```
$ systemctl start ifi-streamer-tidal-connect.service &
```

![volumio-tidal-start](/assets/images/volumio-tidal-start.png)

재부팅 후 서비스가 자동시작 되는지 확인해보고 타이달 앱 상단에서 Tidal Connect 디바이스를 선택하고 재생한다. Tidal Connect 서비스 설정에서 기본값을 수정하지 않았다면 "HiTide RasPi Streamer"로 보여진다.  
서비스 설정 텍스트를 보면 알겠지만 연결명, 로그레벨, MQA 등을 변경 할 수 있다.

![tidal-app-master](/assets/images/tidal-app-master.png)
![tidal-app-connect](/assets/images/tidal-app-connect.png)

참고. 라즈베리파이 보드 및 버전확인

```
$ cat /proc/device-tree/model
```

참고. MPD 상태확인

```
$ systemctl status mpd -l
```

참고. 시스템 로그확인

```
$ sudo journalctl -f
```
