---
title: "[JS] Map과 Object의 차이점"
name: code_wave
writer: code_wave
categories: [프로그래머스, JS]
tags:
  - [코딩테스트, 프로그래머스, Lv2, javascript]

toc: true
toc_sticky: true

date: 2025-03-10
last_modified_at: 2025-03-10
---

`Map`과 일반 객체(`Object`)는 JavaScript에서 데이터를 키-값 쌍으로 저장할 때 사용됩니다. 하지만 이 두 가지는 기능과 사용 목적에서 차이가 있습니다. 아래에서 주된 차이점과 언제 각각을 사용하는 것이 적합한지에 대해 설명하겠습니다.
## 차이점
### 1. **키(Key)로 사용할 수 있는 값의 종류**
- **`Object`**:
  - 키로 사용할 수 있는 값은 **문자열**과 **심볼(Symbol)**만 가능합니다.
  - 숫자나 객체를 키로 사용할 경우, 자동으로 **문자열로 변환**됩니다.
``` javascript
    const obj = {};
    obj[42] = "value"; // 숫자 키가 문자열로 변환됨
    obj[{ foo: "bar" }] = "value2"; // 객체 키도 문자열 "[object Object]"로 변환됨

    console.log(obj); // { '42': 'value', '[object Object]': 'value2' }
```
- **`Map`**:
  - 모든 데이터 유형(Primitive 및 Object)을 키로 사용할 수 있습니다.
``` javascript
    const map = new Map();
    map.set(42, "value"); // 숫자 키 사용
    map.set({ foo: "bar" }, "value2"); // 객체 키 사용
    console.log(map); // 키가 원래 타입 그대로 유지됨
```
**결론**:
`Map`은 객체, 배열, 함수 등 **비문자열 데이터 타입**을 키로 사용하는 경우 유리합니다.
### 2. **키의 순서 보장**
- **`Object`**:
  - 키의 순서는 **정수 키(numeric key)**와 **문자열 키(non-numeric key)**가 섞여 있는 경우에 따라 달라질 수 있습니다.
    - 정수 키는 **오름차순**으로 정렬됩니다.
    - 문자열 키와 심볼 키는 **삽입 순서**를 따릅니다.
``` javascript
    const obj = {};
    obj[2] = "value2";
    obj[1] = "value1";
    obj["a"] = "valueA";
    obj["b"] = "valueB";

    console.log(obj); // { '1': 'value1', '2': 'value2', a: 'valueA', b: 'valueB' }
```
- **`Map`**:
  - **키의 삽입 순서**가 항상 유지됩니다.
``` javascript
    const map = new Map();
    map.set(2, "value2");
    map.set(1, "value1");
    map.set("a", "valueA");
    map.set("b", "valueB");

    console.log(map); // Map(4) { 2 => 'value2', 1 => 'value1', 'a' => 'valueA', 'b' => 'valueB' }
```
**결론**:
키의 삽입 순서를 반드시 보존해야 하는 경우 `Map`이 적합합니다.
### 3. **데이터 크기 확인**
- **`Object`**:
  - `Object`의 키-값 쌍 개수를 확인하려면 수동으로 확인해야 합니다.
``` javascript
    const obj = { a: 1, b: 2, c: 3 };
    console.log(Object.keys(obj).length); // 3
```
- **`Map`**:
  - `Map`은 데이터를 확인할 수 있는 **`size` 프로퍼티**를 제공합니다.
``` javascript
    const map = new Map();
    map.set("a", 1);
    map.set("b", 2);
    map.set("c", 3);

    console.log(map.size); // 3
```
**결론**:
데이터 크기를 자주 확인해야 한다면 `Map`을 사용하는 것이 편리합니다.
### 4. **성능 측면**
- **`Object`**:
  - 키를 검색하거나 추가하는 연산은 일반적으로 빠르지만, 속성을 수정하거나 열거할 때는 더 많은 작업이 필요할 수 있습니다.
  - `Object.keys()`, `Object.entries()` 등 메서드를 사용해야만 키나 값을 열거할 수 있습니다.

