## switch - case 문을 사용할 때 주의해야 할 점

### 1. **엄격한 동등 비교**:

- `switch` 문은 `===` (엄격한 동등 비교)를 사용하여 비교합니다. 즉, 타입이 다르면 일치하지 않는 것으로 간주됩니다.

- 따라서, 비교할 값의 타입을 항상 확인해야 합니다.

```javascript
const value = "5";
switch (value) {
  case 5: // 이 경우는 false
    console.log("일치합니다.");
    break;
  default:
    console.log("일치하지 않습니다.");
}
```

### 2. **break 문 사용**:

- 각 `case` 블록의 끝에 `break` 문을 추가하지 않으면, 다음 `case` 블록으로 계속 실행됩니다.

- 이를 "fall-through"라고 하며, 의도하지 않은 동작을 초래할 수 있습니다.

- **return 사용**: `switch` 문이 함수 내에 있을 경우, `break` 대신 `return`을 사용할 수 있습니다.`return`을 사용하면 해당 함수의 실행을 종료하고 값을 반환합니다. 이 경우, `switch` 문이 끝나면 더 이상 실행되지 않습니다.

```javascript
function getFruitColor(fruit) {
  switch (fruit) {
    case "apple":
      return "빨간색입니다."; // return 사용
    case "banana":
      return "노란색입니다."; // return 사용
    default:
      return "알 수 없는 과일입니다."; // return 사용
  }
}
console.log(getFruitColor("apple")); // '빨간색입니다.'
```

### 3. **default 문 사용**:

- 모든 `case`에 해당하지 않는 경우를 처리하기 위해 `default` 문을 사용하는 것이 좋습니다. 이는 예기치 않은 입력에 대한 처리를 도와줍니다.

```javascript
const color = "purple";
switch (color) {
  case "red":
    console.log("빨간색입니다.");
    break;
  case "blue":
    console.log("파란색입니다.");
    break;
  default:
    console.log("알 수 없는 색상입니다."); // 이 메시지가 출력됩니다.
}
```

### 4. **복잡한 조건 피하기**:

- `switch` 문은 단일 값에 대한 비교에 적합합니다. 복잡한 조건을 처리해야 할 경우, `if - else` 문을 사용하는 것이 더 나을 수 있습니다.

### 5. **문자열과 숫자 혼용 주의**:

- `switch` 문에서 문자열과 숫자를 혼용하여 사용할 경우, 타입이 다르기 때문에 일치하지 않을 수 있습니다.

- 항상 타입을 일치시키는 것이 중요합니다.

- **예시**:

```javascript
const input = "10"; // 문자열
switch (input) {
  case 10: // 숫자
    console.log("숫자 10입니다."); // 이 메시지는 출력되지 않습니다.
    break;
  case "10": // 문자열
    console.log('문자열 "10"입니다.'); // 이 메시지가 출력됩니다.
    break;
  default:
    console.log("알 수 없는 값입니다.");
}
```

### 6. **각 case에서 동일한 변수명 사용 주의**:

- `switch` 문 내의 각 `case`에서 동일한 변수명을 선언하면, 변수의 스코프가 `switch` 문 전체로 확장됩니다. 이로 인해 변수명이 충돌하게 되어 에러가 발생할 수 있습니다.

- 각 `case`는 독립적인 블록이 아니기 때문에, 같은 이름의 변수를 여러 번 선언할 수 없습니다.

- **예시**:

```javascript
switch (fruit) {
  case "apple":
    let color = "red"; // 첫 번째 case에서 color 선언
    console.log(color);
    break;
  case "banana":
    let color = "yellow"; // 두 번째 case에서 color 선언 (에러 발생)
    console.log(color);
    break;
  default:
    console.log("알 수 없는 과일입니다.");
}
```

### 7. **렉시컬 스코프(Lexical Scoping)**:

- **렉시컬 스코프란?**: 렉시컬 스코프는 변수가 정의된 위치에 따라 그 변수가 유효한 범위를 결정하는 개념입니다. 즉, 함수가 선언된 위치에 따라 그 함수 내에서 접근할 수 있는 변수의 범위가 정해집니다.

- `case`와 `default` 절은 레이블과 같아서, 제어 흐름이 점프할 수 있는 가능한 위치를 나타냅니다. 그러나 이들은 자체적으로 렉시컬 스코프를 생성하지 않습니다. 예를 들어:

```javascript
const action = "say_hello";
switch (action) {
  case "say_hello":
    const message = "hello";
    console.log(message);
    break;
  case "say_hi":
    const message = "hi"; // 에러 발생
    console.log(message);
    break;
  default:
    console.log("Empty action received.");
}
```

이 예제는 "Uncaught SyntaxError: Identifier 'message' has already been declared"라는 에러를 출력합니다.

이는 첫 번째 `const message = 'hello';`가 두 번째 `const message = 'hi';` 선언과 충돌하기 때문입니다.

이는 두 `const` 선언이 `switch` 본문에서 생성된 동일한 블록 스코프 내에 있기 때문입니다.

- **해결 방법**: `case` 절에서 `let` 또는 `const` 선언이 필요할 경우, 블록으로 감싸야 합니다.

```javascript
const action = "say_hello";
switch (action) {
  case "say_hello": {
    const message = "hello";
    console.log(message);
    break;
  }
  case "say_hi": {
    const message = "hi";
    console.log(message);
    break;
  }
  default: {
    console.log("Empty action received.");
  }
}
```
