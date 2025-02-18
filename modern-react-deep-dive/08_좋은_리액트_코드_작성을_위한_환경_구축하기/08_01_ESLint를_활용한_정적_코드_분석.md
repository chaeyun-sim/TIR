# 8. 좋은 리액트 코드 작성을 위한 환경 구축하기

## 🏷 ESLint를 활용한 정적 코드 분석
- 버그와 예기치 못한 작동 방지를 위한 방법 중 하나 -> `정적 코드 분석`
- 정적 코드 분석: 코드 스멜을 찾아내어 문제의 코드를 사전에 수정하는 것
- 가장 많이 사용되는 정적 코드 분석 도구: `ESLint`

### 8.1.1 ESLint 살펴보기
#### ESLint는 어떻게 코드를 분석할까?
- 자바스크립트 코드를 정적 분석해 잠재적인 문제를 발견 및 수정하는 도구
- ESLint의 코드 분석 방법
```
1. 자바스크립트 코드를 문자열로 읽음
2. 파서(parser)로 코드 구조화
3. 구조화된 트리(Abstract Syntax Tree)를 기준으로 규칙들과 대조
4. 규칙을 위한반 코드를 알리거나(report) 수정한다(fix)
```
- ESLint는 자바스크립트 코드를 분석하기 위해 espree 사용

<br />

### 8.1.2 eslint-plugin과 eslint-config
#### eslint-plugin
- 규칙을 모아놓은 패키지
- eslint-plugin-import: import 관련 규칙 제공
- eslint-plugin-react: react 관련 규칙 제공

#### eslint-config
- 특정 프레임이나 도메인과 관련된 규칙 패키지
- eslint-config-airbnb: 에어비앤비에서 만듦, 가장 유명함
- @titicaca/triple-config/kit: 한국 커뮤니티 운영, 규칙에 테스트코드 존재
- eslint-config-next: Next.js에서 지원, 유용한 기능 제공

<br />

### 8.1.3 나만의 ESLint 규칙 만들기
#### 이미 존재하는 규칙을 커스터마이징해서 적용하기: import React를 제거하기 위한 ESLint 규칙 만들기
- import React를 삭제하면 번들러 크기 감소 가능
- no-restricted-imports 규칙 사용
```js
module.exports = {
  rules: {
    'no-restricted-imports': [
      'error',
      {
        paths: [
          {
            name: 'react',
            importNames: ['default'],
            message: '...'
          }
        ]
      }
    ]
  }
}
```

<br />

### 8.1.4 주의할 점
#### Prettier와의 충돌
- eslint는 잠재적인 문제 분석, prettier는 포매팅 작업 담당
- ESLint는 자바스크립트에서만, prettier는 HTML/CSS/Markdown/JSON 등 가능
- 충돌되지 않게끔 규칙을 잘 설정할 것

#### 규칙에 대한 예외 처리, 그리고 react-hooks/no-exhaustive-deps
- useEffect, useMemo 등에서 의존 배열을 제대로 선언했는지 확인하는 역할 수행
- typescript-eslint/no-explicit-any: any 강제 사용 방지