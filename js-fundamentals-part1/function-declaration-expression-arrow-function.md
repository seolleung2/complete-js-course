# 자바스크립트 함수 정의 방식

## 1. 함수 선언식 (Function Declaration)

가장 기본적이고 일반적인 함수 만드는 방법입니다.

```javascript
function greet(name) {
  return `Hello ${name}!`;
}

// 호이스팅 예시 - 함수 선언 전에도 호출 가능
console.log(greet("John")); // "Hello John!" 출력
```

특징:

- 어디서든 사용 가능 (코드 위치 상관없이)
- 이해하기 쉽고 읽기 쉬움

## 2. 함수 표현식 (Function Expression)

함수를 변수에 담아서 사용하는 방법입니다.

```javascript
const multiply = function (a, b) {
  return a * b;
};

// 반드시 함수를 선언한 후에 사용해야 함
console.log(multiply(2, 3)); // 6 출력
```

특징:

- 변수에 할당되므로 유연하게 사용 가능
- 다른 함수의 인자로 전달할 때 유용

## 3. 화살표 함수 (Arrow Function)

더 짧게 쓸 수 있는 최신 함수 작성 방법입니다.

```javascript
const add = (a, b) => a + b;

// 여러 줄 코드가 필요할 때
const calculate = (a, b) => {
  const sum = a + b;
  return sum * 2;
};
```

특징:

- 코드가 짧고 간단함
- 한 줄 코드는 return 생략 가능

## 주요 차이점 설명

### 1. 호이스팅 (Hoisting)

호이스팅은 변수와 함수 선언이 코드의 최상단으로 끌어올려지는 것처럼 동작하는 특성입니다.

```javascript
// 함수 선언식: 선언과 동시에 초기화되어 사용 가능
console.log(greeting("John")); // "Hello John!" 출력
function greeting(name) {
  return `Hello ${name}!`;
}

// 함수 표현식: 선언 전에 사용 불가
console.log(multiply(2, 3)); // ReferenceError
const multiply = function (a, b) {
  return a * b;
};
```

### 2. 사용하기 좋은 상황

함수의 선언은 보통 개발자의 취향을 따르는데 아래와 같이 사용하는 경우가 많습니다.

#### 1. 일반적인 함수는 함수 선언식으로 작성

```javascript
function processData(data) {
  return data * 2;
}
```

#### 2. 간단한 계산이나 배열 작업은 화살표 함수

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map((num) => num * 2);
```

#### 3. 변수에 저장하거나 다른 함수에 전달할 때는 함수 표현식

```javascript
const handleClick = function (event) {
  console.log("버튼이 클릭되었습니다");
};
```

#### 4. 리액트 컴포넌트는 함수 선언식 또는 화살표 함수 표현식

```javascript
// 1. 함수 선언식 - 가독성이 좋고 직관적
function Button({ onClick, children }) {
  return <button onClick={onClick}>{children}</button>;
}

// 2. 화살표 함수 표현식 - 간단한 컴포넌트에 적합
const Icon = ({ name, size }) => (
  <span className={`icon-${name} size-${size}`} />
);
```

리액트 컴포넌트는 두 방식 모두 자주 사용되며 :

- 함수 선언식: 큰 컴포넌트나 로직이 많은 경우
- 화살표 함수: 작고 단순한 컴포넌트의 경우
