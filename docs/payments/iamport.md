---
layout: default
title: 아임포트(iamport)
parent: Use Payments
nav_order: 2
---

# Use with 아임포트(iamport)
{: .no_toc }

[iamport.kr](https://iamport.kr) 사용을 통한 PG 결제.
{: .fs-6 .fw-300 }

참고 라이브러리 `@lemoncloud/lemon-payments-api`.
{: .fs-2 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 워크플로우(Workflow)

![](../../../assets/images/import-workflow.png)


### [](#workflow-1) 1.아임포트로 결제 요청

- 기본 iamport 결제 라이브러리 이용.

<div class="code-example" markdown="1">
```js
//! 결제 요청시 필요한 파라미터들...
{
    ...,
    pay_method: "card", // 결제 종류.
    merchant_uid: "<order-id>", // 주문ID (= 앞전에 만들어두어야함 )
    name: "주문(상품) 이름", // 주문(또는 상품) 이름
    amount: 1000,  // 결제 금액 (기본 KRW)
}
```
</div>
<!-- {% highlight markdown %} // 아래 방식은 생각보담 잘 작동안함!!! @220727/steve
```js
{
    ...,
    pay_method: "card", // 결제 종류.
    merchant_uid: "<order-id>", // 주문ID (= 앞전에 만들어두어야함 )
    name: "주문(상품) 이름", // 주문(또는 상품) 이름
    amount: 1000,  // 결제 금액 (기본 KRW)
}
```
{% endhighlight %} -->


### [](#workflow-2) 2.앱카드 결제 완료

- 결제 완료시, 콜백(or Redirect)로 결과 전송됨


### [](#workflow-3) 3.주문결제 완료

- 주문 완료 처리를 위해, `{ imp_uid, merchant_uid }` 포함한 데이터를 `주문API`에 요청함.


### [](#workflow-4) 4.결제 인증

- 결제 성공 여부를 검증하기 위해서 `결제API`에 검증 요청함. 이후 성공시 주문 완료됨.
- 사용자별 모든 결제 정보를 DB에 저장 기록해둠.


