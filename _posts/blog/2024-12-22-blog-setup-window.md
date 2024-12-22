---
title: "[Blog] Blog Window"
date: 2024-12-22 + 0000
categories: [Blog]
tags: [blog]		# TAG는 반드시 소문자로 이루어져야함!
pin: false
---

지금까지 Linux에서만 사용하던 깃헙 블로그를 윈도우에서도 사용하기 위해 정리한 글입니다.

## 개발환경

현재 Github blog는 Windows 11을 사용합니다.

## Ruby 설치

Window에서 Ruby 는 3.2.6-1 버전을 사용했습니다. 다음 [link](https://rubyinstaller.org/downloads/)에서 다운로드해서 설치하면 됩니다. 옵션은 3 - MSYS2 and MINGW development toolchain 설치했습니다.

## 기타 필요한거 설치

블로그 빌드할 때 필요한 패키지인걸로 압니다. 잘 몰라서 따라했습니다.

```
gem install jekyll bundler
```

## 필요한 gem 설치

이 포스팅 목적이 예전에 만들어놓은 블로그 윈도우에서 빌드하기라 처음부터 하는 건 아닙니다.

이미 소스코드가 있다고 가정하고 다음을 실행했습니다.

```
bundle install
bundle update
bundle install
```

이때, "wdm" 패키지가 0.1.1 버전이 설치가 안됩니다. (예전에 썼던 버전). 그래서 2.0.0 버전 설치하고, Gemfile에서는 버전 명시를 지웠습니다.

## 로컬에서 호스팅

이제 블로그가 정상 호스팅 되는지 봅니다.

```
bundle exec jekyll serve
```

