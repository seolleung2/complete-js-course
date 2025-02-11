# JavaScript 반복문 완벽 가이드

## for 반복문

for 반복문은 JavaScript에서 가장 기본적이고 널리 사용되는 반복문입니다. 초기값, 조건식, 증감식으로 구성되며, 조건이 참인 동안 코드 블록을 반복 실행합니다.

```javascript
// 기본 for문 예시
for (let i = 1; i <= 5; i++) {
  console.log(`${i}번째 실행`);
}

// 배열 순회하기
const fruits = ["사과", "바나나", "오렌지"];
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}
```

## break와 continue

반복문을 제어하는 두 가지 중요한 키워드가 있습니다:

- `break`: 반복문을 즉시 종료
- `continue`: 현재 반복을 건너뛰고 다음 반복으로 진행

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// continue로 짝수만 출력하기
for (let i = 0; i < numbers.length; i++) {
  if (numbers[i] % 2 !== 0) continue;
  console.log(numbers[i]); // 2, 4, 6, 8, 10
}

// break로 원하는 값을 찾으면 종료하기
for (let i = 0; i < numbers.length; i++) {
  if (numbers[i] === 5) break;
  console.log(numbers[i]); // 1, 2, 3, 4
}
```

## 다양한 반복문 패턴

### 역순 순회

```javascript
const numbers = [1, 2, 3, 4, 5];
for (let i = numbers.length - 1; i >= 0; i--) {
  console.log(numbers[i]); // 5, 4, 3, 2, 1
}
```

### 중첩 반복문

```javascript
// 구구단 출력
for (let i = 2; i <= 9; i++) {
  for (let j = 1; j <= 9; j++) {
    console.log(`${i} x ${j} = ${i * j}`);
  }
}
```

## while 반복문

while문은 조건식만으로 반복을 제어합니다. 반복 횟수가 불명확할 때 유용합니다.

```javascript
// 기본 while문
let count = 1;
while (count <= 5) {
  console.log(`${count}번째 실행`);
  count++;
}

// 실전 예시: 주사위 게임
let dice = Math.floor(Math.random() * 6) + 1;
while (dice !== 6) {
  console.log(`주사위: ${dice}`);
  dice = Math.floor(Math.random() * 6) + 1;
}
console.log("6이 나왔습니다!");
```

## 모던 JavaScript의 배열 고차함수

최신 JavaScript에서는 배열을 더 우아하게 다룰 수 있는 다양한 메서드를 제공합니다.

```javascript
const numbers = [1, 2, 3, 4, 5];
const users = [
  { id: 1, name: "김철수", age: 25 },
  { id: 2, name: "이영희", age: 28 },
  { id: 3, name: "박지성", age: 32 },
];

// map: 배열 변환
const doubled = numbers.map((num) => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter: 조건부 필터링
const adults = users.filter((user) => user.age >= 30);

// find: 조건에 맞는 첫 번째 요소 찾기
const user = users.find((user) => user.age > 30);

// reduce: 값 누적하기
const sum = numbers.reduce((acc, cur) => acc + cur, 0);

// forEach: 단순 반복
users.forEach((user) => {
  console.log(`${user.name}님은 ${user.age}세입니다.`);
});

// some과 every: 조건 검사
const hasAdult = users.some((user) => user.age >= 30);
const allAdults = users.every((user) => user.age >= 20);
```

### 메서드 체이닝

고차함수들은 체이닝을 통해 복잡한 데이터 처리를 간단히 표현할 수 있습니다.

```javascript
// 30세 이상 사용자의 이름만 추출하기
const adultNames = users
  .filter((user) => user.age >= 30)
  .map((user) => user.name);
console.log(adultNames); // ['박지성']
```

### 고차함수의 장점

- 코드가 더 선언적이고 이해하기 쉬워집니다
- 메서드 체이닝으로 복잡한 로직을 단순화할 수 있습니다
- 불변성을 유지하여 예측 가능한 코드를 작성할 수 있습니다

## Lodash vs 내장 배열 메서드

실무에서 Lodash를 쓸지 내장 메서드를 쓸지 항상 고민되는데요. 제가 실제 개발하면서 느낀 장단점을 정리해봤습니다.

```javascript
const _ = require("lodash");

// 테스트용 더미 데이터
const users = [
  { id: 1, name: "김철수", age: 25, active: true },
  { id: 2, name: "이영희", age: 28, active: false },
  { id: 3, name: "박지성", age: 32, active: true },
];

const numbers = [1, 2, 3, null, 4, undefined, 5];
const obj = { a: 1, b: 2 };

// 1. null/undefined 처리
// Lodash가 null/undefined 처리에 안전
console.log(_.map(numbers, (n) => n * 2)); // [2, 4, 6, 0, 8, 0, 10]
console.log(numbers.map((n) => n * 2)); // [2, 4, 6, 0, 8, NaN, 10] - NaN 주의!

// 2. 객체 다루기
// Lodash는 한 줄로 끝나는데
console.log(_.map(obj, (value, key) => `${key}: ${value}`)); // ["a: 1", "b: 2"]
// 내장 메서드는 이렇게 해야 됨
console.log(Object.entries(obj).map(([k, v]) => `${k}: ${v}`));

// 3. 빈 값 체크할 때
// Lodash는 그냥 isEmpty 하나로 다 됨
console.log(_.isEmpty([])); // true
console.log(_.isEmpty({})); // true
console.log(_.isEmpty("")); // true
console.log(_.isEmpty(null)); // true

// 내장 메서드는 타입마다 다르게 체크해야 힘
console.log([].length === 0); // 배열
console.log(Object.keys({}).length === 0); // 객체

// 4. 중첩 객체 다룰 때
// Lodash의 get은 진짜 편함
const deepObj = { a: { b: { c: 3 } } };
console.log(_.get(deepObj, "a.b.c")); // 3
console.log(_.get(deepObj, "a.b.d", 0)); // 없으면 기본값 반환

// 옵셔널 체이닝으로 어느 정도 커버되긴 함
console.log(deepObj?.a?.b?.c); // 3
```

### Lodash 쓰면 좋은 경우

1. **데이터가 불안정할 때**

   - null/undefined 알아서 처리해줌
   - 타입 에러로 안정성 확보

2. **복잡한 데이터 다룰 때**

   - 중첩 객체? `_.get`으로 끝
   - 정렬, 그룹핑도 한 줄로 가능
   - 함수형으로 깔끔하게 처리 가능

3. **IE 등 레거시 브라우저 지원할 때**
   - 브라우저 호환성 걱정 없음
   - 동작도 일관적

### 내장 메서드 쓰면 좋은 경우

1. **간단한 배열 작업**

   - map, filter 정도면 충분
   - 성능도 좀 더 좋음 (큰 차이는 아닌 듯)

2. **번들 사이즈 신경쓸 때**

   - Lodash는 크기가 좀 있음

3. **모던 자바스크립트 쓸 수 있을 때**
   - 옵셔널 체이닝
   - null 병합
   - 스프레드 연산자

### 실무에서는 이렇게 씁니다

```javascript
// 이렇게 필요한 것만 가져다 쓰세요
import map from "lodash/map";
import isEmpty from "lodash/isEmpty";

// 이렇게 통째로 임포트하면 번들 사이즈 커져요
import _ from "lodash"; // 안티패턴

// 간단한 건 그냥 내장 메서드로
const simpleArray = [1, 2, 3, 4, 5];
const doubled = simpleArray.map((x) => x * 2);

// 복잡한 건 Lodash가 편해요
const complexData = [{ user: { details: { age: 25 } } }];
const ages = _.map(complexData, "user.details.age");
```
