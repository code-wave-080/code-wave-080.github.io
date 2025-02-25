---
# Header
title: "[JS] 트리쉐이킹(Tree shaking)"
name: code_wave
writer: code_wave
categories: [프론트엔드, JS]
tags:
  - [js, treeShaking]

toc: true
toc_sticky: true

date: 2025-02-25
last_modified_at: 2025-02-25
---

프론트엔드 개발에서, 특히 JavaScript 애플리케이션을 개발할 때 "최적화"는 빠른 웹 페이지를 만드는 데 매우 중요한 요소 중 하나입니다. 오늘 소개할 **트리 쉐이킹(Tree Shaking)**은 이러한 최적화 기법 중 하나로 현대 웹 개발에서 빼놓을 수 없는 기술입니다. 쉽게 말하면 **필요 없는 코드를 제거해서 파일 크기를 줄이는 과정**입니다.

그럼 트리 쉐이킹이 정확히 무엇인지, 왜 중요한지, 그리고 어떻게 사용하는지에 대해 쉽게 알아보겠습니다.

---

## 1. 트리 쉐이킹이란?

트리 쉐이킹(Tree Shaking)은 애플리케이션 코드에서 **사용되지 않는(dead code)** 코드를 제거하는 최적화 기술입니다. 이름 그대로 **"나무(Tree)를 흔들어서 불필요한 가지(branches)를 떨어뜨린다"**는 비유에서 유래한 이름입니다.

주로 모듈 번들러(예: Webpack, Rollup)가 최적화를 위해 사용하며, 결과적으로 애플리케이션의 **번들 크기를 줄이고 성능을 향상**시킵니다.

###  트리 쉐이킹의 핵심 원리:
- **ES6(ES2015) 모듈 시스템**을 기반으로 동작 (즉, `import`와 `export` 사용 필수).
- 사용되지 않는 코드(Dead Code)를 컴파일 단계에서 감지하고 제거.

---

## 2. 트리 쉐이킹이 왜 중요한가?

트리 쉐이킹은 현대 웹 애플리케이션에서 다음과 같은 이유로 매우 중요한 역할을 합니다:

### ⚡ **번들 크기 감소**
- 대부분의 프로젝트는 필요 없는 코드도 포함된 상태로 배포되는 경우가 많습니다. 이를 제거하면 전반적인 자바스크립트 파일 크기를 줄일 수 있습니다.
- **예시**:
  ```javascript
  // utilities.js
  export function add(a, b) {
    return a + b;
  }

  export function subtract(a, b) {
    return a - b;
  }

  // main.js
  import { add } from './utilities.js';

  console.log(add(5, 3)); // 'subtract' 함수는 사용되지 않음
  ```
  위 코드는 실제로 `add` 함수만 사용하므로 `subtract`는 번들에서 제거될 수 있습니다.

### ⚡ **더 빠른 로딩 시간**
- 파일 크기가 줄어드니 네트워크에서 전송 속도가 빨라져 웹 페이지 로딩 속도도 빨라집니다.

### ⚡ **사용되지 않는 코드 제거**
- 무분별하게 큰 라이브러리를 사용할 때도 실제로 사용되는 코드만 남기기 때문에 최적화가 가능해집니다.

---

## 3. 트리 쉐이킹은 어떻게 작동할까?

트리 쉐이킹은 크게 다음 두 가지 과정을 바탕으로 작동합니다:

### 1️⃣ **ES6 모듈 분석**
트리 쉐이킹은 반드시 ES6(ECMAScript 2015) 모듈 시스템(`import`와 `export`)만을 지원합니다. 이를 통해 정적 코드 분석이 가능하며, 어떤 코드가 사용되고 어떤 코드가 사용되지 않는지 정확히 판단할 수 있습니다.

```javascript
// math.js
export const multiply = (a, b) => a * b;
export const divide = (a, b) => a / b;

// main.js
import { multiply } from './math.js';

console.log(multiply(2, 3)); // divide는 사용되지 않으므로 제거
```

### 2️⃣ **Dead Code Elimination (DCE)**
번들러는 정적 분석을 통해 사용되지 않는 코드(Dead Code)를 제거합니다. 이 작업은 보통 **번들러(Webpack, Rollup)**와 **최적화 도구(Terser, UglifyJS)**가 수행합니다.

---

## 4. 트리 쉐이킹을 사용하는 방법

트리 쉐이킹을 활용하려면 다음과 같은 환경을 갖춰야 합니다.

### 1️⃣ **ES6 모듈 규칙을 따르기**
CommonJS(`module.exports`, `require()`) 대신 ES6 모듈(`import`, `export`)을 사용해야 합니다. 트리 쉐이킹은 정적 분석을 기반으로 하기 때문에 동적 `require()`는 분석하지 못합니다.

```javascript
// ES6 Modules
export const greet = () => console.log("hello");

// CommonJS (지원 불가)
module.exports = {
  greet: () => console.log("hello"),
};
```

### 2️⃣ **Webpack 설정**
Webpack은 트리 쉐이킹을 기본적으로 지원하지만, 몇 가지 설정이 필요합니다.

#### Webpack 설정 시 트리 쉐이킹 활성화
- **`mode: 'production'`** 설정
- **Tree Shaking 활성화 플래그**(`optimization.usedExports`)

```javascript
// webpack.config.js
module.exports = {
  mode: 'production',
  optimization: {
    usedExports: true, // 사용되지 않은 코드를 제거
  },
};
```

### 3️⃣ **라이브러리도 트리 쉐이킹 가능?**
- Modern JavaScript 라이브러리 대부분은 트리 쉐이킹을 지원합니다.
- 그러나 **default export는 트리 쉐이킹에 문제가 생길 수 있음**.

#### Named Export 사용 권장:
```javascript
// Recommended
export const add = (a, b) => a + b;

// Not Recommended (Default Export)
export default (a, b) => a + b;
```

---

## 5. 트리 쉐이킹의 한계와 주의점

###  **Dynamic Import**
- 트리 쉐이킹은 정적 분석(Static Analysis)을 바탕으로 합니다. 코드가 동적으로 불러오는 경우(`require()`, 동적 `import()`)에는 작동하지 않습니다.

###  **Side Effects**
어떤 모듈은 import 시 그 자체로 실행되는 부작용(side effect)을 가질 수 있습니다. 이를 방지하려면 `package.json`의 **`sideEffects`** 필드를 명시해야 합니다.

#### 예시: `sideEffects` 설정
```json
// package.json
{
  "sideEffects": false
}
```

위와 같이 설정하면 부작용이 없는 코드만 포함합니다. 부작용이 있는 경우 해당 파일이나 디렉터리를 명시적으로 설정해야 합니다:
```json
{
  "sideEffects": ["./polyfill.js"]
}
```

---

## 6. 결론

트리 쉐이킹(Tree Shaking)은 코드에서 사용하지 않는 부분을 제거하여 **애플리케이션의 파일 크기를 줄이고 로딩 속도를 향상시키는 강력한 최적화 기법**입니다. 현대적인 개발 도구(Webpack, Rollup)를 활용하면 어렵지 않게 트리 쉐이킹을 적용할 수 있습니다.

### ✨ 트리 쉐이킹의 핵심 요약:
1. ES6 모듈(`import`와 `export`) 사용.
2. Webpack 또는 Rollup 같은 번들러를 통해 최적화.
3. `sideEffects`를 고려해 불필요한 코드 제거.

----

트리 쉐이킹을 프로젝트에 적용하여 가볍고 빠른 웹 애플리케이션을 만들어보세요! 
