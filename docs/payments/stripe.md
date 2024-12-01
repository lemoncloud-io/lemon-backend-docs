---
layout: default
title: 스트라이프(Stripe)
parent: Use Payments
nav_order: 3
---

# Stripe 구독 결제 시스템
{: .no_toc }

[stripe.com](https://stripe.com/) 기반 구독 결제 프로세스.
{: .fs-6 .fw-300 }

참고 라이브러리 `lemon-payments-api`, `@stripe/stripe-js`, `stripe`.
{: .fs-2 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 워크플로우(Workflow)
![](../../../assets/images/stripe-workflow.png)


### [](#workflow-1) 1. 상품 및 가격 설정

- Stripe 대시보드 또는 API를 통해 구독 상품 및 가격 플랜 정의
- 월간, 연간 등 다양한 결제 주기 설정

<div class="code-example" markdown="1">
```typescript
const price = await stripe.prices.create({
  unit_amount: 1000, // 금액 (센트 단위)
  currency: 'usd',
  recurring: {
    interval: 'month' // 월간 결제
  },
  product: 'prod_example'
});
```
</div>

### [](#workflow-2) 2. 고객 및 결제 방법 등록
- 고객 정보 생성
- 결제 방법(카드 정보) 등록
- 메타데이터 추가 가능

<div class="code-example" markdown="1">
```typescript
const customer = await stripe.customers.create({
  email: 'customer@example.com',
  source: 'payment-method-token',
  metadata: {
    user_id: 'MY_USER_ID', // uid
    plan: 'premium'
  }
});
```
</div>

### [](#workflow-3) 3. 구독 생성
- 고객과 가격 정보를 기반으로 구독 생성
- 무료 체험판 및 할인 옵션 설정 가능

<div class="code-example" markdown="1">
```typescript
const subscription = await stripe.subscriptions.create({
  customer: customer.id,
  items: [{ price: 'price_example' }],
  trial_period_days: 14,
  coupon: 'coupon_id'
});
```
</div>

### [](#workflow-4) 4. 웹훅(Webhook) 처리
- 구독 상태 변경, 결제 성공/실패 등 이벤트 수신
- 내부 시스템과 동기화

<div class="code-example" markdown="1">
```typescript
function handleWebhook(event: Stripe.Event) {
  switch (event.type) {
    case 'customer.subscription.created':
      // 구독 생성 처리
      break;
    case 'invoice.payment_succeeded':
      // 결제 성공 처리
      break;
  }
}
```
</div>

### [](#workflow-5) 5. 구독 관리
- 구독 업그레이드/다운그레이드
- 구독 취소
- 주기적 결제 처리

<div class="code-example" markdown="1">
```typescript
// 구독 업그레이드
const subscription = await stripe.subscriptions.update(
  subscriptionId,
  {
    items: [{ price: 'new_price_id' }],
    proration_behavior: 'always_invoice'
  }
);
// 구독 취소
const canceledSubscription = await stripe.subscriptions.del(subscriptionId);
```
</div>