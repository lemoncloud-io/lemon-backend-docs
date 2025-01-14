---
layout: default
title: Setup Lambda VPC without NAT
parent: Setup DevOps
---

# Lambda VPC 설정과 NAT 없이 구성

- [일반적] VPC가 필요한 서비스들(ex: backend-api)은 외부 필요없도록 구성.
- [외부용] 외부 연결(https포함) 필요시 VPC없이 돌아가도록

주의! `VPCE` 구성이 필요함.

----------------------
## 구성 방법

1. Prepare a `Public Subnet` like `public-2a/2c`
    - 기존 subnet에서 2가지를 고름 (+ 이름 지어주기)

1. Create a `Security Group` like `lemon-services-api`
    - add in-bound for all traffic from self

1. Create `Endpoints` in VPC for accessing internal AWS
    - create a security group as `infra-services`.
        - allow all traffics from `lemon-services-api`

    - add each for `kms`, `sns`, `sqs`, and `execute-api`
        - security-group: `infra-services`
        - subnets: `public-2a/2c`
        - each cost: `$8.76/m` + `$0.01/GB` for `Interface`. but free for `Gateway`

        * kms: `com.amazonaws.ap-northeast-2.kms` w/ `Interface`
        * sns: `com.amazonaws.ap-northeast-2.sns` w/ `Interface`
        * sqs: `com.amazonaws.ap-northeast-2.sqs` w/ `Interface`
        * lambda: `com.amazonaws.ap-northeast-2.lambda` w/ `Interface`
        * dynamodb: `com.amazonaws.ap-northeast-2.dynamodb` w/ `Gateway`

    - if s3 is required in VPC.
        * s3: `com.amazonaws.ap-northeast-2.s3` w/ `Gateway`


----------------------
## 배포후 테스트하기

see `lemon-templates-api`, and check `/hello/echo`.

