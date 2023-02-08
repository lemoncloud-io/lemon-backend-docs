---
layout: default
title: Setup VPC/NAT
nav_order: 2
---

# Setup VPC/NAT for AWS Lambda

Lambda에서 VPC 이용과 NAT 연결 설정 방범 정리

- `VPC`에서 `Lambda` 구성시 인터넷 연결이 끊어지는데, `NAT` 구성으로 가능함.
- `NAT`는 `ElasticIP` 설정으로 외부에서는 고정 아이피로 설정 가능함.

![](../../../assets/images/vpc-diagram.png)

----------------------
## 구성 방법

1. Create a new `VPC` like `<profile>-vpc`.
1. Create a new `IGW` (Internet Gateway) for public Internet from inside your VPC.
1. Create a `Public Subnet` like `<profile>-public-2a`
    - create new `Route Table` like `<profile>-route-public`.
    - add a new route to route `IGW` from `0.0.0.0/0`
    - assign this route table to subnet.
1. Create a new `EIP` (Elastic IP) address like `<profile>-public-eip`.
1. Create a new `NAT` Gateway like `<profile>-public-nat`
    - assign it to the `Public Subnet`.
    - assign `Elastic IP` address.
1. Create a `Private Subnet` like `<profile>-private-2a`
    - create new `Route Table` like `<profile>-route-private`.
    - add a new route to route to your `NAT` Gateway from `0.0.0.0/0`.


----------------------
## 구성 예제

**Lemon**

| Type      | Name                  | Description                   | Range             |
|--         |--                     |--                             |--                 |
| VPC       | `LemonVpc`            | vpc: `vpc-0000000`            | 10.0.0.0/16       |
| Subnets   | `lemon-public-2a`     | Default Route -> IGW          | 10.0.0.0/24       |
| Subnets   | `lemon-private-2a`    | Default Route -> NAT          | 10.0.1.0/24       |
| Subnets   | `lemon-private-3c`    | Default Route -> NAT          | 10.0.2.0/24       |
| NAT       | `lemon-public-nat`    | `198.51.100.4`                | 10.0.0.4          |


----------------------
## 구성 테스트하기

use sample [lemon-hello-api](https://github.com/lemoncloud-io/lemon-hello-api)

- 해당 `<profile>-private-2a`의 VPC/Subnet 으로 Lambda를 올림 (see `env/config.js`)
- http request 로 인터넷 연결이 되는지 확인
- 필요시 `VPC Endpoint` 구성 필요.

