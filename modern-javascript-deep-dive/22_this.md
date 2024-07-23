# 22. this

- 2024.7.23

## 🏷 this 키워드

```
- 자신이 속한 객체 또는 생성할 인스턴스를 가리키는 자기 참조 변수(self-referencing variable)
- 즉, 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있음
- 자바스크립트 엔진에 의해 암묵적 생성, 어디서든 참조 가능
- this 바인딩은 함수 호출 방식에 의해 동적으로 결정
- strict mode도 this 바인딩에 영향을 줌
```

<br />

## 🏷 함수 호출 방식과 this 바인딩

```
- this 바인딩은 함수 호출 방식에 따라 동적으로 결정
```

### 2.1 일반 함수 호출

> 전역 객체를 this에 바인딩

```jsx
function foo() {
  console.log('foo is this: ', this); // window
  function bar() {
    console.log('bar is this: ', this); // window
  }
  bar();
}
foo();
```

```
- 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 등) 내부의 this에는 전역 객체 바인딩
- 그러나 객체를 생성하지 않는 일반 함수에서는 this가 의미가 없다
- 즉, strict mode가 적용된 일반 함수 내부 this에는 undefined 바인딩
- this 명시적으로 바인딩하기: Function.prototype.apply, Function.prototype.call, Function.prototype.bind
```

### 2.2 메서드 호출

> 마침표 연산자 앞에 기술한 객체를 this에 바인딩

```
- 프로토타입 메서드 내부에서 사용된 this도 메서드를 호출한 객체에 바인딩
```

```jsx
const person = {
  name: 'Harry',
  getName() {
    // this는 메서드를 호출한 객체에 바인딩
    return this.name;
  },
};

// 이때 마침표 연산자 앞의 객체는 person이기 때문에 this에 바인딩된 객체는 person이다.
console.log(person.getName());
```

### 2.3 생성자 함수 호출

> 생성자 함수가 미래에 생성할 인스턴스에 this 바인딩

```
- new 연산자와 함께 호출 시 해당 함수는 생성자 함수로 동작
- new 연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작
```

### 2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

```
- 이 메서드들은 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출
- apply/call 메서드: 첫번째 인수로 전달한 객체를 호출한 함수의 this에 바인딩
- bind 메서드: 메서드의 this와 중첩 함수 또는 콜백 함수의 this의 불일치 문제 해결
```

```jsx
function getThisBinding() {
  return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding.apply(thisArg)); // { a : 1 }
console.log(getThisBinding.call(thisArg)); // { a : 1 }
```

### 2.5 최종 정리

| 함수 호출 방식 | this 바인딩 |
| --- | --- |
| 일반 함수 호출 | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 미래에 생성할 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 "간접 호출" | Function.prototype.apply/call/bind 메서드에 "첫번째 인수"로 전달한 객체 |
