---
layout: default
title: OAuth and Pushes
parent: Use Accounts
nav_order: 2
---

# 사용자인증(OAuth) 및 푸쉬(Push)
{: .no_toc }


_참고_ 라이브러리 `@lemoncloud/lemon-accounts-api`
{: .fs-2 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview

`custom-backend-api`와 `lemon-accounts-api`의 인증연동과 푸쉬관리
{: .fs-5 .fw-300 }

![](../../../assets/images/oauth-push.png)

**[모델링]**

1. `Account`: 계정으로, 각 소셜 로그인 마다 1개씩 생성됨
    - `.id` := `identity-id` from AWS Identity Provider. _(참고 identity-id 는 social-id 에 1:1 매칭됨)_{: .fs-2 }
    - `.userId` := `User.id`

1. `User`: 유저(또는 사용자)로, 1유저는 N개의 계정 가능함.
    - `.id` := `<auto-sequence>`

1. `Device`: 장치로, 각 계정은 여러 장치(앱)에서 로그인 가능
    - `.id` := `<uuid>`
    - `.accountId` := `Account.id`


**[로그인-처리]**

1. 소셜(google/apple 등) 로그인 성공
    - (웹뷰방식) redirect를 통한 callback -> `POST /oauth/<provider>/token?code` 사용
    - (네이티브) 토큰 발급 요청 (구글/애플/카톡 가능) -> `POST /oauth/<provider>/native-token?clientId&identityToken` 사용
    - 결과값: `accounts-api` 나 `backend-api` 사용에 따라 약간씩 다름.

1. _(accounts-api)_ 내부적으로 `doGrantAccess()`을 통해서 AWS 액세스 토큰([GetCredentialsForIdentity](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetCredentialsForIdentity.html)) 발급
    - 결과값: `{ authId, credential, ... }`

1. _(backend-api)_ `계정(Account)`와 `유저(User)` 정보 생성 및 동기화
    - 결과값: `{ authId, credential, identityToken?, ... }`

1. _(frontend-app)_ firebase 로부터 푸쉬용 `deviceToken` 얻은 후, 디바이스 등록
    - (backend-api) `POST /accounts/register-device?deviceToken` 등록 요청
    - 결과값: `{ deviceId, ... }`
    -  **주의**) `deviceId`는 내부에 저정해두고 (ex: `localStorage`), 앞으로 계속 이용.

1. _(frontend-app)_ 매 `5분`마다 엑세시 토큰 재발급하여, 세션 유지함
    - `POST /oauth/<auth-id>/refresh?current&signature` 요청
    - `signature` 계산은 아래와 같음
    ```ts
    const hmac = (data: string, sig: string) => $U.hmac(data, sig, 'sha256');
    expect2(() => hmac('', '')).toEqual('xaJI7uxp6BdToJl8B6b6YF5eU4/ZnZIdOalMWjf3M+w=');
    expect2(() => hmac('a', '')).toEqual('HDQJIXFIViPTp4GIM3GfWLvEVMn0xlWMlfnftH9dQI4=');
    expect2(() => hmac('a', 'b')).toEqual('y0SLRAxCrIrQhPyKh5XJj1t4AjWcMF6r1X7Nsg4kiJY=');
    //WARN - userAgent 는 인증당시의 사용자 브라우저 기준임 (최대 30분)
    const data = [current, accountId, identityId, identityToken, userAgent].join('&');
    return hmac(hmac(hmac(data, authId), accountId), identityId)
    ```
    - 결과값: `OAuthTokenResult`


**[푸쉬설정]**

1. _(frontend-app)_ 마이페이지 / 푸쉬 / 수신사용 여부 체크
    - 변경시: 서버에 전달 (TODO)

1. _(frontend-app)_ 마이페이지 / 푸쉬 / 커뮤니티 수신 체크
    - 변경시: 서버에 전달 (TODO)


**[푸쉬메세지]**

1. _(목록)_ 내 푸쉬 목록 읽기는 `GET /pushes` 요청
    - 결과값: `{ page, limit, list:{ id, ... }[] }`

1. _(읽기)_ 내 푸쉬 목록 읽기는 `GET /pushes/<id>?mark&all` 요청
    - `mark: boolean` 읽기 처리 해줌
    - `all: boolean` 전체 읽기 처리 해줌 (풀백시 유용)
    - 결과값: `{ id, title, message, link? }`

1. _(송신)_ 사용자에게 푸쉬 `POST /pushes/<account-id>/send?message&returnUrl` 요청
    - 결과값: `{ id, error?, sent }`

1. _(수신)_ 모바일앱에 전송됨.
    - 데이터: `{ messageType, redirectUrl, pushId }`
    - 바로읽기처리시: `pushId`를 이용하여, 읽기처리
    - TODO) 읽기처리시 AWS 인증이 필요할 수 있음 -> 차후 개선

**[단체푸쉬]**

1. _(송신)_ `POST /pushes/<search-name>/send-batch?message&returnUrl` 요청
    - 결과값: `<string>` 푸쉬 배치ID
    - 사용자 (수신동의 여부) 검색후, 모든 사용자게 개별 푸쉬 전송. (TODO)




---
## HISTORY

| Date       | Author   | Description
|--          |--        |--
| 2022-08-08 | Steve    | summarized as overview.
