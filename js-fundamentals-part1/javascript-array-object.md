# JavaScript의 배열과 객체

## 원시 자료형과의 차이

> 💡 **핵심 개념**: 원시 자료형은 값을 직접 저장하고, 배열/객체는 주소값을 저장합니다.

```javascript
// 원시 자료형 예시
let a = 1;
let b = a; // 값 복사
b = 2;
console.log(a); // 1 (영향 없음)

// 참조형 예시
let arr1 = [1, 2, 3];
let arr2 = arr1; // 주소 복사
arr2.push(4);
console.log(arr1); // [1, 2, 3, 4] (arr1도 변경됨!)
```

## 배열 (Array)

배열은 데이터를 순서대로 저장하는 자료구조입니다.

### 배열 만들기

```javascript
// 1. 배열 리터럴 (가장 일반적)
const fruits = ["사과", "바나나", "오렌지"];

// 2. Array 생성자
const numbers = new Array(1, 2, 3);

// 3. Array.from()
const chars = Array.from("hello"); // ['h', 'e', 'l', 'l', 'o']
```

### 배열 다루기

```javascript
const arr = ["첫번째", "두번째", "세번째"];

// 읽기
console.log(arr[0]); // "첫번째"

// 수정
arr[1] = "새로운 두번째";

// 추가
arr.push("마지막에 추가"); // 끝에 추가
arr.unshift("맨 앞에 추가"); // 앞에 추가

// 삭제
arr.pop(); // 마지막 요소 삭제
arr.shift(); // 첫 요소 삭제
```

## 객체 (Object)

객체는 관련된 데이터를 키-값 쌍으로 저장합니다.

### 기본 객체 만들기

```javascript
const person = {
  name: "김코딩",
  age: 25,
  isStudent: true,
};
```

### 객체 속성 접근하기 🔑

#### 1. 점 표기법 (Dot Notation)

```javascript
console.log(person.name); // "김코딩"
```

- ✅ **사용할 수 있는 경우**:
  - 유효한 JavaScript 식별자인 속성 이름
  - 문자로 시작하고
  - 문자, 숫자, `_`, `$`만 포함

#### 2. 대괄호 표기법 (Bracket Notation)

```javascript
const user = {
  // 특수문자를 포함한 속성 이름
  "user-id": "abc123",
  // 공백을 포함한 속성 이름
  "favorite color": "blue",
  // 숫자로 시작하는 속성 이름
  123: "number key",
  // 일반적인 속성 이름 (이것도 대괄호로 접근 가능)
  name: "Kim",
};

// 특수문자가 있는 경우
console.log(user["user-id"]); // "abc123"

// 공백이 있는 경우
console.log(user["favorite color"]); // "blue"

// 숫자로 시작하는 경우
console.log(user["123"]); // "number key"

// 변수를 사용하여 접근할 때
const propertyName = "name";
console.log(user[propertyName]); // "Kim"

// ❌ 점 표기법으로는 접근 불가능
// console.log(user.user-id); // 에러: undefined - id
// console.log(user.favorite color); // 문법 에러
// console.log(user.123); // 문법 에러
```

#### 속성 이름 규칙 정리 📝

1. **모든 문자열**은 속성 이름으로 사용 가능
2. **점 표기법**은 유효한 식별자인 경우만 사용 가능
3. **대괄호 표기법**은 모든 경우에 사용 가능
4. 가능하면 **점 표기법**을 사용하는 것이 가독성에 좋음
5. 특수한 경우에만 **대괄호 표기법** 사용 권장

### this 키워드 이해하기 🎯

`this`는 "현재 객체"를 가리키는 특별한 키워드입니다.

```javascript
const counter = {
  count: 0,

  // 일반 함수: this가 counter를 가리킴
  increase() {
    this.count += 1;
  },

  // ⚠️ 화살표 함수: this가 다르게 동작
  decreaseArrow: () => {
    this.count -= 1; // 주의! 여기서 this는 counter가 아닐 수 있음
  },
};

counter.increase();
console.log(counter.count); // 1
```

#### this 사용 시 주의사항

1. 일반 함수에서는 `this`가 해당 객체를 가리킴
2. 화살표 함수에서는 `this`가 외부 스코프의 `this`를 가리킴
3. 메서드는 가능하면 일반 함수로 정의하는 것이 안전

