---
layout: default
title: Home
nav_order: 1
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
permalink: /
---

# 프로젝트 이름
{: .fs-9 }

이 프로젝트는 블라블라~~~~~~
{: .fs-6 .fw-300 }

[시작하기](#시작하기){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [깃헙보기](https://github.com/lemoncloud-io{{ site.baseurl }}){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## 시작하기

이 문서를 수정하기 위한 설정 및 기본 사항

### Local installation: Use VSCode

1. _처음설정:_ VSCode 의 확장 플러그인 `Remote Development` 설치하고, 재시작하기.
    - 그러면, `Dev Container: Just the docs` 가 실행할거냐 물어보고, 시작해줌
    {: .fs-2}
    - 그러면, `Terminal` 창에 도커기반 컨테이너 실행하고, 커맨드 실행할 수 있는 터미널 보임.
    {: .fs-2}
    - 


2. `_config.yml` 수정하기 (여러가지 설정들 변경)
  ```yaml
  title: 어떤 타이틀
  ```

3. _Optional:_ 검색데이터 초기화 (creates `search-data.json`)
  ```bash
  $ bundle exec just-the-docs rake search:init
  ```

3. `Jekyll server` 실행하기
  ```bash
  $ jekyll serve
  ```

4. 브라우저에서 열기 [http://localhost:4000](http://localhost:4000)


### 설정하기 (Configure)

- [See configuration options]({% link docs/configuration.md %})

