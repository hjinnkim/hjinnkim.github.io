---
title: "Jekyll blog setup"
excerpt: "Local environment setup for Jekyll blog"

categories:
  - Blog
tags:
  - [Jekyll, ruby, gem]

permalink: /github-blog/Jekyll/first/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-16
last_modified_at: 2022-10-16

---

# 1. Why Github blog & Jekyll?
  Github blog를 시작하게 된 계기는 간단하다. 공부할 내용이 많아 어딘가 기록할 필요가 있었다. 많은 개발자 및 엔지니어들이 그렇듯 blog에 기록하려 한다. 다만, 이를 위해 새로운 언어를 배우긴 싫기 때문에 Markdown으로 기록할 수 있는 블로그들 중, 평소에도 많이 쓰는 Github에서 제공하는 Github blog를 사용할 것이다. 

  Jekyll은 여러 텍스트 파일(특히 Markdown)으로 부터 정적 웹사이트 구축을 위한 파일을 생성해주는 프레임워크이다. 또한, 수많은 Jekyll Theme들이 공개되어 있어 웹프로그래밍을 공부하지 않은 사람들도 테마를 다운받아 손쉽게 사용할 수 있다. 

  참고로 본인은 [
minimal-mistakes-choiiis-customized](https://github.com/choiiis/minimal-mistakes-choiiis-customized)의 테마를 사용하고 있다.

# 2. ruby & Jekyll bundler installation
  Jekyll 프레임워크는 Ruby 기반 프로그램이다. 따라서 Ruby를 설치한 뒤 쉽게 사용할 수 있다.
  
  ```bash
  # ruby installation
  apt install ruby-dev
  ```

  다음 Jekyll 프레임워크와 부속 도구를 설치해야 한다.
  
  ```bash
  # Jekyll framework & tool
  gem install jekyll bundler
  ```

# 3. Github blog repository
  Github blog를 사용하기 위해서는 Github blog용 repository를 생성해야 한다. 사용하고 싶은 Jekyll Theme를 fork(혹은 clone)하여 자신의 Github에 가져와야 한다. 이때, repository명은 **[자신의 Github ID].github.io**가 되어야 한다.

  <figure align="center">
    <img src="{{site.gdrive_url_prefix}}1bNxsJSdcDQHSCZCQnnlsqF6h636_O8kN" title="jekyll-theme-image">
  </figure>

  <figure align="center">
    <img src="{{site.gdrive_url_prefix}}1S--qVzNEdI2bQqL6Vk-UzuIjlxnin9HF" title="fork-image" >
    <figcaption align="right"><b>이미 hjinnkim.github.io라는 레포지토리가 있기 때문에 경고가 뜬다</b></figcaption>
  </figure>


  afadsfadsfasdfadsfasdfasdf

  <!-- ![fork-image]( {{site.gdrive_url_prefix}}1Eeylpez_SEYN0jMzYHwOh7A0aZJzMTHA) -->
