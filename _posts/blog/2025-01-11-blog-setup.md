---
title: "[Blog] Blog setup guide"
categories:
  - Study
tags:
  - Blog
last_modified_at: 2025-01-11T23:20:00
---

지금까지 Linux에서만 사용하던 깃헙 블로그를 윈도우에서도 사용하기 위해 정리한 글입니다.

## 개발환경

현재 Github blog는 Windows 11을 사용합니다. 설치는 Ubuntu 내용도 같이 올립니다.

## Ruby 설치


### Window
Window에서 Ruby 는 3.2.6-1 버전을 사용했습니다. 다음 [link](https://rubyinstaller.org/downloads/)에서 다운로드해서 설치하면 됩니다. 옵션은 3 - MSYS2 and MINGW development toolchain 설치했습니다.

### Ubuntu
CLI로 Ruby를 설치하였습니다.

Ruby 설치 가이드는 다음 [링크](https://gorails.com/setup/ubuntu/20.04)를 쭉 따라가시면 됩니다. (Ruby / node.js 모두 설치하세요.)

## 기타 필요한거 설치

블로그 빌드할 때 필요한 패키지인걸로 압니다. 잘 몰라서 따라했습니다.

```
gem install jekyll bundler
```

## 필요한 gem 설치

Jekyll theme이나 기존 파일을 빌드하기 위해 Gemfile에 기재된 패키지들을 설치합니다.

```
bundle install
bundle update
bundle install
```

이후

```
bundle exec jekyll serve
```

를 실행하면 됩니다.

## tzinfo error

Ubuntu에서는 잘 실행되던 블로그가 윈도우에서는 안 되었습니다.

```
jekyll 4.3.4 | Error: tzinfo
```

위 에러가 발생했는데, tzinfo / tzinfo-data 패키지를 설치해도 안됩니다.

다음 포스팅 <https://thegeekcat.github.io/blogging/tzinfoError/>에서 해결책을 찾았습니다.

Gemfile에 

```
gem 'tzinfo'
gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```
기재하면 잘 실행됩니다.