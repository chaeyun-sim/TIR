# 33. 7번째 데이터 타입 Symbol

- 2024.8.1

## 🏷 심벌이란?

```
- ES6에서 도입된 7번째 데이터 타입, 변경 불가능한 원시 타입의 값
- 다른 값과 중복되지 않는 유일무이한 값
- 주로 충돌 위험이 없는 "유일한 프로퍼티 키"를 만들기 위해 사용
```

<br />

## 🏷 심벌 값의 생성

### 2.1 Symbol 함수

```
- 다른 원시값은 리터럴 표기법을 통해 값을 생성할 수 있으나 심벌 값은 Symbol 함수를 호출하여 생성
- new 생성자 없이 호출 가능
- 인수: 문자열 전달, 생성된 심벌 값에 대한 description, 디버깅 용도로만 사용
- 심벌 값에 객체처럼 접근하면 암묵적으로 래퍼 객체 생성
- 불리언 타입으로 암묵적 타입 변환 가능
```

```jsx
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol
console.log(mySymbol); // Symbol()
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)

const mySymbol1 = Symbol('hi');
const mySymbol2 = Symbol('hi');
console.log(mySymbol1 === mySymbol2); // false
```

### 2.2 Symbol.for / Symbol.keyfor 메서드

```
- Symbol.for: 인수로 전달받은 문자열을 키로 하여 "전역 심벌 레지스트리"에서 해당 키와 일치하는 심벌값 검색
- 검색 성공 시 심벌 값을 반환하고 실패 시 새로운 심벌 값을 생성하여 전역 심벌 레지스트리에 저장한 후 심벌 값 반환
- Symbol.keyFor: 전역 심벌 레지스트리에 저장된 심벌 값의 키 추출 가능
```

```jsx
const s1 = Symbol.for('mySymbol');
const s2 = Symbol.for('mySymbol');
console.log(s1 === s2); // true

Symbol.keyfor(s1); // mySymbol
```

<br />

## 🏷 심벌과 상수

```
- 변경 및 중복될 가능성이 있는 무의미한 상수 대신 중복 가능성이 없는 유일무이한 심벌 값 사용 추천
```

```jsx
// 상수 사용
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
};
const myDirection = Direction.UP;

// 심볼 사용
const Direction = {
  UP: Symbol('UP'),
  DOWN: Symbol('DOWN'),
  LEFT: Symbol('LEFT'),
  RIGHT: Symbol('RIGHT'),
};
const myDrection = Direction.UP;
```

<br />

## 🏷 심벌과 프로퍼티 키

```jsx
- 프로퍼티 키는 모든 문자열 또는 심벌 값으로 생성 가능
- 심벌 값은 유일무이하므로 다른 프로퍼티 키와 절대 충돌하지 않음
```

```jsx
const obj = {
  [Symbol.for('mySymbol')]: 1,
};
obj[Symbol.for('mySymbol')]; // 1
```

<br />

## 🏷 심벌과 프로퍼티 은닉

```
- 심벌 값을 프로퍼티 키로 사용 => 외부에 노출할 필요가 없는 프로퍼티 은닉
- 그러나 완전한 은닉은 불가능 => ES6의 Object.getOwnPropertySymbols로 확인 가능
```

<br />

## 🏷 심벌과 표준 빌트인 객체 확장

```
- 메서드 이름이 중복될 수 있기 때문에 표준 빌트인 객체에 사용자 정의 메서드를 추가하는 것은 권장하지 않음
- 심벌 값으로 프로퍼티 키를 생성하면 기존 키와 충돌하지 않고 안전하여 표준 빌트인 객체를 확장할 수 있음
```

```jsx
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
}[(1, 2)][Symbol.for('sum')](); // 3
```

<br />

## 🏷 Well-known Symbol

```
- Well-known Symbol: 자바스크립트 기본 제공 빌트인 심벌 값
- 빌트인 이터러블은 Symbol.iterator을 키로 가지는 메서드를 가지며, 호출 시 이터레이터를 반환하도록 규정
- 일반 객체를 이터러블처럼 동작하도록 구현 => Symbol.iterator을 키로 갖는 메서드를 객체에 추가하고, 이터레이터 반환
```
