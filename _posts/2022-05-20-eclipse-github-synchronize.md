---
layout: post
title: 이클립스 프로젝트를 github에 올리기 간단버전
tags: [Github]
---

로컬에서 작업한 프로젝트를 깃헙에 올리려고 하니 이클립스에서 로컬 저장소를 먼저 생성하면 브랜치명이 master라서 깃헙의 main과 일치하지 않아서 브랜치를 이동해야 하고 저장소부터 연결하면 프로젝트를 이동하는 등의 귀찮은 작업을 좀 해야 한다. 그래서 간단하게 진행하는 방법을 정리해 본다.

작업환경은

- OS : Windows 10
- IDE : STS 4
- 프로젝트명 : hello-boot

## 1. Github 에서

hello-boot라는 저장소 생성

## 2. Git Bash 에서

```
# 프로젝트 폴더로 이동
$ cd ..

# 저장소 생성 및 초기화
$ git init

# 브랜치명을 main으로 강제 변경
$ git branch -M main

# 원격저장소 연결
$ git remote add orign https://github.com/parkbongil/hello-boot.git
```

## 2. Eclipse 에서

해당 프로젝트에서 마우스오른쪽버튼 > Team > Share Project...
![eclipse-share-project](/assets/images/eclipse-share-project.png)

끝.

처음부터 로컬 저장소를 생성할때 브랜치명을 main으로 하고 원격저장소를 연결하는게 전부이긴 하다.  
사실 github에서 repository를 생성하면 자세히 안내하고 있다. 구글링보다 가이드를 먼저보자.ㅠㅠ  
![github-hello-boot](/assets/images/github-hello-boot.png)
