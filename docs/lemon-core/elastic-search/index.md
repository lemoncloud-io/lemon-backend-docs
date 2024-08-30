---
layout: default
title: Use ElasticSearch
parent: Use lemon-core
has_children: true
nav_order: 1
permalink: /docs/lemon-core/elastic-search
---

# Use ElasticSearch
{: .no_toc }

lemon-core의 `elaticService` 사용을 위한 기본 안내서
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## ElasticSearch란?

ElasticSearch는 실시간 분산 검색 및 분석 엔진으로, 대용량의 데이터를 신속하게 저장, 검색, 분석할 수 있는 기능을 제공합니다. JSON 형태로 데이터를 저장하며, 분산 환경에서 동작하여 대규모 데이터를 처리하는 데 적합합니다. 주로 로그 및 이벤트 데이터의 실시간 분석, 전체 텍스트 검색, 추천 시스템, 보안 분석 등에 사용됩니다.

### 특징
- **고성능**: 대규모 데이터 처리 및 실시간 검색이 가능.
- **확장성**: 분산 시스템으로 설계되어 수평 확장이 용이.
- **RESTful API**: HTTP를 통해 데이터의 색인, 검색, 관리가 가능.
- **JSON 문서**: 데이터를 JSON 형식으로 저장하고 처리.

### 사용 예시
- **로그 분석**: 서버 로그를 수집하고 분석하여 실시간 모니터링 및 이상 탐지.
- **검색 엔진**: 웹사이트 내 문서 및 제품의 빠른 검색.
- **데이터 분석**: 대규모 데이터를 실시간으로 분석하여 통계 및 시각화.

---

## 서비스 소개

ElasticSearch Service는 ElasticSearch 6.x부터 7.x를 위한 공통 서비스를 제공합니다.

이 서비스는 기본적인 CRUD 작업을 수행하며, 추가적으로 인덱스 생성, 삭제, 갱신 및 검색 기능을 포함합니다.

### 지원 버전

| ElasticSearch 버전 | OpenSearch 버전 |
|:-------------------|:-----------------|
| 6.2                | 1.1              |
| 6.8                | 1.2              |
| 7.1                | 2.13             |
| 7.2                |                  |
| 7.10               |                  |

---

## ElasticService

[ElasticService](elastic-service) 는 ElasticSearch 클라이언트를 사용하여 인덱스 생성, 삭제, 갱신 및 검색 기능을 제공하는 서비스입니다.

기본적인 CRUD 작업 외에도 다양한 설정을 통해 ElasticSearch를 쉽게 관리할 수 있습니다.

- 인스턴스 생성 및 기본 사용법
- 인덱스 관리 (생성, 삭제, 갱신)
- 데이터 CRUD 작업
- 검색 기능 (단순 검색, 원시 쿼리 검색)

[더 알아보기](elastic-service)

---

## Elastic6QueryService

[ElasticQueryService](elastic-query-service) 는 ElasticSearch 6.x부터 7.x를 위한 쿼리 서비스를 제공합니다.

ID 기반의 간단한 검색을 지원하며, Autocomplete 검색을 위한 기능도 포함하고 있습니다.

- ID 기반 검색
- 간단한 검색 쿼리 작성 및 실행
- Autocomplete 검색 기능

[더 알아보기](elastic-query-service)

---

## HangulService

[HangulService](hangul-service) 는 한글을 처리하기 위한 공통 서비스를 제공합니다.

이 서비스는 한글을 자모 시퀀스로 변환하고, 자모를 QWERTY 키보드 입력으로 변환하는 기능 등을 포함합니다.

- 한글 여부 확인
  - 한글 텍스트인지 확인하여 한글 관련 기능을 제공
- 자모 시퀀스로 변환
  - 검색 엔진에서 초성 검색을 지원 (예: "한글" -> "ㅎㄱ")
  - 텍스트 분석 및 처리에 활용 (예: 특정 패턴 추출)
- QWERTY 키보드 입력 시퀀스로 변환
  - 사용자가 QWERTY 키보드로 입력한 한글을 변환하여 검색 지원 (예: "gksrmf" -> "한글")
- 초성 추출
  - 검색 엔진에서 초성만으로도 검색할 수 있도록 지원 (예: "한글" -> "ㅎㄱ")

[더 알아보기](hangul-service)