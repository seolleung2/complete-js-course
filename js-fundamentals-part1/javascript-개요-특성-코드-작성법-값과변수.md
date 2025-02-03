![](https://velog.velcdn.com/images/seolleung_ee/post/e33fc41b-47c6-4be6-b1ed-361cc7757fdd/image.png)

## JavaScript의 특성

### 1. **High-level (고급 언어)** :

- JavaScript는 고급 프로그래밍 언어로, 개발자가 복잡한 세부 사항에 대해 걱정할 필요가 없습니다. 예를 들어, 메모리 관리와 같은 저수준의 작업은 JavaScript 엔진이 자동으로 처리합니다.
- **고수준 언어 vs 저수준 언어** :

  - **고수준 언어**: 개발자가 쉽게 이해하고 사용할 수 있는 구문을 제공하며, 메모리 관리와 같은 복잡한 세부 사항을 추상화합니다. 예: Python, JavaScript.

  - **저수준 언어**: 하드웨어와 가까운 수준에서 작동하며, 메모리 관리와 같은 세부 사항을 직접 다뤄야 합니다. 예: C, 어셈블리 언어.

  - **예시**: C 언어에서 메모리를 직접 할당하고 해제해야 하는 반면, JavaScript에서는 `let a = [];`와 같이 배열을 선언하면 메모리가 자동으로 관리됩니다.

### 2. **Object-oriented (객체 지향)** :

- JavaScript는 객체 지향 프로그래밍(OOP) 패러다임을 지원합니다. 이는 데이터와 그 데이터를 조작하는 함수를 객체라는 단위로 묶어 관리할 수 있음을 의미합니다.
- **프로토타입 기반** : JavaScript는 클래스 기반이 아닌 프로토타입 기반의 객체 지향 언어입니다.
  이는 객체가 다른 객체를 상속할 수 있도록 하며, 새로운 객체를 생성할 때 기존 객체를 기반으로 할 수 있습니다.
- **예시** :

  ```javascript
  function Person(name) {
    this.name = name;
  }
  Person.prototype.greet = function () {
    console.log(`Hello, my name is ${this.name}`);
  };

  const john = new Person("John");
  john.greet(); // Hello, my name is John
  ```

### 3. **Multi-paradigm (다양한 패러다임 지원)** :

- JavaScript는 여러 프로그래밍 패러다임을 지원합니다.
  즉, 객체 지향 프로그래밍 뿐만 아니라 함수형 프로그래밍, 명령형 프로그래밍 등 다양한 스타일로 코드를 작성할 수 있습니다.
- **예시 코드** :

  ```javascript
  // 함수형 프로그래밍 예시
  const numbers = [1, 2, 3, 4, 5];
  const doubled = numbers.map((num) => num * 2);
  console.log(doubled); // [2, 4, 6, 8, 10]

  // 객체 지향 프로그래밍 예시
  class Animal {
    constructor(name) {
      this.name = name;
    }
    speak() {
      console.log(`${this.name} makes a noise.`);
    }
  }

  const dog = new Animal("Dog");
  dog.speak(); // Dog makes a noise.
  ```

## JavaScript 코드 작성 방법

JavaScript 코드를 작성하는 방법에는 여러 가지가 있으며, 각 방법은 특정 상황에 따라 유용하게 사용될 수 있습니다. 아래는 HTML 문서에서 JavaScript를 작성하는 주요 방법들입니다.

### 1. 인라인 스크립트

HTML 요소 내에서 직접 JavaScript 코드를 작성할 수 있습니다. 이 방법은 간단한 스크립트나 이벤트 핸들러에 유용합니다.

```html
<button onclick="alert('Hello, World!')">Click Me</button>
```

### 2. `<script>` 태그를 사용한 내부 스크립트

HTML 문서의 `<head>` 또는 `<body>` 섹션에 `<script>` 태그를 사용하여 JavaScript 코드를 작성할 수 있습니다. 이 방법은 문서 내에서 직접 스크립트를 작성할 수 있게 해줍니다.

```html
<script>
  console.log("Hello from internal script!");
</script>
```

### 3. 외부 JavaScript 파일

JavaScript 코드를 별도의 파일에 작성하고, HTML 문서에서 `<script>` 태그를 사용하여 링크할 수 있습니다. 이 방법은 코드의 재사용성과 유지보수성을 높여줍니다.

```html
<script src="script.js"></script>
```

#### 외부 파일 예시 (script.js)

```javascript
console.log("Hello from external script!");
```

### 스크립트 위치: `<body>` 태그 끝에 작성하는 이유

JavaScript 파일을 `<body>` 태그의 끝에 작성하는 이유는 페이지의 모든 HTML 요소가 로드된 후에 스크립트가 실행되도록 하기 위함입니다.

이렇게 하면 DOM 요소에 접근할 때 해당 요소가 이미 존재하므로, 스크립트가 오류 없이 정상적으로 작동할 수 있습니다.

만약 `<head>` 섹션에 스크립트를 작성하면, HTML 요소가 로드되기 전에 스크립트가 실행되어 DOM 요소를 찾지 못해 오류가 발생할 수 있습니다.

따라서, 일반적으로 다음과 같은 구조를 따릅니다 :

```html
<body>
  <h1>JavaScript Fundamentals – Part 1</h1>
  <script src="script.js"></script>
</body>
```

이렇게 하면 페이지가 로드된 후에 JavaScript 코드가 실행되어, 모든 요소에 안전하게 접근할 수 있습니다.

## JavaScript의 값과 변수

JavaScript에서 변수는 데이터를 저장하는 공간입니다. 변수를 사용하면 프로그램에서 값을 쉽게 관리하고 조작할 수 있습니다. 변수에 값을 할당하는 과정을 상자에 라벨을 붙이는 것에 비유할 수 있습니다.

### 값과 변수의 비유

- **상자**: 변수는 데이터를 저장하는 상자와 같습니다.

- **라벨**: 변수 이름은 상자에 붙이는 라벨로, 상자가 어떤 데이터를 담고 있는지를 나타냅니다.

- **값**: 상자 안에 들어 있는 데이터가 바로 값입니다.

예를 들어, 다음과 같은 코드가 있다고 가정해 보겠습니다:

```javascript
let age = 25; // 'age'라는 라벨이 붙은 상자에 25라는 값이 저장됨
```

여기서 `age`는 상자의 라벨이고, `25`는 상자 안에 들어 있는 값입니다. 이처럼 변수를 사용하면 나중에 `age`라는 라벨을 통해 언제든지 25라는 값을 참조할 수 있습니다.

### 변수 이름 규칙 (컨벤션)

변수 이름을 정할 때는 몇 가지 규칙과 컨벤션이 있습니다:

1. **영문자, 숫자, 언더스코어(\_), 달러 기호($)**: 변수 이름은 영문자, 숫자, 언더스코어, 달러 기호를 사용할 수 있습니다. 그러나 숫자로 시작할 수는 없습니다.

   - 올바른 예: `myVariable`, `age_1`, `$price`
   - 잘못된 예: `1stVariable`, `@name`

2. **대소문자 구분**: JavaScript는 대소문자를 구분합니다. 즉, `myVariable`과 `myvariable`은 서로 다른 변수입니다.

3. **의미 있는 이름**: 변수 이름은 그 변수가 어떤 값을 담고 있는지를 잘 나타내야 합니다. 예를 들어, `age`는 나이를 나타내는 변수로 적절하지만, `x`는 의미가 불분명합니다.

4. **예약어 사용 금지**: JavaScript의 예약어(예: `if`, `for`, `function` 등)는 변수 이름으로 사용할 수 없습니다.

### 사용할 수 없는 변수 이름

- **예약어**: JavaScript에서 미리 정의된 키워드나 예약어는 변수 이름으로 사용할 수 없습니다.
- **특수 문자**: 공백이나 특수 문자는 변수 이름에 포함될 수 없습니다. 예를 들어, `my variable`이나 `my-variable!`는 유효하지 않습니다.

이러한 규칙을 따르면, 코드의 가독성을 높이고, 다른 개발자와의 협업 시 혼란을 줄일 수 있습니다.
