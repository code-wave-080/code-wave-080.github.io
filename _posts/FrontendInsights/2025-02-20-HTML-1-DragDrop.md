---
title: "[HTML] Drag & Drop API"
name: code_wave
writer: code_wave
categories: [프론트엔드, HTML5]
tags:
  - [html, drag, drop]

toc: true
toc_sticky: true

date: 2025-02-20
last_modified_at: 2025-02-20
---

HTML5 Drag & Drop API는 **드래그 가능**한 요소와 **드롭 가능**한 영역을 정의하고, 드래그-드롭 작업을 제어하기 위한 여러 속성과 이벤트를 제공합니다.

---

## 주요 속성들

### 1. `draggable` 속성
- 드래그 가능 여부를 현재 요소에 설정합니다.

```html
<div draggable="true">드래그 가능 요소</div>
```

**주요 내용**
- 기본적으로 HTML에서 대부분의 요소는 드래그할 수 없습니다.
- `draggable="true"`를 설정해야 해당 요소가 드래그 가능해집니다.
- 텍스트 입력 필드 등 일부 요소는 기본적으로 드래그 가능합니다.

---

### 2. `DataTransfer` 객체와 관련된 속성
드래그 도중 데이터 전송을 처리하기 위해 사용하는 가장 중요한 객체입니다.

#### (1) 데이터 설정 및 가져오기
- **`setData(format, data)`**
  - 드래그된 데이터의 포맷과 데이터를 설정합니다.
- **`getData(format)`**
  - 설정된 데이터를 특정 포맷으로 가져옵니다.

```javascript
element.addEventListener("dragstart", (event) => {
  event.dataTransfer.setData("text/plain", "드래그된 데이터입니다.");
});

element.addEventListener("drop", (event) => {
  const data = event.dataTransfer.getData("text/plain");
  console.log(data); // "드래그된 데이터입니다."
});
```

#### (2) Drag Effect 제어
- **`effectAllowed`**
  - 드래그 시작 시 데이터 전송 효과를 설정합니다.
  - 값: `"none"`, `"copy"`, `"link"`, `"move"`, `"all"`, `"copyMove"` 등
- **`dropEffect`**
  - 드롭 시의 효과를 정의합니다.
  - 값: `"none"`, `"copy"`, `"link"`, `"move"`

```javascript
element.addEventListener("dragstart", (event) => {
  event.dataTransfer.effectAllowed = "copyMove"; // 복사와 이동 허용
});

element.addEventListener("dragover", (event) => {
  event.preventDefault(); // 드래그 도중 드롭 허용
  event.dataTransfer.dropEffect = "copy"; // 드롭 시 복사로 제한
});
```

---

## Drag & Drop 관련 이벤트

### (1) 드래그 시작 이벤트
- **`dragstart`**: 드래그가 시작될 때 발생합니다.
  - 드래그할 데이터를 `event.dataTransfer`를 통해 지정.
  - 드래그 효과(`effectAllowed`)를 설정.

```javascript
element.addEventListener("dragstart", (event) => {
  event.dataTransfer.setData("text/plain", "이 데이터를 드래그 중입니다.");
  event.dataTransfer.effectAllowed = "copyMove";
});
```

---

### (2) 드래그 진행 중 이벤트
- **`drag`**
  - 드래그 중 지속적으로 발생합니다.
    드래그 도중 애니메이션 처리나 상태를 업데이트할 때 사용.

```javascript
element.addEventListener("drag", () => {
  console.log("드래그가 진행 중...");
});
```

---

### (3) 드래그 가능한 영역 판별
- **`dragover`**
  - 요소 위에서 드래그 중일 때 발생합니다.
  - 이 이벤트 안에서 `event.preventDefault()`를 호출해야만 해당 요소가 **드롭 가능 영역**으로 인식됩니다.

```javascript
dropZone.addEventListener("dragover", (event) => {
  event.preventDefault(); // 드래그 가능 영역으로 지정
});
```

---

### (4) 드롭 처리 이벤트
- **`drop`**
  - 드래그한 데이터를 실제로 드롭했을 때 발생.
  - `event.preventDefault()` 호출 필요.
  - 드롭한 데이터는 `event.dataTransfer.getData()`를 통해 가져옵니다.

```javascript
dropZone.addEventListener("drop", (event) => {
  event.preventDefault(); // 기본 드롭 처리 방지
  const data = event.dataTransfer.getData("text/plain");
  console.log("드롭된 데이터:", data);
});
```

---

### (5) 드래그 종료 이벤트
- **`dragend`**
  - 드래그가 완료되거나 취소된 후 발생.
  - 드래그가 성공적으로 처리됐는지 여부는 `event.dataTransfer.dropEffect`를 참조.

```javascript
element.addEventListener("dragend", (event) => {
  console.log("드래그가 종료되었습니다!");
  console.log("최종 드롭 효과:", event.dataTransfer.dropEffect);
});
```

---

## Summary: 주요 속성과 이벤트 플로우
일반적인 Drag & Drop 동작 순서는 다음과 같습니다:

1. **시작 (`dragstart`)**
  - `draggable="true"` 설정
  - `event.dataTransfer.setData()`로 드래그 데이터 설정
  - `event.dataTransfer.effectAllowed`로 허용 효과 설정
2. **드래그 진행 (`drag`)**
  - 사용자와 상호작용에 따라 로직 추가 가능
3. **드래그 영역 도달 (`dragover`)**
  - 드롭 가능 영역에서 반드시 `event.preventDefault()` 호출
  - `event.dataTransfer.dropEffect`로 드롭 행동 설정
4. **드롭 (`drop`)**
  - `event.preventDefault()`로 기본 드롭 방지
  - `event.dataTransfer.getData()`로 드래그 데이터를 처리
5. **드래그 종료 (`dragend`)**
  - 드래그의 최종 상태 확인 (`event.dataTransfer.dropEffect`)

---

## 데모 예제
아래는 위 모든 내용을 간단히 종합한 예제입니다:

```html
<div id="drag-item" draggable="true" style="width: 100px; height: 100px; background-color: #3498db;">
  드래그 가능 요소
</div>

<div id="drop-zone" style="width: 200px; height: 200px; background-color: #95a5a6; margin-top: 20px;">
  여기에 드롭하세요
</div>

<script>
  const dragItem = document.getElementById("drag-item");
  const dropZone = document.getElementById("drop-zone");

  dragItem.addEventListener("dragstart", (event) => {
    event.dataTransfer.setData("text/plain", "Hello, Drop Zone!");
    event.dataTransfer.effectAllowed = "copyMove";
    console.log("Drag 시작");
  });

  dropZone.addEventListener("dragover", (event) => {
    event.preventDefault();
    event.dataTransfer.dropEffect = "copy";
    console.log("Dragging over...");
  });

  dropZone.addEventListener("drop", (event) => {
    event.preventDefault();
    const data = event.dataTransfer.getData("text/plain");
    dropZone.innerText = `드롭 된 데이터: ${data}`;
    console.log("Dropped:", data);
  });

  dragItem.addEventListener("dragend", () => {
    console.log("드래그 종료");
  });
</script>
```

---

## 마무리
HTML5 Drag & Drop API는 여러 속성과 이벤트를 제공하며, 이를 효과적으로 사용하면 드래그-드롭 작업을 유연하게 구현할 수 있습니다. 만약 추가로 궁금한 점이 있다면 편하게 물어보세요! 😊
