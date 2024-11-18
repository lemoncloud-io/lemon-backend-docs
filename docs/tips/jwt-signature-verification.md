---
layout: default
title: JWT Signature Verification
parent: Tips
---
# Research on Efficient JWT Signature Verification in Serverless Microservices

## 📄 Abstract

이 연구는 서버리스 마이크로서비스 아키텍처(MSA)에서 효율적인 JWT 서명 검증 방법을 제시하고자 합니다. 다양한 AWS Lambda 메모리 설정과 서명 알고리즘(RS256, RS512)을 사용하여 JWT 검증 성능을 분석하였으며, AWS KMS 기반 검증과 자체 서명 검증 방식을 비교하였습니다.

## 🛠️ Experimental Setup

### 1. Environment Configuration
- **Infrastructure**: AWS Lambda
- **Automation Tools**: Terraform, Serverless Framework
- **Runtime**: Node.js 18
- **Memory Settings**: 128MB, 256MB, 512MB, 1024MB, 2048MB

### 2. JWT Verification Methods
- **No Verification**: 검증을 수행하지 않는 대조군
- **KMS Verification**: AWS KMS를 사용한 서명 검증
- **Self-Signing Verification**: 공개 키를 사용한 자체 서명 검증

### 3. Signature Algorithms
- **RS256**: RSA-SHA256 알고리즘
- **RS512**: RSA-SHA512 알고리즘

## 📊 Results Analysis

![Response Time by Algorithm](assets/images/response-time-by-algorithm.png)
### 1. Verification Method Comparison
- **Self-Signing Verification** 방식은 네트워크 통신이 없어 가장 빠르고 안정적인 성능을 보였습니다.
  - 평균 응답 시간이 가장 낮았으며, 오차 범위(MAD)도 작아 고빈도 요청이 발생하는 환경에서 효율적입니다.
- **KMS Verification** 방식은 높은 보안성을 제공하지만, AWS KMS API 호출로 인한 네트워크 지연과 오버헤드로 인해 응답 시간이 증가하고 변동성이 컸습니다.

### 2. Signature Algorithm Performance
- **RS256**과 **RS512** 알고리즘 간 성능 차이는 미미했으나, **RS512**가 더 일관된 성능을 보였습니다.
  - RS512는 더 강력한 암호화 알고리즘이지만, 효율적인 서명 검증 과정 덕분에 안정적인 응답 시간을 기록했습니다.
  - 보안 요구 사항이 높은 환경에서는 RS512 알고리즘이 적합한 선택입니다.

![Response Time by Memory](assets/images/response-time-by-memory.png)
### 3. Memory Allocation Impact
- 메모리 용량이 **128MB**일 때, 모든 검증 방식에서 가장 긴 응답 시간을 기록하였으며, 특히 **KMS Verification**에서 성능 저하가 두드러졌습니다.
- **512MB** 이상의 메모리 할당에서는 안정적인 성능을 보였으며, 메모리 증가에 따른 성능 개선 효과는 미미했습니다.
  - **2048MB** 메모리 설정에서도 간헐적 성능 스파이크가 관찰되었으며, 이는 네트워크 상태나 AWS 내부 처리 지연의 영향으로 분석됩니다.
- **512MB** 메모리 설정이 성능과 비용의 균형을 맞출 수 있는 효율적인 선택으로 나타났습니다.

![Response Time Change Over Load Increase](assets/images/response-time-change-over-load-increase.png)
### 4. Load Testing Analysis
- Artillery를 사용한 부하 테스트에서, 요청량 증가에 따라 **Self-Signing Verification** 방식이 가장 일관된 성능을 보였습니다.
- **KMS Verification** 방식은 네트워크 지연과 AWS API 호출로 인해 성능 변동이 컸습니다.

## 🔍 Key Findings
- **KMS Verification** 방식은 높은 보안을 제공하지만, 네트워크 지연과 높은 리소스 사용으로 인해 성능 저하가 발생할 수 있습니다.
- **Self-Signing Verification** 방식은 비교적 낮은 응답 시간과 일관된 성능을 제공하여, 보안 요구 사항이 낮은 경우 효율적인 선택이 될 수 있습니다.
- **RS512** 알고리즘은 더 강력한 암호화에도 불구하고 안정적인 응답 시간을 기록하였습니다.
- 메모리 할당이 512MB 이상일 때, 성능과 비용의 균형을 맞출 수 있는 최적의 환경을 제공하였습니다.