# Parkbongil Pages

Parkbongil Pages 는 개인 블로그 입니다.  
정적인 사이트의 무료호스팅 서비스를 제공하는 [Github Pages](https://pages.github.com/)를 이용하고 있습니다. 테마는 [Beautiful Jekyll](https://github.com/daattali/beautiful-jekyll)를 사용했습니다. 주요특징 및 사용법은 Beautiful Jekyll 저장소를 참고바랍니다.

## Beautiful Jekyll 설정 및 변경

GitHub에서 [Beautiful Jekyll](https://github.com/daattali/beautiful-jekyll)의 README.md 를 참고합니다.

1. 해당 프로젝트를 포크 합니다.
2. 포크한 프로젝트의 Settings > Pages > Build and deployment > Source 를 Github Actions 으로 변경합니다.
3. [GitHub Actions 가이드](https://jekyllrb.com/docs/continuous-integration/github-actions/) 를 참조해서 Gemfile에 추가합니다.

```
group :jekyll_plugins do
    gem "jekyll-timeago", "~> 0.13.1"
end

gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]

gem "wdm", "~> 0.2.0" if Gem.win_platform?
```

4. \_config.yml 에서 timezone: "Asia/Seoul" 으로 설정 합니다.
5. beautiful-jekyll-theme.gemspec 에서 "rake", "~> 13.0" 으로 변경 합니다.

## 윈도우즈에서 Jekyll 환경 만들기

최초 Jekyll 환경을 만들고 운영하다가 PC를 포멧하거나 다른 PC에서 동일한 환경을 만들수 있습니다.
[Jekyll on Windows](https://jekyllrb.com/docs/installation/windows/)

1. Ruby+Devkit 3.3.6-2 (x64)을 설치합니다. (최초환경버전)
2. 설치한 루비 터미널을 엽니다.
3. 'ridk install' 을 실행 후 'MSYS2 and MINGW development toolchain' 옵션을 선택합니다.
4. 터미널에서 'gem install jekyll bundler' 를 입력합니다. (최초환경버전)
5. tzinfo, tzinfo-data, rake 등 라이브러리를 추가 및 업데이트 합니다.
6. 터미널에서 클론한 위치로 이동 후 Gemfile.lock 파일을 삭제합니다. (예. cd c:\parkbongil.github.io)
7. 터미널에서 'bundle install' 를 입력합니다.
8. 이후는 윈도우에서 Jekyll 실행과 동일합니다.

## 윈도우즈에서 Jekyll 실행

Windows 10에서 localhost로 실행해 보기 위해서 [윈도우즈에서의 Jekyll](https://jekyllrb-ko.github.io/docs/windows/)에서 제시하는 방법대로 설치 및 설정변경을 하면 됩니다. Ruby+Devkit 3.3.6-2 (x64) 버전으로 확인한 상태 입니다.

1. 설치한 루비 터미널을 엽니다.
2. 터미널에서 클론한 위치로 이동합니다. (예. cd c:\parkbongil.github.io)
3. 터미널에서 'chcp 65001' 를 입력합니다.
4. 터미널에서 'jekyll serve' 를 입력합니다.
5. 브라우저 주소창에 터미널 로그에 안내한 'localhost:4000'을 입력합니다.
