# 20. strict mode

- 2024.7.21

## 🏷 strict mode란?

```
- 자바스크립트 언어의 문법을 더 엄격하게 적용하는 모드
- 최적화 작업에 문제를 일으킬 수 있는 코드에 명시적 에러 발생
- ESLint도 strict mode와 유사항 효과
- 린트 도구는 문법적 오류만이 아니라 잠재적 오류까지 찾아내고 원인도 리포팅해준다.
```

<br />

## 🏷 strict mode의 적용

```jsx
// 전역의 선두 또는 함수 내부 선두에 'use strict' 추가

'use strict';

function foo() {
  x = 10;
}

foo();
```

<br />

## 🏷 전역에 strict mode를 적용하는 것은 피하자

```
- 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않는다.
- strict mode와 non-strict mode를 혼용하는 것은 큰 문제 발생 가능성 많다. 이 경우 즉시 실행 함수로 스크립트 전체를 감싸 스코프를 구분하고, 선두에 strict mode를 적용한다.
```

```jsx
(function () {
  'use strict';

  // Do something...
})();
```

<br />

## 🏷 함수 단위로 strict mode를 적용하는 것도 피하자

```
- 위와 같이 어떤 것에는 strict mode, 어떤 것에는 non-strict mode를 적용한다면 문제가 발생할 가능성이 높다.
- 따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다
```

<br />

## 🏷 strict mode가 발생시키는 에러

### 5.1 암묵적 전역

> 선언하지 않은 변수 참조에 의한 ReferenceError 발생

```jsx
(function () {
  'use strict';
  x = 1;
  console.log(x); // ReferenceError
});
```

### 5.2 변수, 함수, 매개변수의 삭제

> delete 연산자에 의한 SyntaxError 발생

```jsx
(function () {
  'use strict';

  var x = 1;
  delete x; // SyntaxError
});
```

### 5.3 매개변수 이름의 중복

> SyntaxError 발생

```jsx
(function () {
  'use strict';

  // SyntaxError
  function foo(x, y) {
    return x + y;
  }
  console.log(foo(1, 2));
});
```

### 5.4 with 문의 사용

> SyntaxError 발생

```
- with문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 이름을 생략할 수 있다.
- 장점: 간단한 코드 / 단점: 성능과 가독성 저하
```

```jsx
(function () {
  'use strict';

  // SyntaxError
  with ({ x: 1 }) {
    console.log(x);
  }
});
```

<br />

## 🏷 strict mode 적용에 의한 변화

### 6.1 일반 함수의 this

```
- strict mode에서 일반 함수를 호출하면 this에 undefined 바인딩
```

### 6.2 arguments 객체

```
- 매개변수에 전달된 인수를 재할당해서 변경해도 arguments 객체에 반영하지 않음
```
