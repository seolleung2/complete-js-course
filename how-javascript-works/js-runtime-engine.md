## 자바스크립트 엔진과 런타임

### 1. 자바스크립트 엔진 기초

자바스크립트는 싱글 스레드 언어입니다. 이는 한 번에 하나의 작업만 처리할 수 있다는 의미입니다.

> 🤔 **싱글 스레드란?**
>
> 식당에 요리사가 한 명만 있다고 상상해보세요. 이 요리사는 한 번에 하나의 요리만 만들 수 있습니다. 자바스크립트도 마찬가지로 한 번에 하나의 작업만 처리할 수 있습니다.

#### 1.1 주요 엔진들

- **V8**: Google Chrome, Node.js에서 사용
- **SpiderMonkey**: Firefox에서 사용
- **JavaScriptCore**: Safari에서 사용

#### 1.2 핵심 구성 요소

##### Memory Heap (메모리 힙)

데이터를 저장하는 창고라고 생각하면 됩니다.

```javascript
// 이런 데이터들이 Heap에 저장됩니다
const user = {
  name: "철수",
  age: 25,
};

const numbers = [1, 2, 3, 4, 5];
```

##### Call Stack (호출 스택)

실행할 코드를 순서대로 쌓아두는 곳입니다.

> 🍽️ **호출 스택 비유**
>
> 접시를 쌓는 것을 생각해보세요. 새로운 접시는 항상 위에 쌓고, 접시를 치울 때는 항상 위에서부터 치웁니다.
> 코드 실행도 마찬가지입니다!

```javascript
function makeBreakfast() {
  toastBread(); // 1. 빵을 굽고
  fryEggs(); // 2. 계란을 부치고
  pourCoffee(); // 3. 커피를 따릅니다
}

makeBreakfast();

// 호출 스택:
// 1. makeBreakfast
// 2. toastBread
// 3. fryEggs
// 4. pourCoffee
```

### 2. 이벤트 루프와 비동기 처리

#### 2.1 왜 이벤트 루프가 필요한가요?

자바스크립트는 싱글 스레드이지만, 여러 작업을 동시에 처리하는 것처럼 보입니다. 이것이 가능한 이유가 바로 이벤트 루프 때문입니다.

> 🏃 **이벤트 루프 이해하기**
>
> 패스트푸드점 직원을 생각해보세요. 주문을 받으면서(동기 작업), 주방에 요리를 요청하고(비동기 작업 시작),
> 다른 손님의 주문을 받습니다(다음 작업 처리). 요리가 완성되면 알림을 받고 손님에게 전달합니다(비동기 작업 완료).
>
> 자바스크립트의 이벤트 루프도 이와 같이 작업을 관리합니다!

#### 2.2 이벤트 루프 동작 방식

##### 주요 구성 요소

1. Call Stack: 현재 실행 중인 작업 (주문 받는 카운터)
2. Web APIs: 시간이 걸리는 작업 처리 (주방)
3. Callback Queue: 완료된 작업 대기열 (완성된 음식 대기 선반)
4. Event Loop: 전체 작업 관리 (매장 관리자)

```javascript
console.log("손님 입장!"); // 1️⃣ 즉시 처리

setTimeout(() => {
  console.log("주문하신 음식이 완성됐습니다!"); // 3️⃣ 3초 후 처리
}, 3000);

console.log("주문 접수 완료!"); // 2️⃣ 즉시 처리

// 출력:
// 손님 입장!
// 주문 접수 완료!
// (3초 후)
// 주문하신 음식이 완성됐습니다!
```

##### 이벤트 루프와 태스크 처리 순서

마이크로태스크(Promise 등)는 매크로태스크(setTimeout 등)보다 먼저 처리됩니다.

```javascript
console.log("1. 주문 시작");

setTimeout(() => {
  console.log("4. 음료 제조 완료");
}, 0);

Promise.resolve().then(() => {
  console.log("2. 결제 완료");
});

console.log("3. 주문 접수 완료");

// 출력:
// 1. 주문 시작
// 3. 주문 접수 완료
// 2. 결제 완료
// 4. 음료 제조 완료
```

#### 2.3 실제 사용 예시

##### 1. 사용자 입력 처리

```javascript
// 나쁜 예 - 메인 스레드 차단
function handleClick() {
  for (let i = 0; i < 1000000000; i++) {
    // 무거운 작업
  }
  updateUI(); // UI가 멈춤
}

// 좋은 예 - 비동기 처리
function handleClick() {
  Promise.resolve().then(() => {
    // 무거운 작업을 청크로 나누기
    processDataInChunks(data, updateUI);
  });
}
```

##### 2. API 호출 처리