- **`Map`**:
  - `Map`은 데이터 검색, 추가, 삭제에서 **일정한 성능**을 제공합니다.
  - 특히 데이터가 많을 경우 `Map`은 더 효율적입니다.

**결론**:
대규모 데이터를 처리하는 경우 `Map`이 더 성능이 좋습니다.
### 5. **메서드 및 기능**
- **`Object`**:
  - 키와 값을 처리하기 위한 메서드는 많지 않으며, 일반적으로 아래와 같은 유틸리티 함수들을 활용합니다.
``` javascript
    const obj = { a: 1, b: 2 };
    console.log(Object.keys(obj)); // ['a', 'b']
    console.log(Object.values(obj)); // [1, 2]
```
- **`Map`**:
  - `Map`은 키와 값의 반복을 위한 메서드들을 기본적으로 제공합니다:
``` javascript
    const map = new Map();
    map.set("a", 1);
    map.set("b", 2);

    console.log(map.keys()); // MapIterator { 'a', 'b' }
    console.log(map.values()); // MapIterator { 1, 2 }
    console.log(map.entries()); // MapIterator { [ 'a', 1 ], [ 'b', 2 ] }
```
**결론**:
키와 값을 반복(iteration)하거나 동작이 많은 경우 `Map`을 사용하는 것이 편리합니다.
### 6. **`__proto__` 충돌 문제**
- **`Object`**:
  - 키로 `"__proto__"`를 사용하면 객체의 프로토타입과 충돌할 수 있습니다.
``` javascript
    const obj = {};
    obj["__proto__"] = "value"; // __proto__는 객체의 프로토타입과 관련된 특별한 키

    console.log(obj["__proto__"]); // [object Object] (원래 프로토타입이 출력됨)
```
- **`Map`**:
  - `Map`은 키로 모든 데이터 유형을 허용하기 때문에 충돌 문제가 없습니다.
``` javascript
    const map = new Map();
    map.set("__proto__", "value");

    console.log(map.get("__proto__")); // "value" (일반 키로 처리)
```
**결론**:
시스템 예약 키와의 충돌을 방지하려면 `Map`을 사용하는 것이 안전합니다.
## 언제 `Map`과 `Object`를 사용해야 할까?
### `Map`을 사용해야 하는 경우:
1. 키가 문자열뿐만 아니라 숫자, 객체 등 다양한 값일 때.
2. 키의 삽입 순서를 유지해야 할 때.
3. 데이터 크기를 자주 확인해야 할 때 (`size` 사용).
4. 대규모 데이터를 빠르게 처리해야 할 때.
5. 키와 값의 반복 작업이 많을 때.

### `Object`를 사용해야 하는 경우:
1. 키로 문자열 또는 심볼만 필요한 경우.
2. 데이터를 단순히 저장하고 접근하는 데 사용할 때.
3. 성능보다 일반적인 사용이 중심일 때.
4. 프로토타입 체인을 활용할 때.

## 요약

| 특징 | Object | Map |
| --- | --- | --- |
| 키(Key) 유형 | 문자열, 심볼 | 모든 데이터 유형 |
| 키 순서 유지 | 정수 키: 오름차순, 나머지: 삽입 순서 | 삽입 순서 유지 |
| 크기 확인 | `Object.keys().length` 사용 | `size` 프로퍼티 사용 |
| 성능 | 가벼운 작업에 적합 | 대규모 데이터에 유리 |
| 반복(iteration) 지원 | 제한적 | 편리한 메서드 제공 (`keys()`, `values()`, etc.) |
| 충돌 문제 | `__proto__` 충돌 위험 | 충돌 없음 |
`Map`은 더 많은 데이터 유형을 지원하고, 성능과 반복 작업에서 유리합니다. 반면, 간단한 데이터 저장 및 변환 작업에는 `Object`가 충분히 효율적입니다.
