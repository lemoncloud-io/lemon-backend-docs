---
layout: default
title: Hangul Service
parent: Use ElasticSearch
grand_parent: Use lemon-core
nav_order: 3
permalink: docs/lemon-core/elastic-search/hangul-service
---

# Hangul Service
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview
한글을 처리하기 위한 공통 서비스를 제공합니다.

이 서비스는 한글을 자모 시퀀스로 변환하고, 자모를 QWERTY 키보드 입력으로 변환하는 기능 등을 포함합니다.

---

## Constructor

HangulService의 인스턴스를 생성합니다.
- **How to use:**
    ```typescript
    const hangulService = new HangulService();
    ```

---

## Hello Hangul ✋

### hello()

`hello(): string`

서비스의 인스턴스를 식별하는 문자열을 반환합니다.
   - **Response:**
     - `string` : `hangul-service`
   - **How to use:**
     ```typescript
     const message = hangulService.hello();
     console.log(message);  // "hangul-service"
     ```

---

## Methods

### isHangul()

`isHangul(text: string, partial: boolean = false): boolean`

주어진 텍스트가 한글인지 여부를 확인합니다.
   - **Params:**
     - `text: string` : 입력 텍스트
     - `partial: boolean` : 텍스트에 한글이 하나라도 포함되어 있으면 true (default: false)
   - **Response:**
     - `boolean` : 한글 여부
   - **How to use:**
     ```typescript
     const isHangul = hangulService.isHangul('한글');
     console.log(isHangul);  // true
     ```

### asJamoSequence()

`asJamoSequence(text: string): string`

한글을 Compatibility Jamo 시퀀스로 분해합니다.
   - **Params:**
     - `text: string` : 입력 텍스트
   - **Response:**
     - `string` : 분해된 자모 시퀀스
   - **How to use:**
     ```typescript
     const jamoSequence = hangulService.asJamoSequence('한글');
     console.log(jamoSequence);  // "ㅎㅏㄴㄱㅡㄹ"
     ```

### asBasicJamoSequence()

`asBasicJamoSequence(text: string): string`

한글을 기본 자모 시퀀스로 분해합니다.
   - **Params:**
     - `text: string` : 입력 텍스트
   - **Response:**
     - `string` : 분해된 기본 자모 시퀀스
   - **How to use:**
     ```typescript
     const basicJamoSequence = hangulService.asBasicJamoSequence('한글');
     console.log(basicJamoSequence);  // "ㅎㅏㄴㄱㅡㄹ"
     ```

### asAlphabetKeyStokes()

`asAlphabetKeyStokes(text: string): string`

한글을 QWERTY 키보드 입력 시퀀스로 변환합니다.
   - **Params:**
     - `text: string` : 입력 텍스트
   - **Response:**
     - `string` : QWERTY 키보드 입력 시퀀스
   - **How to use:**
     ```typescript
     const keyStrokes = hangulService.asAlphabetKeyStokes('한글');
     console.log(keyStrokes);  // "gksrmf"
     ```

### asChoseongSequence()

`asChoseongSequence(text: string): string`

텍스트에서 초성만 추출합니다.
   - **Params:**
     - `text: string` : 입력 텍스트
   - **Response:**
     - `string` : 초성 시퀀스
   - **How to use:**
     ```typescript
     const choseongSequence = hangulService.asChoseongSequence('한글');
     console.log(choseongSequence);  // "ㅎㄱ"
     ```

### static isHangulChar()

`static isHangulChar(code: number): boolean`

주어진 코드가 한글 문자인지 확인합니다.
   - **Params:**
     - `code: number` : 문자 코드
   - **Response:**
     - `boolean` : 한글 문자 여부
   - **How to use:**
     ```typescript
     const isHangulChar = HangulService.isHangulChar(0xAC00);
     console.log(isHangulChar);  // true
     ```

### static isHangulSyllable()

`static isHangulSyllable(code: number): boolean`

주어진 코드가 한글 음절인지 확인합니다.
   - **Params:**
     - `code: number` : 문자 코드
   - **Response:**
     - `boolean` : 한글 음절 여부
   - **How to use:**
     ```typescript
     const isHangulSyllable = HangulService.isHangulSyllable(0xAC00);
     console.log(isHangulSyllable);  // true
     ```

### static isHangulJamo()

`static isHangulJamo(code: number): boolean`

주어진 코드가 한글 자모인지 확인합니다.
   - **Params:**
     - `code: number` : 문자 코드
   - **Response:**
     - `boolean` : 한글 자모 여부
   - **How to use:**
     ```typescript
     const isHangulJamo = HangulService.isHangulJamo(0x1100);
     console.log(isHangulJamo);  // true
     ```

### static isHangulCompatibilityJamo()

`static isHangulCompatibilityJamo(code: number): boolean`

주어진 코드가 한글 호환 자모인지 확인합니다.
   - **Params:**
     - `code: number` : 문자 코드
   - **Response:**
     - `boolean` : 한글 호환 자모 여부
   - **How to use:**
     ```typescript
     const isHangulCompatibilityJamo = HangulService.isHangulCompatibilityJamo(0x3131);
     console.log(isHangulCompatibilityJamo);  // true
     ```

---

## Constants

### CHOSEONG

- `CHOSEONG: string[]` : 초성 자모 배열
   - **How to use:**
     ```typescript
     console.log(HangulService.CHOSEONG);  // ['ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', ...]
     ```

### JUNGSEONG

- `JUNGSEONG: string[]` : 중성 자모 배열
   - **How to use:**
     ```typescript
     console.log(HangulService.JUNGSEONG);  // ['ㅏ', 'ㅐ', 'ㅑ', 'ㅒ', ...]
     ```

### JONGSEONG

- `JONGSEONG: string[]` : 종성 자모 배열
   - **How to use:**
     ```typescript
     console.log(HangulService.JONGSEONG);  // ['ㄱ', 'ㄲ', 'ㄳ', 'ㄴ', ...]
     ```

---

## Examples of using Hangul Service

```typescript
const hangulService = new HangulService();

// Check if the text is Hangul
const isHangul = hangulService.isHangul('한글');
console.log(isHangul);  // true

// Get Jamo sequence
const jamoSequence = hangulService.asJamoSequence('한글');
console.log(jamoSequence);  // "ㅎㅏㄴㄱㅡㄹ"

// Get Basic Jamo sequence
const basicJamoSequence = hangulService.asBasicJamoSequence('한글');
console.log(basicJamoSequence);  // "ㅎㅏㄴㄱㅡㄹ"

// Get QWERTY keystrokes
const keyStrokes = hangulService.asAlphabetKeyStokes('한글');
console.log(keyStrokes);  // "gksrmf"

// Get Choseong sequence
const choseongSequence = hangulService.asChoseongSequence('한글');
console.log(choseongSequence);  // "ㅎㄱ"
```