### 객체에 메서드 추가하기 🛠️

객체에 함수를 추가하는 방법에는 세 가지가 있습니다:

```javascript
const calculator = {
  // 1. 메서드 축약 표현 (권장)
  add(a, b) {
    return a + b;
  },

  // 2. 함수 표현식
  subtract: function (a, b) {
    return a - b;
  },

  // 3. 화살표 함수
  multiply: (a, b) => a * b,
};

console.log(calculator.add(2, 3)); // 5
console.log(calculator.subtract(5, 2)); // 3
console.log(calculator.multiply(4, 2)); // 8
```

#### 메서드 정의 시 주의사항 ⚠️

1. **메서드 축약 표현** (추천)

   - 가장 현대적이고 깔끔한 문법
   - `this` 바인딩이 올바르게 동작

2. **함수 표현식**

   - 전통적인 방식
   - `this` 바인딩이 올바르게 동작
   - 더 긴 문법

3. **화살표 함수**
   - 간결한 문법
   - ⚠️ `this` 바인딩이 다르게 동작
   - 객체의 메서드로는 권장하지 않음

### 화살표 함수와 this 이해하기 쉽게 설명하기 🎯

#### 일반 함수의 this

일반 함수에서 `this`는 "누가 이 함수를 실행했는지"를 가리킵니다. 마치 "나를 부른 사람"을 가리키는 것과 같습니다.

```javascript
const house = {
  owner: "Kim",

  // 일반 함수
  greet() {
    console.log(`Hello! This is ${this.owner}'s house!`);
  },
};

house.greet(); // "Hello! This is Kim's house!"
// 'house'가 greet을 호출했으므로, this는 'house'를 가리킴
```

#### 화살표 함수의 this

화살표 함수는 자신만의 `this`를 가지지 않고, 함수가 정의될 때 둘러싸고 있는 스코프의 `this`를 그대로 사용합니다.

```javascript
// 전역 스코프에서 객체를 정의할 때
const school = {
  student: "Park",

  // 이 화살표 함수는 전역 스코프에서 정의됨
  introduce: () => {
    console.log(`Hello! I am ${this.student}!`);
    // 여기서 this는 전역 객체(window)를 가리킴
  },
};

// 반면, 일반 함수 내부에서 화살표 함수를 정의하면 다르게 동작
const classroom = {
  teacher: "Kim",
  students: ["Park", "Lee"],

  // 일반 함수
  startClass() {
    // 이 화살표 함수는 startClass 함수 내부에서 정의됨
    this.students.forEach((student) => {
      console.log(`${this.teacher} is teaching ${student}`);
      // 여기서 this는 classroom을 가리킴 (startClass의 this를 그대로 사용)
    });
  },
};

classroom.startClass();
// "Kim is teaching Park"
// "Kim is teaching Lee"
```

#### 왜 이렇게 동작할까요? 🤔

1. **객체 리터럴은 새로운 스코프를 만들지 않습니다**

```javascript
const obj = {
  name: "Kim",
  // 이 화살표 함수는 객체 안에 있지만,
  // 실제로는 전역 스코프에서 정의됨
  sayHi: () => {
    console.log(this.name); // undefined
  },
};
```

2. **함수는 새로운 스코프를 만듭니다**

```javascript
const obj = {
  name: "Kim",
  // 일반 함수는 새로운 스코프를 만듦
  sayHi() {
    // 이 화살표 함수는 sayHi 함수 스코프 안에서 정의됨
    const arrow = () => {
      console.log(this.name); // "Kim"
    };
    arrow();
  },
};
```

#### 정리하면 📝

- 화살표 함수는 자신이 **실제로 정의된 스코프**의 `this`를 사용
- 객체 리터럴은 새로운 스코프를 만들지 않음
- 따라서 객체의 메서드로 직접 화살표 함수를 사용하면, `this`는 전역 객체를 가리킴
- 하지만 일반 함수 안에서 화살표 함수를 사용하면, 그 함수의 `this`를 그대로 사용할 수 있음

## 배열 vs 객체 비교

| 특징      | 배열               | 객체                    |
| --------- | ------------------ | ----------------------- |
| 접근 방식 | 숫자 인덱스        | 키(문자열)              |
| 순서      | 순서 있음          | 순서 없음               |
| 용도      | 비슷한 데이터 모음 | 관련 데이터/동작 그룹화 |
