---
layout: default
title: Setup DevOps
nav_order: 99
has_children: true
permalink: /docs/devops
---

# Setup DevOps
{: .no_toc }

Lemon DevOps 관련 문서 정리
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Basic

원본 문서 [lemon-devops-doc](https://github.com/lemoncloud-io/lemon-devops-doc) 참고하여, 공개되는 내용만 옮겨옴
{: .fs-3 .fw-300 }

### 명명 규칙 (Naming Rule) - `JS+TS` 기준.

- 기본적으로 `Camel` 스타일을 유지 ex: `sayHelloWorld(name: string)`
- 클래스/인터페이스는 첫글자가 대문자로, `Camel` 스타일. ex: `UserGuests{}`
- 데이터는 `json` 호환이며, 멤버 이름은 `Camel` 형식 ex: `.userName`
- API의 endpoint는 `Dash` 스타일로 소문자로. ex: `/user/:id/say-hello-world`
- 깃 커밋시 코멘트는 [Karma Style](http://karma-runner.github.io/4.0/dev/git-commit-msg.html) 유지.
- 클래스(Class) 파일 이름은 `Dash`스타일. 예: `class HelloWorld{}` -> `hello-world.ts`
- 테스트 스펙 파일은 `.spec.ts` 으로 설정. 예: `hello-world.spec.ts`


### 에러 처리 (Exception)

- 에러 메세지는 `에러코드(숫자) + 메세지(대문자) - 설명`로 표현. 다만 숫자는 http 프로토콜 호환성. 예: `new Error('404 NOT FOUND')`

    - `302` 에러: Redirect.
    - `4XX` 에러: 사용자 입력 관련 에러 (입출력 데이터 이상. 예상가능한)
    - `5XX` 에러: 서버에러 (unexpected)

    - `302 REDIRECT - http://~` : redirect to `http://~`.
    - `400 INVALID PARAM - param:name` : `param.name` is not valid.
    - `404 NOT FOUND - id:abc` : abc is not found in resource.
    - `500 SERVER ERROR` : 알 수 없는 서버 에러.

- 인라인 에러 처리는 아래와 같이 표현 가능

    ```ts
    const a = {
        sayHello: async () => throw new Error('404 NOT FOUND - me')
    };
    //! call and check error.
    a.sayHello().catch((e: Error) => {
        if (`{e.message}`.startsWith('404 NOT FOUND')) return true;
        throw e;
    })

    //! equivalent code.
    try {
        a.sayHello();
    }catch(e: Error){
        if (`{e.message}`.startsWith('404 NOT FOUND')) return true;
        throw e;
    }
    ```


### 파라미터 검증 (Validation)

- 파이미터에서 필수 속성이 없을 경우. `@name` 형식으로 시작.

    ```ts
    function fx(name: string){
        if (!name) throw new Error('@name (string) is required');
    }
    ```

- 노드/오브젝트의 필수 속성이 없을 경우. `.name` 형식으로 시작.

    ```ts
    function fx(node: User){
        if (!node) throw new Error('@node (NodeModel) is required');
        if (!node.name) throw new Error('.name (string) is required');
    }
    ```
