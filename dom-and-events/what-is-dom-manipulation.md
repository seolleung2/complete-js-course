# DOM (Document Object Model) 이해하기

## DOM의 기본 개념

- DOM은 HTML 문서의 구조화된 표현(Structured representation)입니다
- 웹 페이지와 JavaScript 코드 사이의 인터페이스 역할을 합니다
- 브라우저가 HTML을 로드할 때 자동으로 DOM 트리를 생성합니다
- DOM은 프로그래밍 언어와 독립적이며, 플랫폼/언어 중립적입니다
- DOM은 문서를 논리 트리로 표현하며, 각 노드는 객체로 취급됩니다

## DOM의 핵심 특징

### DOM 조작의 비용

- **리플로우(Reflow)**: DOM 요소의 크기나 위치가 변경될 때 발생
  - 변경된 요소와 그 자식, 부모 요소들의 위치와 크기를 재계산
  - 레이아웃 단계를 다시 수행하므로 매우 비용이 큼
- **리페인트(Repaint)**: 가시성이나 색상 등 스타일 변경 시 발생
  - 리플로우보다는 비용이 적지만, 여전히 부하가 발생
- **해결방안**:
  - 여러 변경사항을 일괄 처리
  - DocumentFragment 사용
  - 가상 DOM 활용 (React 등)

### 이벤트 전파 메커니즘

- **캡처링 단계(Capturing Phase)**:
  - 이벤트가 최상위 요소에서 시작하여 타겟 요소까지 내려오는 단계
  - `addEventListener`의 세 번째 인자를 `true`로 설정하여 캡처링 이벤트 감지

```javascript
element.addEventListener("click", handler, true);
```

- **버블링 단계(Bubbling Phase)**:
  - 이벤트가 타겟 요소에서 시작하여 최상위 요소까지 올라가는 단계
  - 대부분의 이벤트 핸들링이 이 단계에서 발생
  - `event.stopPropagation()`으로 버블링 중단 가능

```javascript
element.addEventListener("click", function (e) {
  e.stopPropagation(); // 이벤트 전파 중단
});
```

이벤트 전파는 HTML의 계층 구조를 따라 진행됩니다. 다음 예시로 살펴보겠습니다:

```html
<div class="grandparent">
  <div class="parent">
    <button class="child">클릭하세요</button>
  </div>
</div>
```

```javascript
// 각 요소에 이벤트 리스너 추가
const grandparent = document.querySelector(".grandparent");
const parent = document.querySelector(".parent");
const child = document.querySelector(".child");

// 캡처링 단계 이벤트 리스너
grandparent.addEventListener(
  "click",
  (e) => {
    console.log("캡처링 - Grandparent");
  },
  true
);

parent.addEventListener(
  "click",
  (e) => {
    console.log("캡처링 - Parent");
  },
  true
);

child.addEventListener(
  "click",
  (e) => {
    console.log("캡처링 - Child");
  },
  true
);

// 버블링 단계 이벤트 리스너
grandparent.addEventListener("click", (e) => {
  console.log("버블링 - Grandparent");
});

parent.addEventListener("click", (e) => {
  console.log("버블링 - Parent");
});

child.addEventListener("click", (e) => {
  console.log("버블링 - Child");
});
```

버튼 클릭 시 콘솔 출력 순서:

```
캡처링 - Grandparent
캡처링 - Parent
캡처링 - Child
버블링 - Child
버블링 - Parent
버블링 - Grandparent
```

- **캡처링 단계**: 이벤트가 최상위 요소(grandparent)에서 시작하여 아래로 전파
- **타겟 단계**: 실제 클릭된 요소(child)에 도달
- **버블링 단계**: 다시 아래에서 위로 전파

실제 활용 예시:

```javascript
// 이벤트 위임(Event Delegation) 패턴
document.querySelector("ul").addEventListener("click", function (e) {
  if (e.target.tagName === "LI") {
    // li 요소가 클릭되었을 때만 처리
    console.log("리스트 아이템 클릭:", e.target.textContent);
  }
});
```

## DOM의 주요 특징

1. **트리 구조**

   - Document: DOM 트리의 진입점
   - Element nodes: HTML 요소들
   - Text nodes: 텍스트 컨텐츠
   - Attribute nodes: 요소의 속성들

2. **DOM과 JavaScript의 관계**
   - DOM은 JavaScript의 일부가 아닙니다
   - Web API의 일부로, JavaScript에서 접근 가능한 인터페이스를 제공합니다

## DOM 조작의 주요 메서드

```javascript
// 요소 선택
document.querySelector(".className"); // 단일 요소 선택
document.querySelectorAll(".className"); // 모든 일치하는 요소 선택

// 콘텐츠 조작
element.textContent; // 텍스트 내용 읽기/쓰기
element.value; // 입력 요소의 값 읽기/쓰기

// 스타일 조작
element.style.propertyName; // CSS 속성 직접 조작
```

## Guess My Number 게임 구현

image.png

### 게임 로직

1. 1부터 20 사이의 랜덤 숫자 생성
2. 사용자 입력값과 랜덤 숫자 비교
3. 점수 시스템 구현 (초기 점수: 20)
4. 최고 점수 기록 유지

### 주요 기능

```javascript
// 1. 상태 관리 변수 정의
let secretNumber = Math.trunc(Math.random() * 20) + 1;
let score = 20;
let highscore = 0;

// 2. DOM 요소 선택 - 모든 필요한 요소 선택
const numberEl = document.querySelector(".number");
const guessEl = document.querySelector(".guess");
const messageEl = document.querySelector(".message");
const scoreEl = document.querySelector(".score");
const highscoreEl = document.querySelector(".highscore");

// 3. 메시지 표시 함수 추가 (코드 재사용성 향상)
const displayMessage = function (message) {
  messageEl.textContent = message;
};

// 4. 클릭 이벤트 핸들러 구현
document.querySelector(".check").addEventListener("click", function () {
  const guess = Number(guessEl.value);

  // 가이드라인의 게임 로직을 실제 구현
  // 4.1 입력값 검증
  if (!guess) {
    displayMessage("⛔ 숫자를 입력하세요!");
  }
  // 4.2 정답 확인
  else if (guess === secretNumber) {
    displayMessage("🎉 정답입니다!");
    numberEl.textContent = secretNumber;
    document.body.style.backgroundColor = "#60b347";
    numberEl.style.width = "30rem";

    // 최고 점수 업데이트
    if (score > highscore) {
      highscore = score;
      highscoreEl.textContent = highscore;
    }
  }
  // 4.3 오답 처리
  else if (guess !== secretNumber) {
    if (score > 1) {
      displayMessage(
        guess > secretNumber ? "📈 너무 높아요!" : "📉 너무 낮아요!"
      );
      score--;
      scoreEl.textContent = score;
    } else {
      displayMessage("💥 게임 오버!");
      scoreEl.textContent = 0;
    }
  }
});
```
