---
layout: default
title: Elastic Query Service
parent: Use ElasticSearch
grand_parent: Use lemon-core
nav_order: 2
permalink: docs/lemon-core/elastic-search/elastic-query-service
---

# Elastic Query Service
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
ElasticSearch 6.x부터 7.x를 위한 쿼리 서비스를 제공합니다.

이 서비스는 ID 기반의 간단한 검색을 지원하며, Autocomplete 검색을 위한 기능도 포함하고 있습니다.

---

## Constructor

Elastic6QueryService의 인스턴스를 생성합니다.
- **Params:**
    - `service: Elastic6Service` : [Elastic6Service](#elastic6service)
- **How to use:**
    ```typescript
    const queryService = new Elastic6QueryService(service);
    ```

---

## Hello Elastic ✋

### hello()

`hello(): string`

서비스의 인스턴스를 식별하는 문자열을 반환합니다.
   - **Response:**
     - `string` : `elastic6-query-service:${this.options.indexName}`
   - **How to use:**
     ```typescript
     const message = queryService.hello();
     console.log(message);  // "elastic6-query-service:my-index"
     ```

---

## Methods

### queryAll()

`queryAll(id: string, limit?: number, isDesc?: boolean): Promise<QueryResult<T>>`

주어진 ID를 기반으로 모든 결과를 조회합니다.
   - **Params:**
     - `id: string` : 조회할 ID
     - `limit?: number` : 결과 제한 수 (optional)
     - `isDesc?: boolean` : 내림차순 여부 (optional)
   - **Response:**
     - `Promise<QueryResult<T>>` : [QueryResult](#queryresult)
   - **How to use:**
     ```typescript
     const results = await queryService.queryAll('123');
     console.log(results);
     ```

### searchSimple()

`searchSimple(param: SimpleSearchParam): Promise<QueryResult<T>>`

간단한 검색을 수행합니다.
   - **Params:**
     - `param: SimpleSearchParam` : [SimpleSearchParam](#simplesearchparam)
   - **Response:**
     - `Promise<QueryResult<T>>` : [QueryResult](#queryresult)
   - **How to use:**
     ```typescript
     const results = await queryService.searchSimple({ stock: '>1' });
     console.log(results);
     ```

### search()

`search(body: SearchBody, searchType?: SearchType): Promise<any>`

원시 쿼리를 사용하여 검색을 수행합니다.
   - **Params:**
     - `body: SearchBody` : [SearchBody](#searchbody)
     - `searchType?: SearchType` : [SearchType](#searchtype)
   - **Response:**
     - `Promise<any>` : 검색 결과
   - **How to use:**
     ```typescript
     const results = await queryService.search({ query: { match_all: {} } });
     console.log(results);
     ```

### asQueryResult()

`asQueryResult(body: SearchBody, res: any): QueryResult<T>`

검색 결과를 `QueryResult` 형식으로 변환합니다.
   - **Params:**
     - `body: SearchBody` : [SearchBody](#searchbody)
     - `res: any` : 검색 결과
   - **Response:**
     - `QueryResult<T>` : [QueryResult](#queryresult)
   - **How to use:**
     ```typescript
     const queryResult = queryService.asQueryResult(body, res);
     console.log(queryResult);
     ```

### asSearchBody()

`asSearchBody(param: AutocompleteSearchParam): SearchBody`

`AutocompleteSearchParam`을 `SearchBody`로 변환합니다.
   - **Params:**
     - `param: AutocompleteSearchParam` : [AutocompleteSearchParam](#autocompletesearchparam)
   - **Response:**
     - `SearchBody` : [SearchBody](#searchbody)
   - **How to use:**
     ```typescript
     const searchBody = queryService.asSearchBody({ $query: { name: 'example' } });
     console.log(searchBody);
     ```

### searchAutocomplete()

`searchAutocomplete(param: AutocompleteSearchParam): Promise<QueryResult<T>>`

`Search-as-You-Type` 방식으로 아이템을 검색합니다.
   - **Params:**
     - `param: AutocompleteSearchParam` : [AutocompleteSearchParam](#autocompletesearchparam)
   - **Response:**
     - `Promise<QueryResult<T>>` : [QueryResult](#queryresult)
   - **How to use:**
     ```typescript
     const results = await queryService.searchAutocomplete({ $query: { name: 'example' } });
     console.log(results);
     ```

### buildQueryBody()

`buildQueryBody(param: SimpleSearchParam): SearchBody`

검색 파라미터에서 쿼리 파라미터를 생성합니다.
   - **Params:**
     - `param: SimpleSearchParam` : [SimpleSearchParam](#simplesearchparam)
   - **Response:**
     - `SearchBody` : [SearchBody](#searchbody)
   - **How to use:**
     ```typescript
     const queryBody = queryService.buildQueryBody({ stock: '>1' });
     console.log(queryBody);
     ```

---

## Interface

### Elastic6Service

ElasticSearch 6.x 및 7.x를 위한 공통 서비스
- **How to use:**
    ```typescript
    const service = new Elastic6Service({
        endpoint: 'https://my-endpoint',
        indexName: 'my-index',
    });
    ```

### GeneralItem

일반 아이템 인터페이스
- **How to use:**
    ```typescript
    interface GeneralItem {
        _id?: string;
        _score?: number;
    }
    ```

### QueryResult

검색 결과 인터페이스
- **How to use:**
    ```typescript
    interface QueryResult<T> {
        list: T[];
        total: number;
        last?: any;
        aggregations?: any;
    }
    ```

### SimpleSearchParam

간단한 검색 파라미터 인터페이스
- **How to use:**
    ```typescript
    interface SimpleSearchParam {
        [key: string]: any;
    }
    ```

### AutocompleteSearchParam

자동완성 검색 파라미터 인터페이스
- **How to use:**
    ```typescript
    interface AutocompleteSearchParam {
        $query: { [key: string]: any };
        $filter?: { [key: string]: any };
        $limit?: number;
        $page?: number;
        $highlight?: boolean | string;
    }
    ```

### SearchBody

검색 바디 인터페이스
- **How to use:**
    ```typescript
    interface SearchBody {
        query: any;
        size?: number;
        from?: number;
        [key: string]: any;
    }
    ```

### SearchType

검색 타입 인터페이스
- **How to use:**
    ```typescript
    type SearchType = 'query_then_fetch' | 'dfs_query_then_fetch';
    ```

---

## Examples of using Elastic6 Query Service

```typescript
const queryService = new Elastic6QueryService(service);

// Query all items by ID
const results = await queryService.queryAll('123');
console.log(results);

// Perform a simple search
const simpleSearchResults = await queryService.searchSimple({ stock: '>1' });
console.log(simpleSearchResults);

// Perform a raw search
const rawSearchResults = await queryService.search({ query: { match_all: {} } });
console.log(rawSearchResults);

// Convert to QueryResult
const queryResult = queryService.asQueryResult(body, res);
console.log(queryResult);

// Convert AutocompleteSearchParam to SearchBody
const searchBody = queryService.asSearchBody({ $query: { name: 'example' } });
console.log(searchBody);

// Perform autocomplete search
const autocompleteResults = await queryService.searchAutocomplete({ $query: { name: 'example' } });
console.log(autocompleteResults);
```