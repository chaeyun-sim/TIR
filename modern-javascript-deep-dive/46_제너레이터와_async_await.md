# 46. 제너레이터와 async/await

- 2024.8.10

## 🏷 제너레이터란?

```
- 코드블록을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수
```

| 제너레이터                                 | 일반 함수                                        |
| -- | -- |
| 함수 호출자에게 함수 실행 제어권 양도 가능 | 제어권은 함수에게 넘어가고 함수 코드를 일괄 실행 |
| 함수 호출자와 함수 상태 주고받음 | 햠수 코드를 일괄 실행하여 결과값을 함수 외부로 반환 |
| 함수 호출 시 제너리에티 객체 반환 | 함수 호출 시 함수 코드 일괄 실행 후 값 반환 |

<br />

## 🏷 제너레이터 함수의 정의

```jsx
// 함수 선언문
function* myFunc() {
	yield 1;
}

// 함수 표현식
const myFunc = function* () {
	yield 1;
};

// 메서드
const obj = {
	*myFunc() {
		yield 1;
	},
};

// 클래스 메서드
class MyClass {
	*myFunc() {
		yield 1;
	}
}
```

<br />

## 🏷 제너레이터 객체

```
- 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환
- 제너레이터 객체는 이터러블이면서 이터레이터
- 이터레이터지만 이터레이터에는 없는 return, throw 메서드 보유
```

<br />

## 🏷 제너레이터의 일시 중지와 재개

```
- yield 키워드: 제너레이터 함수의 실행을 일시 중지 또는 표현식 평가 결과를 제너레이터 함수 호출자에게 반환
- next 메서드 호출 시 yield 표현식까지 실행되고 일시 중지됨 => 함수의 제어권이 호출자로 양도
- 제너레이터 객체의 next 메서드에 전달한 인수는 yield 표현식을 할당받는 변수에 할당
```

## 🏷 제너레이터의 활용

### 5.1 이터러블의 구현

```
- 제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 더 간단히 이터러블 구현가능
```

```jsx
const inifiteFibonacci = (function() {
  let [pre, cur] = [0, 1];
  while (true) {
    [pre, cur] = [cur, pre + cur]
    yield cur;
  }
}());

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num)
}
```

### 5.2 비동기 처리

```
- next 메서드와 yield 표현식을 통해 비동기 처리를 동기 처리처럼 구현 가능
- 제너레이터 실행기가 필요하다면 직접 구현하는 것 보다 co 라이브러리를 사용하는 것을 추천
```

<br />

## 🏷 async/await

```
- 프로미스 기반으로 동작
- 후속 메서드에 콜백함수를 전달해서 비동기 처리 결과를 후속 처리할 필요 없이 동기 처리처럼 프로미스 사용 가능
```

### 6.1 async 함수

```
- async 키워드를 사용해 정의, 언제나 프로미스 반환
- 암묵적으로 반환값을 resolve하는 프로미스 반환
- 클래스 constructor 메서드는 인스턴스를 반환해야하지만 async 인수는 언제나 프로미스 반환
```

```jsx
async function foo(n) {
	return n;
}
foo(1).then((v) => console.log(v));
```

### 6.2 await 키워드

```
- 프로미스가 settled 상태가 될때까지 대기하다가 settled 상태가 되면 resolve한 처리 결과 반환
```

### 6.3 에러 처리

```
- 비동기 함수의 콜백함수를 호출한 것은 비동기 함수가 아니기 때문에 try...catch 문을 사용해 에러 캐치 불가
- async/await에서는 try...catch문으로 에러 처리 가능
- async 함수 내에서 catch 문을 사용해 에러 처리를 하지 않으면 => 발생한 에러를 reject하는 프로미스 반환
```

```jsx
const fetch = require('node-fetch');

const foo = async () => {
	try {
		const wrongUrl = 'https://worng.url';
		const response = await fetch(wrongUrl);
		const data = await response.json();
		console.log(data);
	} catch (error) {
		console.error(err);
	}
};
```
