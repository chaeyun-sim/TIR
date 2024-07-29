# 28. Number

- 2024.7.29

## 🏷 Number 생성자 함수

```
- new 연산자와 함께 호출하여 Number 인스턴스 생성
- 또한 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체 생성
- 인수를 전달하면서 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 전달받은 숫자 할당
- 숫자가 아닌 값을 넣으면 숫자로 강제 변환 수행
```

## 🏷 Number 프로퍼티

### 2.1 Number.EPSILON

> 1과 같거나 큰 숫자 중에서 가장 작은 숫자와의 차이와 동일

```jsx
// 부동소수점으로 인해 발생하는 오차를 줄이기 위해 사용

function isEqual(a, b) {
  return Math.abs(a - b) < Number.EPSILON;
}

console.log(0.1 + 0.2 === 0.3); // false
isEqual(0.1 + 0.2, 0.3); // true
```

### 2.2 Number.MAX_VALUE

> 자바스크립트에서 표현할 수 있는 가장 큰 양수 값, 이보다 큰 값은 Infinity

```jsx
Infinity > Number.MAX_VALUE; // true
```

### 2.3 Number.MIN_VALUE

> 자바스크립트에서 표현할 수 있는 가장 작은 양수 값, 이보다 작은 값은 0

```jsx
Number.MIN_VALUE > 0; // true
```

### 2.4 Number.MAX_SAFE_INTEGER

> 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수 값

```jsx
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

### 2.5 Number.MIN_SAFE_INTEGER

> 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수 값

```jsx
Number.MIN_SAFE_INTEGER; // -9007199254740991
```

### 2.6 Number.POSITIVE_INFINITY

> Infinity와 동일

```jsx
Number.POSITIVE_INFINITY; // Infinity
```

### 2.7 Number.NEGATIVE_INFINITY

> -Infinity와 동일

```jsx
Number.NEGATIVE_INFINITY;
```

### 2.8 Number.NAN

> 숫자가 아님(Not-a-Number)을 나타내는 숫자값 (window.NaN과 동일)

```jsx
Number.NaN; // NaN
```

<br />

## 🏷 Number 메서드

### 3.1 Number.isFinite

```jsx
Number.isFinite(0); // true
Number.isFinit(Number.MAX_VALUE); // true
Number.isFinit(Infinity); // false
```

```
- Infinity / -Infinity인지 검사하여 boolean 반환
- NaN이면 언제나 false
- Number.isFinite는 isFinite 함수와 다름 => isFinit는 인수를 숫자로 암묵적 타입 변환하여 사용하나 Number.isFinit는 타입 변환하지 않음
```

### 3.2 Number.isInteger

```jsx
Number.isInteger(0); // true
Number.isInteger(0.5); // false
```

```
- 인수로 전달된 숫자가 정수인지 검사하여 boolean 반환 (암묵적 타입 변환 없음)
```

### 3.3 Number.isNaN

```jsx
Number.isNaN(NaN); // true
Number.isNaN(undefined); // false
```

```
- 인수로 전달된 숫자가 NaN인지 검사하여 boolean 반환
- Number.isNaN은 isNaN 함수와 다름 => isNaN은 인수를 숫자로 암묵적 타입 변환을 수행하나 Number.isNaN은 타입 변환하지 않음
```

### 3.4 Number.isSafeInteger

```jsx
Number.isSafeInteger(0); // true
Number.isSafeInteger(0.5); // false
Number.isSafeInteger('123'); // false
```

```
- 인수로 전달된 숫자가 안전한 정수인지 검사하여 boolean 반환 (암묵적 타입 변환 없음)
- 안전한 정수 값: -(2^53 - 1) ~ (2^53 - 1) 사이의 정수 값
```

### 3.5 Number.prototype.toExponential

```jsx
(77.1234).toExponential(); // '7.71234e+1'
(77.1234).toExponential(4); // '7.7123e+1
```

```
- 숫자를 지수 표기법으로 변환하여 문자열로 반환
- .toExponential 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 표시
- 괄호 없이 숫자 리터럴만 작성하여 SyntaxError 발생
```

### 3.6 Number.prototype.toFixed

```jsx
(12345.6789).toFixed(); // '12346'
(12345.6789).toFixed(3); // '12345.679'
```

```
- 숫자로 반올림하여 문자열로 반환
- 소수점 이하 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달 가능 (생략 시 기본값 0)
```

### 3.7 Number.prototype.toPrecision

```jsx
(12345.6789)
  .toPrecision()(
    // '12345.6789'
    12345.6789
  )
  .toPrecision(1); // '1e+4'
```

```
- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수 반올림 및 문자열로 반환
- 전체 자릿수로 표기할 수 없는 경우 지수 표기법으로 반환
```

### 3.9 Number.prototype.toString

```jsx
(16).toString(); // '16'
(16).toString(2); // '10000'
(16).toString(8); // '20'
(16).toString(16); // '10'
```

```
- 숫자를 문자열로 변환하여 반환
- 진법을 표시하는 2~36 사이의 정수값을 인수로 전달 가능 (생략 시 기본값 10)
```
