---
title: "[Jekyll] 03. Unpublish posts"
excerpt: "Secret post"

categories:
  - Blog
tags:
  - [Jekyll]

permalink: /blog/blog/003/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-16
last_modified_at: 2022-10-16
order: 2

---

## 1. published option
  
  다음 옵션을 비공개하고 싶은 포스트의 설정 부분에 추가한다.

  ```bash
  published: false
  ```

## 2. local environment

  비공개 포스트는 기본적으로 로컬 블로그에서도 비공개처리 된다. 해당 비공개 포스트를 확인하기 위해서는 다음 명령어를 통해 로컬 블로그를 실행시켜야 한다.

  ```bash
  bundle exec jekyll serve --unpublished
  ```
