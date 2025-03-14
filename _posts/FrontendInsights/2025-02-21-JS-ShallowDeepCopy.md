---
title: "[JS] 얕은복사, 깊은복사"
name: code_wave
writer: code_wave
categories: [프론트엔드, JS]
tags:
  - [js, shallow, deep]

toc: true
toc_sticky: true

date: 2025-02-21
last_modified_at: 2025-02-21
---

# 얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy)에 대한 이해

프로그래밍에서 **객체(Object)** 를 복사할 때, 데이터를 복사하는 방식은 크게 **얕은 복사(Shallow Copy)** 와 **깊은 복사(Deep Copy)** 두 가지로 나눌 수 있습니다. 이 둘은 객체 내 데이터가 복사되는 방식의 차이에서 비롯되며, 프로그램에서의 동작과 메모리 관리에 큰 영향을 미칩니다.

---

## 1. 얕은 복사(Shallow Copy)

**얕은 복사**는 객체의 "1단계" 데이터만 복사합니다. 객체의 프로퍼티 값이 **값(primitive)** 일 경우, 값을 복사합니다. 하지만 프로퍼티 값이 **참조(reference)** 되는 또 다른 객체일 경우, 그 객체의 메모리 주소(참조)만 복사합니다. 즉, 내부의 중첩된 객체는 여전히 원본 객체와 **같은 참조**를 공유합니다.

### 특징
- 최상위 레벨의 프로퍼티들만 복사됨.
- 중첩 객체의 참조만 가져오기 때문에 **원본과 복사본이 중첩 객체를 공유**.
- 한 쪽에서 중첩 객체를 수정하면 다른 쪽에도 영향을 미침.

### 예제 (JavaScript)
```javascript
const original = {
  name: "Alice",
  address: { city: "Seoul", zip: "12345" }
};

const shallowCopy = { ...original }; // 얕은 복사

shallowCopy.name = "Bob"; // 최상위 값 수정 (독립적)
shallowCopy.address.city = "Busan"; // 중첩 객체 수정 (공유됨)

console.log(original.name); // "Alice" (독립적)
console.log(original.address.city); // "Busan" (공유됨)
```

---

## 2. 깊은 복사(Deep Copy)

**깊은 복사**는 객체를 완전히 독립적으로 복사합니다. 객체 안의 중첩된 객체까지 모두 재귀적으로 복사하여 **원본 객체와 복사본이 전혀 다른 메모리 주소를 가지도록** 만듭니다. 따라서, 원본 쪽을 수정하더라도 복사본에는 아무런 영향을 미치지 않습니다.

### 특징
- 객체의 모든 레벨 데이터를 완전히 복사.
- 중첩된 객체도 새롭게 생성되며, 원본과 독립적으로 동작.
- 메모리는 더 많이 소모되지만, 데이터가 독립적이므로 안전.

### 예제 (JavaScript)
```javascript
const original = {
  name: "Alice",
  address: { city: "Seoul", zip: "12345" }
};

// 깊은 복사
const deepCopy = JSON.parse(JSON.stringify(original));

deepCopy.name = "Bob"; // 최상위 값 수정 (독립적)
deepCopy.address.city = "Busan"; // 중첩 객체 수정 (독립적)

console.log(original.name); // "Alice" (독립적)
console.log(original.address.city); // "Seoul" (독립적)
```

---

## 3. 얕은 복사 vs 깊은 복사

| **특징**           | **얕은 복사(Shallow Copy)**                          | **깊은 복사(Deep Copy)**                             |
|--------------------|---------------------------------------------------|---------------------------------------------------|
| **복사 대상**       | 최상위 값만 복사, 중첩 객체는 참조를 복사                 | 모든 데이터(값 혹은 중첩 객체)를 재귀적으로 복사             |
| **데이터 독립성**   | 중첩 객체는 원본과 독립적이지 않음                       | 완전한 독립성 보장                                  |
| **속도**            | 메모리 및 CPU 자원을 덜 소모                            | 재귀적으로 모든 데이터를 복사하므로 더 느림               |
| **안전성**          | 원본/복사본 양쪽 모두 영향을 줄 수 있음                   | 원본과 복사본이 서로 완전히 독립적임                    |

---

## 4. 다른 프로그래밍 언어에서의 구현

### JavaScript에서 얕은 복사의 방법:
1. `Object.assign()`:
   ```javascript
   const shallowCopy = Object.assign({}, original);
   ```
2. **스프레드 연산자(...)**:
   ```javascript
   const shallowCopy = { ...original };
   ```

### JavaScript에서 깊은 복사의 방법:
1. **JSON 객체 사용**:
   ```javascript
   const deepCopy = JSON.parse(JSON.stringify(original));
   ```
   단, 이 방법은 **Date, 함수, undefined** 같은 데이터는 복사하지 못하므로 제한적입니다.

2. **Lodash 라이브러리**:
   ```javascript
   const _ = require("lodash");
   const deepCopy = _.cloneDeep(original);
   ```

### Python에서:
- **얕은 복사**: `copy.copy()`
- **깊은 복사**: `copy.deepcopy()`

```python
import copy

original = {"name": "Alice", "address": {"city": "Seoul", "zip": "12345"}}

# 얕은 복사
shallow_copy = copy.copy(original)

# 깊은 복사
deep_copy = copy.deepcopy(original)
```

---

## 5. 언제 어떤 방식을 사용할까?

- **얕은 복사**를 사용:
  - 데이터 구조가 간단할 때.
  - 중첩 객체가 없는 경우.
  - 참조를 공유하더라도 문제가 없을 때.

- **깊은 복사**를 사용:
  - 중첩된 객체가 많은 복잡한 데이터 구조를 다룰 때.
  - 원본 데이터와 복사본의 완전한 독립성이 중요할 때.
  - 데이터 변경으로 인한 **부작용(side effect)** 을 방지하고 싶을 때.

---

**결론적으로** 데이터를 효율적으로 관리하기 위해 얕은 복사와 깊은 복사의 차이를 명확히 이해하고, 상황에 맞는 적절한 복사 방식을 사용하는 것이 중요합니다.
