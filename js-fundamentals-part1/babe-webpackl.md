# 프론트엔드 개발의 핵심 도구: 웹팩과 바벨

## 1. 바벨(Babel)

### 1.1 AST(Abstract Syntax Tree) 쉽게 이해하기

코드를 트리 구조로 표현한 것입니다. 마치 가계도처럼 코드의 구조를 표현합니다.

```javascript
// 원본 코드
const greeting = (name) => `Hello ${name}`;

// AST로 표현하면 이런 구조가 됩니다
{
  // 변수 선언
  type: "VariableDeclaration",
  declarations: [
    {
      // 변수 이름: greeting
      name: "greeting",
      // 화살표 함수
      value: {
        type: "ArrowFunction",
        // 매개변수: name
        params: ["name"],
        // 함수 본문
        body: `Hello ${name}`
      }
    }
  ]
}
```

이렇게 트리 구조로 만들면 바벨이 코드를 쉽게 분석하고 변환할 수 있습니다.

### 1.2 바벨이란?

- JavaScript 컴파일러(정확히는 트랜스파일러)로, 최신 버전의 JavaScript 코드를 구버전으로 변환
- 브라우저 호환성 문제 해결을 위한 필수 도구
- 모던 자바스크립트(ES6+) 기능을 구버전 브라우저에서도 사용 가능하게 해줌

> **컴파일러 vs 트랜스파일러**
>
> - 컴파일러: 한 언어를 다른 언어로 변환 (예: C++ → 기계어)
> - 트랜스파일러: 같은 언어의 다른 버전으로 변환 (예: ES6+ → ES5)
> - 바벨은 엄밀히 말하면 "트랜스파일러"이지만, 통상적으로 "컴파일러"라고도 부름

### 1.3 바벨의 동작 과정

1. **파싱(Parsing)**

   - 소스코드를 추상 구문 트리(AST)로 변환
   - 코드를 토큰으로 분해하고 문법 분석

2. **변환(Transform)**

   - 플러그인을 통해 AST 변환
   - 예: 화살표 함수를 일반 함수로 변환

3. **생성(Generate)**
   - 변환된 AST를 실제 코드로 생성

### 1.4 바벨 설정 예시

```javascript
// babel.config.json
{
  "presets": [
    ["@babel/preset-env", {
      "targets": {
        "edge": "17",
        "firefox": "60",
        "chrome": "67",
        "safari": "11.1"
      }
    }],
    "@babel/preset-react"
  ]
}
```

### 1.5 실제 변환 예시

```javascript
// 변환 전 (모던 자바스크립트)
const multiply = (a, b) => a * b;
const greeting = `Hello ${name}`;
const values = [...array];

// 변환 후 (ES5)
var multiply = function (a, b) {
  return a * b;
};
var greeting = "Hello " + name;
var values = [].concat(array);
```

## 2. 웹팩(Webpack)

### 2.1 웹팩 쉽게 이해하기

웹팩은 마치 레고 블록을 조립하는 것과 비슷합니다:

1. **여러 개의 파일을 하나로 합치기**

```javascript
// math.js
export const add = (a, b) => a + b;

// greeting.js
export const sayHello = (name) => `Hello ${name}`;

// index.js
import { add } from "./math";
import { sayHello } from "./greeting";

// 웹팩이 이 파일들을 하나로 합쳐줍니다!
```

2. **HMR(Hot Module Replacement) 이해하기**

HMR은 "실시간 업데이트"라고 생각하면 됩니다.

```javascript
// 리액트 컴포넌트
function Button() {
  return <button>클릭</button>;
}

// 버튼 스타일을 변경하면
function Button() {
  return <button style={{ color: "red" }}>클릭</button>;
}

// 페이지 새로고침 없이 바로 반영됩니다!
```

### 2.2 웹팩이란?

- 모던 JavaScript 애플리케이션을 위한 정적 모듈 번들러
- 프로젝트의 모든 리소스(JS, CSS, 이미지 등)를 모듈로 관리
- 의존성 관리와 리소스 최적화를 자동화

### 2.3 웹팩의 주요 기능

1. **모듈 번들링**

   - 여러 파일을 하나로 병합
   - 불필요한 코드 제거
   - 성능 최적화