```javascript
// 실제 서비스에서 자주 사용되는 패턴
async function fetchUserData() {
  try {
    console.log("데이터 요청 시작...");

    const response = await fetch("api/users");
    const data = await response.json();

    console.log("데이터 도착!", data);
  } catch (error) {
    console.error("에러 발생:", error);
  }
}

// 사용자 경험 개선
function showLoadingUI() {
  const button = document.querySelector("#fetchButton");
  button.disabled = true;
  button.textContent = "로딩 중...";
}

function hideLoadingUI() {
  const button = document.querySelector("#fetchButton");
  button.disabled = false;
  button.textContent = "데이터 가져오기";
}

// 실제 사용
async function handleFetchClick() {
  showLoadingUI();
  await fetchUserData();
  hideLoadingUI();
}
```

### 3. 성능 최적화 팁

#### 3.1 작업 분할하기

```javascript
// 나쁜 예 - 한 번에 처리
function processItems(items) {
  items.forEach((item) => heavyProcessing(item));
}

// 좋은 예 - 작업 분할
async function processItemsInChunks(items) {
  const CHUNK_SIZE = 5;

  for (let i = 0; i < items.length; i += CHUNK_SIZE) {
    const chunk = items.slice(i, i + CHUNK_SIZE);

    // 다음 프레임까지 대기
    await new Promise((resolve) => requestAnimationFrame(resolve));

    chunk.forEach((item) => heavyProcessing(item));

    // 진행률 업데이트
    updateProgress((i / items.length) * 100);
  }
}
```

#### 3.2 비동기 작업 최적화

```javascript
// 나쁜 예 - 순차적 실행
async function fetchUserData() {
  const user = await fetchUser(userId);
  const posts = await fetchUserPosts(userId);
  const comments = await fetchUserComments(userId);
  return { user, posts, comments };
}

// 좋은 예 - 병렬 실행
async function fetchUserData() {
  const [user, posts, comments] = await Promise.all([
    fetchUser(userId),
    fetchUserPosts(userId),
    fetchUserComments(userId),
  ]);
  return { user, posts, comments };
}
```

#### 3.3 메모리 관리

```javascript
// 나쁜 예 - 메모리 누수
function addEventListeners() {
  const button = document.querySelector("#button");
  const heavyData = new Array(10000).fill("🔥");

  button.addEventListener("click", () => {
    console.log(heavyData); // 클로저가 heavyData 참조 유지
  });
}

// 좋은 예 - 적절한 메모리 해제
function addEventListeners() {
  const button = document.querySelector("#button");
  let cleanup = null;

  function handleClick() {
    if (cleanup) cleanup();

    const heavyData = new Array(10000).fill("🔥");
    console.log(heavyData);

    cleanup = () => {
      button.removeEventListener("click", handleClick);
      // heavyData는 자동으로 가비지 컬렉션됨
    };
  }

  button.addEventListener("click", handleClick);
}
```

#### 3.4 DOM 업데이트 최적화

```javascript
// 나쁜 예 - 반복적인 DOM 업데이트
function updateList(items) {
  const list = document.querySelector("#list");
  items.forEach((item) => {
    list.innerHTML += `<li>${item}</li>`; // 매번 DOM 리렌더링
  });
}

// 좋은 예 - DocumentFragment 사용
function updateList(items) {
  const fragment = document.createDocumentFragment();
  items.forEach((item) => {
    const li = document.createElement("li");
    li.textContent = item;
    fragment.appendChild(li);
  });

  document.querySelector("#list").appendChild(fragment); // 한 번만 DOM 업데이트
}
```

#### 3.5 이벤트 최적화

```javascript
// 나쁜 예 - 과도한 이벤트 핸들러
window.addEventListener("scroll", () => {
  // 무거운 작업 수행
  updateScrollAnimation();
});

// 좋은 예 - 디바운싱 적용
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

const optimizedScroll = debounce(() => {
  updateScrollAnimation();
}, 150);

window.addEventListener("scroll", optimizedScroll);
```

#### 3.6 Web Worker 활용

```javascript
// 메인 스레드
// main.js
const worker = new Worker("worker.js");

worker.postMessage({ data: complexData });
worker.onmessage = (event) => {
  console.log("작업 완료:", event.data);
};

// worker.js
self.onmessage = (event) => {
  const result = heavyComputation(event.data);
  self.postMessage(result);
};
```

이러한 최적화 기법들을 적절히 활용하면 애플리케이션의 성능을 크게 향상시킬 수 있습니다. 특히:

1. 큰 작업은 작은 단위로 분할
2. 비동기 작업은 가능한 병렬 처리
3. 메모리 누수 방지
4. DOM 조작 최소화
5. 이벤트 핸들링 최적화
6. 무거운 계산은 Web Worker로 분리

이러한 원칙들을 지키면서 코드를 작성하면 더 나은 사용자 경험을 제공할 수 있습니다!
