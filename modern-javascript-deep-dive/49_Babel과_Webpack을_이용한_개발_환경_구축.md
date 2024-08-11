# 49. Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축

- 2024.8.11

## 🏷 Babel

```
- Babel은 ES6+/ES.NEXT로 구현된 최신 사양의 소스코드를 구형 브라우저에서 동작하는 소스코드로 변환할 수 있음
```

### 1.1 Babel 설치

```
$ mkdir esnext-project && cd esnext-project
$ npm init -v
$ npm install --save-dev @babel/cli
```

### 2.1 Babel 프리셋 설치와 babel.config.json 설정 파일 작성

```
$ npm install --save-dev @babel/preset-env

- 필요한 플러그인들을 프로젝트 지원 환경에 맞춰 동적으로 결정해줌
- 프로젝트 지원 환경은 Browserlist 형식으로 .browserlistrc 파일에 설정 가능
```

### 1.3 트랜스파일링

```
- package.json 파일의 scripts에 아래의 빌드 명령어를 작성
  - babel src/js -w -d dist/js
- -w: 타깃 폴더에 있는 모든 js 파일의 변경 감지 및 자동 트랜스파일링
- -d: 트랜스파일링된 결과물이 지정될 폴더로 설정
```

### 1.4 바벨 플러그인 설치

```
- Babel 사이트에서 플러그인 탐색 가능
- 설치된 플러그인은 babel.config.json의 plugins에 작성
- 예시) $ npm install --save-dev @babel/plugin-proposal-class-properties
```

### 1.5 브라우저에서 모듈 로딩 테스트

```
- 브라우저는 CommonJS 방식의 require 함수 지원하지 않음 => 트랜스파일링된 결과를 그대로 실행 시 에러 발생
- Webpack으로 문제 해결 가능
```

<br />

## 🏷 Webpack

```
- 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스를 하나 또는 여러개의 파일로 번딜링하는 모듈 번들러
- 여러 개의 자바스크립트 파일을 로드해야하는 번거로움 없음
```

### 2.1 Webpack 설치

```
$ npm install --save-dev webpack webpack-cli
```

### 2.2 babel-loader 설치

```
$ npm install --save-dev babel-loader

- babel-loader: webpack이 모듈을 번들링할 때 babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 프랜스파일링할 수 있도록 사용
```

### 2.3 webpack.config.js 설정 파일 작성

```jsx
const path = require('path')

module.exports = {
  entry: './src/js/main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'js/bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        include: [
          path.resolve(__dirname, 'src/js')
        ],
        exclude: /node_modules/
      },
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env'],
          plugins: ['@babel/preset-proposal-class-properties']
        }
      }
    ]
  },
  devtool: 'source-map',
  mode: 'development'
}
```

### 2.4 babel-polyfill 설치

```
$ npm install @babel/polyfill

- ES6에서 추가된 Promise, Object.assign, Array.from 등은 ES5 사양에서 대체할 기능이 없기 떄문에 트랜스파일링 불가 => babel-polyfill 사용
- babel-polyfill은 실제 운영 환경에서도 사용하기 때문에 devDependencies에 설치하지 않음
- ES6의 import을 사용하는 경우 진입점의 선두에 polyfill 로드
```
