---
layout: default
title: Elastic Service
parent: Use ElasticSearch
grand_parent: Use lemon-core
nav_order: 1
permalink: docs/lemon-core/elastic-search/elastic-service
---

# Elastic Service
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
ElasticSearch 6.x부터 7.x를 위한 서비스를 제공합니다.

이 서비스는 기본적인 CRUD 작업을 수행하며, 추가적으로 인덱스 생성, 삭제, 갱신 및 검색 기능을 포함합니다.

---

## Constructor

`constructor(options: ElasticOption)`

ElasticIndexService 클래스를 사용하여 Elasticsearch 클라이언트 인스턴스를 생성할 수 있습니다. 객체를 생성할 때 constructor는 자동으로 호출되므로 사용자가 직접 호출할 필요는 없습니다.

- **Params:**
  - options: [ElasticOption](#elasticoption)
- **How to use:**
    ```typescript
    const service = new Elastic6Service({
        endpoint: 'https://my-endpoint',
        indexName: 'my-index',
    });
    ```

---

## Hello Elastic ✋

### hello()

`hello(): string`

서비스의 인스턴스를 식별하는 문자열을 반환합니다.
   - **Response:**
     - `string` : `elastic6-service:${this.options.indexName}:${this.options.version}`
   - **Response Example:**
     - `elastic6-service:indexName:6.8`
   - **How to use:**
     ```typescript
     const message = service.hello();
     console.log(message);  // "elastic6-service:my-index:6.8"
     ```

---

## Getter

### get client()

`client(): elasticsearch.Client`

ElasticSearch 클라이언트 인스턴스를 반환합니다.
   - **Response:**
     - `elasticsearch.Client` : ElasticSearch 클라이언트 인스턴스
   - **Response Example:**
   - **How to use:**
     ```typescript
     const client = service.client;
     ```

### get options()

`options(): ElasticOption`

서비스의 옵션을 반환합니다.
   - **Response:**
     - [ElasticOption](#elasticoption)
   - **Response Example:**
   - **How to use:**
     ```typescript
     const options = service.options;
     console.log(options.indexName);  // "my-index"
     ```


### get version()

`version(): number`

옵션으로 설정한 버전을 반환합니다.
   - **Response:**
     - `number`
   - **Response Example:**
     - `6.8`
   - **How to use:**
     ```typescript
     const version = service.version;
     console.log(version);  // 6.8
     ```


### get parsedVersion()

`version(): number`

parsedVersion을 반환합니다.
   - **Response:**
     - [ParsedVersion](#parsedversion)
   - **Response Example:**
     - `{ engine: 'os', major: 1, minor: 2, patch: 0 }`
   - **How to use:**
     ```typescript
      const parsedVersion = service.parsedVersion;
      console.log(parsedVersion.major);  // { engine: 'os', major: 1, minor: 2, patch: 0 }
     ```

### get isOldES6()

`isOldES6(): boolean`

Elasticsearch의 버전이 7.x 미만인지 확인합니다.

- **Response:**
  - `boolean`
- **Response Example:**
  - `true`
- **How to use:**
  ```typescript
  const isOld = service.isOldES6;
  console.log(isOld);  // true 또는 false
     ```

### get isOldES71()

`isOldES71(): boolean`

Elasticsearch의 버전이 Old 7 버전인지 확인합니다.

- **Response:**
  - `boolean`
- **Response Example:**
  - `true`
- **How to use:**
  ```typescript
  const isOld71 = service.isOldES71;
  console.log(isOld71);  // true 또는 false
     ```

### get isLatestOS2()

`isLatestOS2(): boolean`

OpenSearch의 버전이 최신 2.x 버전인지 확인합니다.

- **Response:**
  - `boolean`
- **Response Example:**
  - `true`
- **How to use:**
  ```typescript
  const isOS2 = service.isLatestOS2;
  console.log(isOS2);  // true 또는 false
     ```

---

## Methods


### executeSelfTest()

`protected executeSelfTest(): Promise<{ isEqual: boolean; optionVersion: ParsedVersion; rootVersion: ParsedVersion }>`

서비스의 버전과 실제 Elasticsearch 서버의 버전이 일치하는지 테스트합니다.

- **Response:**
  - `isEqual: boolean` : 버전이 일치하는지 여부
  - `optionVersion: ParsedVersion` : 옵션에 설정된 버전 정보
  - `rootVersion: ParsedVersion` : 서버의 실제 버전 정보
- **Response Example:**
     ```json
{
      isEqual: true,
      optionVersion: { engine: 'es', major: 6, minor: 2, patch: 0 },
      rootVersion: { engine: 'es', major: 6, minor: 2, patch: 3 },
  }
     ```
- **How to use:**
  ```typescript
  const testResult = await service.executeSelfTest();
  console.log(testResult.isEqual);  // true 또는 false
     ```

### listIndices()

`listIndices(): Promise<{ list: any[] }>`

모든 인덱스의 목록을 반환합니다.
   - **Response:**
     - `list: any[]` : 인덱스 목록
   - **Response Example:**
     ```json
     {
       "list": [
         {
           "pri": 1,
           "rep": 1,
           "docsCount": 1000,
           "docsDeleted": 100,
           "health": "green",
           "index": "my-index",
           "status": "open",
           "uuid": "uuid123",
           "priStoreSize": "1gb",
           "storeSize": "2gb"
         }
       ]
     }
     ```
   - **How to use:**
     ```typescript
     const indices = await service.listIndices();
     console.log(indices.list);
     ```

### getIndexMapping()

`getIndexMapping(): Promise<any>`

특정 인덱스의 매핑 정보를 반환합니다.

- **Response:**
- **Response Example:**
- **How to use:**
  ```typescript
  const mapping = await service.getIndexMapping();
  console.log(mapping);
     ```

### findIndex()

`findIndex(indexName?: string): Promise<any>`

주어진 이름의 인덱스를 찾습니다.
   - **Params:**
     - `indexName` : 찾을 인덱스 이름 (default: service.options의 `indexName`)
   - **Response:**
   - **Response Example:**
     ```json
     {
       "pri": 1,
       "rep": 1,
       "docsCount": 1000,
       "docsDeleted": 100,
       "health": "green",
       "index": "my-index",
       "status": "open",
       "uuid": "uuid123",
       "priStoreSize": "1gb",
       "storeSize": "2gb"
     }
     ```
   - **How to use:**
     ```typescript
     const index = await service.findIndex('my-index');
     console.log(index);
     ```

### createIndex()

`createIndex(settings?: any): Promise<{ status: number, index: string, acknowledged: boolean }>`

주어진 설정으로 인덱스를 생성합니다.
   - **Params:**
     - `settings` : 인덱스 설정 (default: [Elastic6Service.prepareSettings](#settings)로 생성)
   - **Response:**
     - `status: number`
     - `index: string`
     - `acknowledged: boolean`
   - **Response Example:**
     ```json
     {
       "status": 200,
       "index": "my-index",
       "acknowledged": true
     }
     ```
   - **How to use:**
     ```typescript
     const result = await service.createIndex();
     console.log(result.status);  // 200
     ```

### destroyIndex()

`destroyIndex(): Promise<{ status: number, index: string, acknowledged: boolean }>`

인덱스를 삭제합니다.
   - **Response:**
     - `status: number`
     - `index: string`
     - `acknowledged: boolean`
   - **Response Example:**
     ```json
     {
       "status": 200,
       "index": "my-index",
       "acknowledged": true
     }
     ```
   - **How to use:**
     ```typescript
     const result = await service.destroyIndex();
     console.log(result.status);  // 200
     ```

### refreshIndex()

`refreshIndex(): Promise<any>`

인덱스를 새로 고칩니다.
   - **Response:**
   - **Response Example:**
     ```json
     {
       "_shards": {
         "total": 10,
         "successful": 5,
         "failed": 0
       }
     }
     ```
   - **How to use:**
     ```typescript
     const result = await service.refreshIndex();
     console.log(result);
     ```

### flushIndex()

`flushIndex(): Promise<any>`

인덱스를 플러시합니다.
   - **Response:**
   - **Response Example:**
     ```json
     {
       "_shards": {
         "total": 10,
         "successful": 5,
         "failed": 0
       }
     }
     ```
   - **How to use:**
     ```typescript
     const result = await service.flushIndex();
     console.log(result);
     ```

### describe()

`describe(): Promise<{ settings: any, mappings: any }>`

인덱스의 설정 및 매핑 정보를 반환합니다.
   - **Response:**
     - `settings: any` : [Elastic6Service.prepareSettings](#settings)
     - `mappings: any` : 매핑 정보
     - **Response Example:**
     ```json
     {
       "settings": {
         "index": {
           "number_of_shards": 5,
           "number_of_replicas": 1
         }
       },
       "mappings": {
           "properties": {
             "name": {
               "type": "text"
             }
           }
         }
       }
     ```
   - **How to use:**
     ```typescript
     const description = await service.describe();
     console.log(description.settings);
     ```

### saveItem()

`saveItem(id: string, item: T, type?: string): Promise<{ _id: string, _version: number, name: string }>`

주어진 아이템을 저장합니다.
   - **Params:**
     - `id`: 아이템 ID
     - `item`: 저장할 아이템
     - `type`: 문서 타입 (default: service.options의 `docType`)
   - **Response:**
     - `_id: string`
     - `_version: number`
     - `name: string`
   - **Response Example:**
     ```json
     {
       "_id": "1",
       "_version": 1,
       "name": "example"
     }
     ```
   - **How to use:**
     ```typescript
     const savedItem = await service.saveItem('1', { name: 'example' });
     console.log(savedItem);
     ```

### pushItem()

`pushItem(item: T, type?: string): Promise<{ _id: string, _version: number, name: string }>`

타임시리즈 데이터를 푸시합니다.
   - **Params:**
     - `item`: 푸시할 아이템
     - `type`: 문서 타입 (default: service.options의 `docType`)
   - **Response:**
     - `_id: string`
     - `_version: number`
     - `name: string`
   - **Response Example:**
     ```json
     {
       "_id": "auto-generated-id",
       "_version": 1,
       "name": "example"
     }
     ```
   - **How to use:**
     ```typescript
     const pushedItem = await service.pushItem({ name: 'example' });
     console.log(pushedItem);
     ```

### readItem()

`readItem(id: string, views?: string[] | object): Promise<{ _id: string, _version: number, name: string }>`

주어진 아이템을 읽습니다.
   - **Params:**
     - `id`: 아이템 ID
     - `views`: 조회할 필드
   - **Response:**
     - `_id: string`
     - `_version: number`
     - `name: string`
   - **Response Example:**
     ```json
     {
       "_id": "1",
       "_version": 1,
       "name": "example"
     }
     ```
   - **How to use:**
     ```typescript
     const item = await service.readItem('1');
     console.log(item);
     ```

### deleteItem()

`deleteItem(id: string): Promise<{ _id: string, _version: number }>`

주어진 아이템을 삭제합니다.
   - **Params:**
     - `id`: 아이템 ID
   - **Response:**
     - `_id: string`
     - `_version: number`
   - **Response Example:**
     ```json
     {
       "_id": "1",
       "_version": 2
     }
     ```
   - **How to use:**
     ```typescript
     const deletedItem = await service.deleteItem('1');
     console.log(deletedItem);
     ```

### updateItem()

`updateItem(id: string, item: T | null, increments?: Incrementable, options?: { maxRetries?: number }): Promise<{ _id: string, _version: number, name: string }>`

주어진 아이템으로 업데이트합니다.
   - **Params:**
     - `id`: 아이템 ID
     - `item`: 업데이트할 아이템
     - `increments`: 증가할 필드 키와 증가할 value
     - `options`: 클라이언트 요청 옵션
   - **Response:**
     - `_id: string`
     - `_version: number`
     - `name: string`
   - **Response Example:**
     ```json
     {
       "_id": "1",
       "_version": 3,
       "name": "updated example"
     }
     ```
   - **How to use:**
     ```typescript
     const updatedItem = await service.updateItem('1', { name: 'updated example' });
     console.log(updatedItem);
     ```

### searchRaw()

`searchRaw(body: SearchBody, searchType?: SearchType): Promise<any>`

검색을 수행하고 원시 응답을 반환합니다.
   - **Params:**
     - `body`: 검색 쿼리
     - `searchType`: 검색 타입 (default: `query_then_fetch`)
   - **Response:**
   - **Response Example:**
     ```json
     {
       "took": 5,
       "timed_out": false,
       "_shards": {
         "total": 5,
         "successful": 5,
         "skipped": 0,
         "failed": 0
       },
       "hits": {
         "total": 1,
         "max_score": 1.0,
         "hits": [
           {
             "_index": "my-index",
             "_type": "_doc",
             "_id": "1",
             "_score": 1.0,
             "_source": {
               "name": "example"
             }
           }
         ]
       }
     }
     ```
   - **How to use:**
     ```typescript
     const searchResponse = await service.searchRaw({ query: { match_all: {} } });
     console.log(searchResponse);
     ```

### search()

`search(body: SearchBody, searchType?: SearchType): Promise<{ total: number, list: T[], last: any, aggregations: any }>`

검색을 수행하고 형식화된 응답을 반환합니다.
   - **Params:**
     - `body`: 검색 쿼리
     - `searchType`: 검색 타입 (default: `query_then_fetch`)
   - **Response:**
     - `total: number;`
     - `list: T[]`
     - `last: any`
     - `aggregations: any`
   - **How to use:**
     ```typescript
     const searchResult = await service.search({ query: { match_all: {} } });
     console.log(searchResult.total);
     console.log(searchResult.list);
     ```
   - **Response Example:**
     ```json
     {
       "total": 1,
       "list": [
         {
           "_id": "1",
           "_score": 1.0,
           "name": "example"
         }
       ],
       "last": null,
       "aggregations": null
     }
     ```

### searchAll()

`searchAll(body: SearchBody, params?: ElasticSearchAllParams): Promise<T[]>`

모든 결과를 검색하여 반환합니다. (`limit`이 -1인 경우 무제한으로 검색)

- **Params:**
  - `body`: 검색 쿼리를 정의하는 `SearchBody` 객체
  - `params`: 검색 파라미터를 정의하는 `ElasticSearchAllParams` 객체
- **Response:**
  - `T[]`: 검색된 모든 아이템의 리스트
- **How to use:**
  ```typescript
  const allItems = await service.searchAll({ query: { match_all: {} } });
  console.log(allItems);
     ```

### generateSearchResult()

`generateSearchResult(body: SearchBody, params?: ElasticSearchAllParams): AsyncGenerator<T[]>`

모든 결과를 검색하여 비동기 생성기로 반환합니다.

- **Params:**
  - `body`: 검색 쿼리를 정의하는 `SearchBody` 객체
  - `params`: 검색 파라미터를 정의하는 `ElasticSearchAllParams` 객체 (옵션)
- **Response:**
  - `AsyncGenerator<T[]>`: 검색된 아이템의 청크(chunk)를 반환하는 비동기 생성기
- **How to use:**
  ```typescript
  for await (const chunk of service.generateSearchResult({ query: { match_all: {} } })) {
      console.log(chunk);
  }
     ```

---

## Interface

### ElasticOption

ElasticSearch 서비스 초기화에 필요한 옵션을 정의하는 인터페이스입니다.

- **Properties:**
  - `endpoint: string` : ElasticSearch 서비스의 URL.
  - `indexName: string` : 인덱스 이름.
  - `docType?: string` : 문서 타입 (optional, default: `_doc`).
  - `idName?: string` : 아이디 이름 (optional).
  - `timeSeries?: boolean` : 타임시리즈 데이터 여부 (optional).
  - `version?: string` : 엔진 버전 (optional, default: `6.8`).
  - `autocompleteFields?: string[]` : 자동완성 필드 (optional).

### ElasticItem

ElasticSearch의 일반적인 아이템을 나타내는 인터페이스입니다.

- **Properties:**
  - `_id?: string` : 문서의 ID (optional).
  - `_version?: number` : 문서의 버전 (optional).
  - `_score?: number` : 문서의 점수(관련성) (optional).

### ElasticSearchAllParams

검색 작업에 사용되는 파라미터를 정의하는 인터페이스입니다.

- **Properties:**
  - `searchType?: SearchType` : 검색 유형을 지정하는 선택적 필드 (예: `query_then_fetch`).
  - `limit?: number` : 검색 결과의 제한 수 (기본값은 -1로, 무제한을 의미).
  - `retryOptions?: RetryOptions` : 검색 실패 시 재시도 옵션을 정의하는 선택적 필드.

### ParsedVersion

버전 정보를 구문 분석하여 제공하는 인터페이스입니다.

- **Properties:**
  - `engine?: EngineType` : 검색 엔진 유형 (`os` 또는 `es`).
  - `major: number` : 주 버전 번호.
  - `minor: number` : 부 버전 번호.
  - `patch: number` : 패치 버전 번호.
  - `prerelease?: string` : 프리릴리즈 레이블 (예: 'alpha', 'beta').
  - `build?: string` : 빌드 메타데이터.

### RetryOptions

검색 실패 시 재시도 동작을 제어하기 위한 옵션을 정의하는 인터페이스입니다.

- **Properties:**
  - `do?: boolean` : 재시도 여부를 지정하는 선택적 필드 (기본값은 `true`).
  - `t?: number` : 재시도 간의 대기 시간(밀리초 단위, 기본값은 5000ms).
  - `maxRetries?: number` : 최대 재시도 횟수를 지정하는 선택적 필드 (기본값은 3회).

### SearchResponse<T = any>

ElasticSearch에서의 검색 응답을 나타내는 인터페이스입니다.

- **Properties:**
  - `total: number` : 검색 결과 총 개수.
  - `list: Array<T>` : 검색된 문서 리스트.
  - `last: Array<T>` : 마지막 검색된 문서 리스트.
  - `aggregations: T` : 검색 결과의 집계 데이터.

### ElasticParams<T extends object = any>

ElasticSearch 작업에 사용되는 기본 파라미터를 정의하는 인터페이스입니다.

- **Properties:**
  - `index: string` : 인덱스 이름.
  - `id: string` : 문서 ID.
  - `body: T` : 문서의 본문 내용.
  - `type?: string` : 문서 타입 (optional).

### ElasticUpdateParams

Elasticsearch 문서 업데이트에 사용되는 파라미터를 정의하는 인터페이스입니다.

- **Properties:**
  - `index: string` : 인덱스 이름.
  - `id: string` : 문서 ID.
  - `if_seq_no: number` : 낙관적 동시성 제어를 위한 시퀀스 번호.
  - `if_primary_term: number` : 낙관적 동시성 제어를 위한 기본 용어.
  - `body: object` : 업데이트 본문 내용.

### ElasticSearchParams<T extends object = any>

ElasticSearch 검색 작업에 사용되는 파라미터를 정의하는 인터페이스입니다.

- **Properties:**
  - `index: string` : 인덱스 이름.
  - `body: T` : 검색 본문 내용.
  - `type?: string` : 문서 타입 (optional).
  - `searchType: SearchType` : 검색 유형 (예: 'query_then_fetch', 'dfs_query_then_fetch').

### Settings

`settings` 객체는 인덱스를 생성할 때 사용되는 설정을 포함합니다. 주요 구성 요소는 다음과 같습니다:

- `number_of_shards: number` : 인덱스의 기본 샤드 수
- `number_of_replicas: number` : 각 샤드의 복제본 수
- `analysis: object` : 인덱스에서 사용할 분석기, 토크나이저 등의 설정
  - `tokenizer: object`
    - `hangul: object` : 한글 분석을 위한 `seunjeon_tokenizer` 설정
    - `edge_30grams: object` : `edge_ngram` 토크나이저 설정
  - `analyzer: object`
    - `hangul: object` : 한글 분석기 설정
    - `autocomplete_case_insensitive: object` : 대소문자 구분 없이 자동 완성을 위한 분석기 설정
    - `autocomplete_case_sensitive: object` : 대소문자 구분하여 자동 완성을 위한 분석기 설정
- `mappings: object` : 인덱스의 매핑 설정
  - `dynamic_templates: object[]` : 동적 템플릿을 사용하여 특정 패턴의 필드에 대해 매핑 설정을 자동으로 적용
  - `properties: object` : 인덱스 필드와 해당 필드의 타입, 분석 방법 등을 정의
    - `@version: object`
    - `created_at: object`
    - `updated_at: object`
    - `deleted_at: object`
- `timeSeries: object` : 설정 시 추가 설정
  - `refresh_interval: string` : 인덱스 새로 고침 간격 (예: '5s')
  - `@timestamp: object` : 타임스탬프 필드
  - `ip: object` : IP 주소 필드
  - `CLEANS: string[]` : 특정 필드들을 제거 (예: `@version`, `created_at`, `updated_at`, `deleted_at`)


---

## Examples of using Elastic Service

```typescript
const service = new Elastic6Service({
    endpoint: 'https://my-endpoint',
    indexName: 'my-index',
});

await service.createIndex();
await service.saveItem('1', { name: 'example' });
const item = await service.readItem('1');
console.log(item);
```

---

## Tests
`elastic6-service.spec.js` 파일은 `Elastic6Service`의 기능을 검증하기 위한 유닛 테스트를 포함합니다.


서비스가 다양한 Elasticsearch 및 OpenSearch 버전과 호환되는지 확인하고, 예상되는 에러 처리와 기능의 정확성을 검증합니다.

### initService(version)
- **목적**: 지정된 버전의 Elasticsearch 서비스 인스턴스를 초기화합니다.
- **작동 방식**: 서비스 인스턴스를 생성하고, 주어진 엔드포인트로부터 연결을 시도합니다.
- **검증 포인트**: 서비스 인스턴스가 올바르게 초기화되었는지, 기본 설정이 예상대로 설정되었는지 확인합니다.

### setupIndex(service)
- **목적**: 테스트를 위한 인덱스를 설정합니다.
- **작동 방식**: 기존 인덱스를 찾아 제거하고 새로운 인덱스를 생성합니다.
- **검증 포인트**: 인덱스 생성과 삭제가 성공적으로 수행되었는지, 에러 핸들링이 적절히 동작하는지 확인합니다.

### basicCRUDTest(service)
- **목적**: CRUD 연산을 테스트하여 데이터 저장, 읽기, 업데이트, 삭제 기능을 검증합니다.
- **작동 방식**: 특정 아이템을 저장하고, 읽어보고, 업데이트 후 삭제합니다.
- **검증 포인트**: 각 CRUD 작업이 예상대로 동작하는지, 반환된 데이터가 정확한지 확인합니다.

### basicSearchTest(service)
- **목적**: 검색 기능을 테스트합니다.
- **작동 방식**: 복잡한 검색 쿼리를 실행하여 검색 결과를 검증합니다.
- **검증 포인트**: 검색 결과가 정확하고, 검색 쿼리가 기대한대로 실행되는지 확인합니다.

### detailedCRUDTest(service)
- **목적**: 보다 상세한 CRUD 작업을 테스트합니다.
- **작동 방식**: 다양한 유형의 데이터를 사용하여 보다 복잡한 CRUD 시나리오를 수행합니다.
- **검증 포인트**: 다양한 데이터 타입과 복잡한 상황에서도 CRUD 작업이 정확히 수행되는지 확인합니다.

### mismatchedTypeTest(service)
- **목적**: 데이터 유형 불일치 시 오류 핸들링을 테스트합니다.
- **작동 방식**: 잘못된 데이터 유형을 사용하여 CRUD 작업을 시도합니다.
- **검증 포인트**: 데이터 유형 오류가 적절히 처리되고, 오류 메시지가 명확한지 확인합니다.

### autoIndexingTest(service)
- **목적**: 자동 인덱싱과 토크나이저 설정을 테스트합니다.
- **작동 방식**: 특정 조건을 가진 데이터를 저장하고 색인 후 검색하여 색인이 예상대로 작동하는지 검사합니다.
- **검증 포인트**: 자동 인덱싱이 정확히 적용되어 검색 결과에 반영되는지 확인합니다.

### bulkDummyData(service, n, t)
- **목적**: 더미 데이터를 사용하여 대량 작업을 수행합니다.
- **작동 방식**: 주어진 데이터 세트를 n개의 청크로 나누고, 각 청크에 대해 대량 작업을 수행합니다.
- **검증 포인트**: 대량 작업이 예상대로 수행되고, 모든 데이터가 올바르게 색인되는지 확인합니다.

### totalSummaryTest(service)
- **목적**: 대규모 데이터(20,000개)를 사용하여 요약 정보를 수행합니다.
- **작동 방식**: 대량의 더미 데이터를 생성하고, 색인한 후 요약 테스트를 수행합니다.
- **검증 포인트**: 검색 결과가 정확하게 집계되고, 모든 데이터가 예상대로 검색되는지 확인합니다.

### aggregationTest(service)
- **목적**: 20,000개의 데이터를 사용하여 집계 테스트를 수행합니다.
- **작동 방식**: 집계 쿼리를 실행하여 결과를 검증합니다.
- **검증 포인트**: 각 집계 결과가 정확하게 반환되는지 확인합니다.

### searchFilterTest(service)
- **목적**: 검색 및 필터링 기능을 20,000개의 데이터로 테스트합니다.
- **작동 방식**: 다양한 조건을 사용하여 검색 쿼리를 실행하고 결과를 검증합니다.
- **검증 포인트**: 검색 및 필터링 결과가 정확하게 반환되는지 확인합니다.

### Note
테스트는 실제 서버 인스턴스에 연결이 가능할 때만 수행됩니다.  
SSH 터널링이 필요할 수 있으며, 네트워크 환경에 따라 설정이 달라질 수 있습니다.


### Version Compatibility
테스트는 다음과 같은 버전의 Elasticsearch 및 OpenSearch 서버에 대해 설정되어 있습니다:

- Elasticsearch 6.2, 7.1, 7.2, 7.10
- OpenSearch 1.1, 1.2, 2.13