2. **코드 분할 (Code Splitting)**

   ```javascript
   // webpack.config.js
   module.exports = {
     optimization: {
       splitChunks: {
         chunks: "all",
         minSize: 20000,
         minChunks: 1,
       },
     },
   };
   ```

3. **자동 리로드와 HMR**
   ```javascript
   // webpack.config.js
   module.exports = {
     devServer: {
       hot: true,
       port: 3000,
       open: true,
     },
   };
   ```

### 2.4 웹팩 설정 예시

```javascript
// webpack.config.js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "[name].[contenthash].js",
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
  ],
};
```

## 3. 실무 활용 예시

### 3.1 리액트 프로젝트 설정

```javascript
// package.json
{
  "scripts": {
    "start": "webpack serve --mode development",
    "build": "webpack --mode production"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@babel/core": "^7.22.0",
    "@babel/preset-react": "^7.22.0",
    "webpack": "^5.85.0",
    "webpack-cli": "^5.1.1"
  }
}
```

### 3.2 최적화 설정

```javascript
// webpack.config.js (프로덕션 최적화)
module.exports = {
  optimization: {
    minimize: true,
    minimizer: [
      new TerserPlugin({
        terserOptions: {
          compress: {
            drop_console: true,
          },
        },
      }),
    ],
    splitChunks: {
      chunks: "all",
    },
  },
};
```

### 3.3 리액트에서 웹팩 활용 예시

1. **코드 분할하기**

```javascript
// 기존 방식
import BigComponent from "./BigComponent";

// 코드 분할 방식
const BigComponent = React.lazy(() => import("./BigComponent"));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <BigComponent />
    </Suspense>
  );
}
```

2. **이미지 최적화**

```javascript
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: "file-loader",
            options: {
              name: "[name].[ext]",
              outputPath: "images/",
            },
          },
          {
            loader: "image-webpack-loader",
            options: {
              mozjpeg: {
                progressive: true,
                quality: 65,
              },
            },
          },
        ],
      },
    ],
  },
};

// 리액트 컴포넌트에서 사용
import myImage from "./images/photo.jpg";

function Profile() {
  return <img src={myImage} alt="프로필" />;
}
```

3. **CSS 모듈화**

```javascript
// Button.module.css
.button {
  background: blue;
  color: white;
}

// Button.js
import styles from './Button.module.css';

function Button() {
  return <button className={styles.button}>클릭하세요</button>;
}
```

### 3.4 실무에서 자주 사용하는 웹팩 설정

```javascript
// webpack.config.js
module.exports = {
  // 개발 모드 설정
  mode: "development",

  // 소스맵 생성 (디버깅용)
  devtool: "source-map",

  // 개발 서버 설정
  devServer: {
    port: 3000,
    hot: true,
    // API 서버 연결
    proxy: {
      "/api": "http://localhost:8080",
    },
  },

  // 파일 처리 규칙
  module: {
    rules: [
      // 자바스크립트 처리
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
      // CSS 처리
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
      // 이미지 처리
      {
        test: /\.(png|jpg|gif)$/,
        use: ["file-loader"],
      },
    ],
  },
};
```

### 3.5 성능 최적화 팁

1. **코드 분할**

```javascript
// 라우터 기반 코드 분할
import { lazy, Suspense } from "react";

const Home = lazy(() => import("./pages/Home"));
const About = lazy(() => import("./pages/About"));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Suspense>
  );
}
```

2. **이미지 최적화**

```javascript
// next.js에서의 이미지 최적화
import Image from "next/image";

function Profile() {
  return (
    <Image
      src="/profile.jpg"
      alt="프로필"
      width={500}
      height={300}
      placeholder="blur"
    />
  );
}
```

> **실무 팁**
>
> - Create React App을 사용할 때는 별도의 웹팩 설정이 필요 없습니다
> - 커스텀 설정이 필요하면 craco나 react-app-rewired를 사용하세요
> - Next.js는 이미 최적화된 설정을 제공하므로 추가 설정이 거의 필요 없습니다
> - 큰 프로젝트에서는 코드 분할을 적극적으로 활용하세요
