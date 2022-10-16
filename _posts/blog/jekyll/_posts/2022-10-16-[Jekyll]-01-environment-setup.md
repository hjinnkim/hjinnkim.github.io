---
title: "[Jekyll] 01. Github blog setup"
excerpt: "(Local) Environment setup for Jekyll blog"

categories:
  - Blog
tags:
  - [Jekyll, ruby, gem]

permalink: /blog/blog/001/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-16
last_modified_at: 2022-10-16
order: 2

---

# 1. Why Github blog & Jekyll?
  Github blog를 시작하게 된 계기는 간단하다. 공부할 내용이 많아 어딘가 기록할 필요가 있었다. 많은 개발자 및 엔지니어들이 그렇듯 blog에 기록하려 한다. 다만, 이를 위해 새로운 언어를 배우긴 싫기 때문에 Markdown으로 기록할 수 있는 블로그들 중, 평소에도 많이 쓰는 Github에서 제공하는 Github blog를 사용할 것이다.

  Jekyll은 여러 텍스트 파일(특히 Markdown)으로 부터 정적 웹사이트 구축을 위한 파일을 생성해주는 프레임워크이다. 또한, 수많은 Jekyll Theme들이 공개되어 있어 웹프로그래밍을 공부하지 않은 사람들도 테마를 다운받아 손쉽게 사용할 수 있다. *(그래도 커스터마이징하려면 싫어도 배워야한다...)*

  참고로 본인은 [
minimal-mistakes-choiiis-customized](https://github.com/choiiis/minimal-mistakes-choiiis-customized)의 테마를 사용하고 있다.

  참고로 본 포스트는 Ubuntu 20.04 환경을 기준으로 설명한다.

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
    <figcaption align="center"><b>이미 hjinnkim.github.io라는 레포지토리가 있기 때문에 경고가 뜬다</b></figcaption>
  </figure>

# 4. Running on local
  그리고 생성한 repository를 clone한 뒤, terminal에서 clone받은 directory로 이동한 뒤, 다음 명령어를 입력하여 fork한 theme에서 필요로 하는 플러그인을 설치한다.

  ```bash
  bundle install
  ```

  마지막으로 다음 명령어를 실행하면 local에서 실시간으로 블로그를 확인할 수 있다.
  ```bash
  bundle exec jekyll serve
  ```
  <figure align="center">
    <img src="{{site.gdrive_url_prefix}}1G8fy0ipJgTb6jZwi4M8WEFv45oIiKosN" title="bundle-exec">
  </figure>
  
  이후 웹브라우저에서 *http://127.0.0.1:4000*에 접속하면 현재 생성한 블로그를 확인할 수 있다.

# 5. Push on Github
  이후 포스트를 업로드하는 등, 새롭게 업데이트한 뒤 온라인 상의 자신의 블로그를 업데이트하고 싶으면 Github에 push하면 된다. 이때, 실제 자신의 블로그에 반영되기 위해서는 시간이 걸리게 된다. 따라서, 이러한 이유로 local에서 먼저 변경사항을 확인하고 최종적으로 Github에 반영하는 것을 추천한다.

# 6. References
  1. [https://seungwubaek.github.io/blog/post_1/#page-title](https://seungwubaek.github.io/blog/post_1/#page-title)
  2. [https://www.youtube.com/watch?v=ACzFIAOsfpM&t=575s](https://www.youtube.com/watch?v=ACzFIAOsfpM&t=575s)