# JavaScript 변수에 값을 할당할 때의 과정 및 호이스팅

## 1. 변수 선언

변수를 선언할 때, JavaScript 엔진은 해당 변수를 메모리에 생성합니다. 이때 변수는 기본적으로 `undefined`로 초기화됩니다. 즉, 변수를 선언하는 순간에는 값이 할당되지 않았기 때문에 `undefined` 상태가 됩니다.

### 예시

```javascript
let myVariable; // 변수 선언
console.log(myVariable); // undefined
```

위의 코드에서 `myVariable`을 선언했지만, 값이 할당되지 않았기 때문에 `undefined`가 출력됩니다.

## 2. 값 할당

변수를 선언한 후, 값을 할당할 수 있습니다. 이때 변수는 새로운 값을 저장하게 되며, 이전의 `undefined` 상태에서 해당 값으로 변경됩니다.

### 예시

```javascript
myVariable = 10; // 값 할당
console.log(myVariable); // 10
```

위의 코드에서 `myVariable`에 `10`이라는 값을 할당하면, 이제 `myVariable`은 `10`이라는 값을 가지게 됩니다.

## 3. 변수 호이스팅 (Hoisting)

JavaScript에서는 변수 선언이 코드의 최상단으로 끌어올려지는 현상을 **호이스팅**이라고 합니다.

이로 인해 변수를 선언하기 전에 해당 변수를 참조할 수 있습니다. 그러나 변수의 값은 호이스팅되지 않으며, 초기화는 여전히 선언된 위치에서 이루어집니다.

### 호이스팅의 예시

```javascript
console.log(myVariable); // undefined
var myVariable = 10;
console.log(myVariable); // 10
```

위의 코드에서 `console.log(myVariable);`가 변수 선언 이전에 호출되었지만, `undefined`가 출력됩니다. 이는 JavaScript 엔진이 다음과 같이 해석하기 때문입니다:

```javascript
var myVariable; // 호이스팅: 변수 선언이 최상단으로 끌어올려짐
console.log(myVariable); // undefined
myVariable = 10; // 값 할당
```

### `let`과 `const`의 호이스팅

`let`과 `const`로 선언된 변수도 호이스팅이 발생하지만, 이들 변수는 **일시적 사각지대(Temporal Dead Zone, TDZ)**에 존재합니다. 즉, 변수 선언 이전에 해당 변수를 참조하려고 하면 `ReferenceError`가 발생합니다.

![](https://velog.velcdn.com/images/seolleung_ee/post/13b878d6-d974-4200-b94a-054710fb96b7/image.jpg)

### 예시

```javascript
console.log(myLet); // ReferenceError: Cannot access 'myLet' before initialization
let myLet = 20;

console.log(myConst); // ReferenceError: Cannot access 'myConst' before initialization
const myConst = 30;
```

위의 코드에서 `myLet`과 `myConst`는 선언 이전에 접근하려고 할 때 오류가 발생합니다. 이는 `let`과 `const`가 선언된 위치에서만 사용할 수 있기 때문입니다.

선언된 변수 전 줄까지가 바로 일시적 사각지대에 해당됩니다.

## 4. 변수의 상태 변화

변수에 값을 할당한 후, 해당 변수는 새로운 값을 참조하게 됩니다. 이때 변수의 타입은 할당된 값에 따라 결정됩니다. JavaScript는 동적 타이핑 언어이기 때문에, 변수의 타입은 값에 따라 변경될 수 있습니다.

### 예시

```javascript
myVariable = "Hello"; // 문자열 할당
console.log(myVariable); // 'Hello'

myVariable = true; // 불리언 할당
console.log(myVariable); // true
```

## 5. 요약

1. **변수 선언**: 변수를 선언하면 메모리에 생성되고, 기본적으로 `undefined`로 초기화됩니다.

2. **값 할당**: 값을 할당하면 변수는 해당 값을 참조하게 되며, 이전의 `undefined` 상태에서 새로운 값으로 변경됩니다.

3. **호이스팅**: 변수 선언이 최상단으로 끌어올려지며, 초기화는 선언된 위치에서 이루어집니다. 이로 인해 선언 이전에 변수를 참조하면 `undefined`가 출력됩니다. `let`과 `const`는 호이스팅이 발생하지만, 일시적 사각지대(TDZ)가 존재하여 선언 이전에 접근하면 `ReferenceError`가 발생합니다.

4. **상태 변화**: 변수는 동적 타이핑 언어인 JavaScript에서 할당된 값에 따라 타입이 변경될 수 있습니다.
