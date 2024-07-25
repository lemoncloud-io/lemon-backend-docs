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


`constructor(options: Elastic6Option)`


Elastic6Service 클래스를 사용하여 Elasticsearch 클라이언트 인스턴스를 생성할 수 있습니다. 객체를 생성할 때 constructor는 자동으로 호출되므로 사용자가 직접 호출할 필요는 없습니다.
- **Params:**
    - options: [Elastic6Option](#elastic6option)
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
     - 예시: `elastic6-service:indexName:6.8`
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
   - **How to use:**
     ```typescript
     const client = service.client;
     ```

### get options()

`options(): Elastic6Option`

서비스의 옵션을 반환합니다.
   - **Response:**
     - [Elastic6Option](#elastic6option)
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
     - 예시: `6.8`
   - **How to use:**
     ```typescript
     const version = service.version;
     console.log(version);  // 6.8
     ```

### get isOpenSearch()

`isOpenSearch(): boolean`

검색 엔진이 OpenSearch 기반인지 판단합니다.
   - **Response:**
     - `boolean`
     - 예시: false
   - **How to use:**
     ```typescript
     const isOpenSearch = service.isOpenSearch;
     console.log(isOpenSearch);  // false
     ```

---

## Methods

### getVersion()

`getVersion(): Promise<{ major: number; minor: number }>`

ElasticSearch 서버의 버전을 반환합니다.
   - **Response:**
     - `major: number`
     - `minor: number`
   - **Response Example:**
     ```json
     {
       "major": 6,
       "minor": 8
     }
     ```
   - **How to use:**
     ```typescript
     const version = await service.getVersion();
     console.log(version.major);  // 6
     console.log(version.minor);  // 8
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

주어진 아이템을 업데이트합니다.
   - **Params:**
     - `id`: 아이템 ID
     - `item`: 업데이트할 아이템
     - `increments`: 증가할 필드
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

---

## Interface

### Elastic6Option

- `endpoint: string` : ElasticSearch 서비스의 URL
- `indexName: string` : 인덱스 이름
- `docType?: string` : 문서 타입 (optional, default: `_doc`)
- `idName?: string` : 아이디 이름 (optional)
- `timeSeries?: boolean` : 타임시리즈 데이터 여부 (optional)
- `version?: string` : 엔진 버전 (optional, default: `6.8`)
- `autocompleteFields?: string[]` : 자동완성 필드 (optional)

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
