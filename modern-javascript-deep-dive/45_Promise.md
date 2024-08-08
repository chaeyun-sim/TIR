# 45. 프로미스

- 2024.8.8

## 🏷 비동기 처리를 위한 콜백 패턴의 단점

### 1.1 콜백 헬

```
- 비동기 함수 호출 시 함수 내부의 비동기 코드가 완료되지 않아도 즉시 종료 가능
- 비동기 함수: 함수 내부에 비동기로 동작하는 코드를 포함한 함수
- get 함수가 비동기인 이유 => 함수 내부의 onload 이벤트 핸들러가 비동기로 동작하기 때문
- xhr.onload 이벤트 핸들러는 load 이벤트가 발생하면 => 태스크 큐 저장 => 콜 스택이 비면 => 이벤트 루프에 의해 콜 스택으로 푸시되어 실행
- 비동기 함수: 비동기 처리 결과를 외부 반환 불가, 상위 스코프의 변수에 할당 불가
- 비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행
- 콜백 헬 callback hell: 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상
```

### 1.2 에러 처리의 한계

```
- 에러는 호출자의 방향으로 전파 (콜 스택의 아래 방향)
- 따라서 setTimeout 함수의 경우 catch 블록에서 에러가 캐치되지 않음
- 이를 극복하기 위해 ES6에서 프로미스 도입
```

<br />

## 🏷 프로미스의 생성

```
- new 연산자와 함께 호출 시 Promise 생성
- Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받음
- 이때 콜백함수는 resolve와 reject 함수를 인수로 전달받음
```

```jsx
const promise = new Promise((resolve, reject) => {
  if (/* 비동기 처리 성공 */) {
    resolve('result')
  } else {
    reject('failure reason')
  }
})
```

- 프로미스의 상태 정보

| 프로미스의 상태 정보 | 의미                                  | 상태 변경 조건                   |
| -------------------- | ------------------------------------- | -------------------------------- |
| pending              | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
| fulfilled            | 비동기 처리가 수행된 상태 (성공)      | resolve 함수 호출                |
| rejected             | 비동기 처리가 수행된 상태 (실패)      | reject 함수 호출                 |

```
- 생성된 직후의 프로미스는 pending 상태
- settled 상태는 pending이 아닌 상태로 비동기 처리가 수행된 상태
- 비동기 처리 성공 : pending -> fulfilled
- 비동기 처리 실패 : pending -> rejected
```

<br />

## 🏷 프로미스의 후속 처리 메서드

```
- 프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출
```

### 3.1 Promise.prototype.then

```
- 두 개의 콜백 함수를 인수로 전달받음
- 첫번째 콜백 함수: fulfulled 상태
- 추번째 콜백 함수: rejected 상태
- then 메서드는 언제나 프로미스 반환
```

```jsx
new Promise((resolve) => resolve('fulfilled')).then(
	(v) => console.log(v),
	(e) => console.error(e)
);
```

### 3.2 Promise.prototype.catch

```
- 한 개의 콜백 함수를 인수로 전달받음
- 콜백 함수의 프로미스가 rejected 상태인 경우만 호출
```

```jsx
new Promise((_, reject) => reject(new Error('rejected'))).catch((e) =>
	console.log(e)
);
```

### 3.3 Promise.prototype.finally

```
- 한 개의 콜백 함수를 인수로 전달받음
- 프로미스의 성공/실패에 상관 없이 무조건 한 번 호출
- 프로미스 상태와 상관없이 공통적으로 수행해야할 처리 내용이 있을 때 유용
```

```jsx
new Promise(() => {}).finally(() => console.log('finally'));
```

<br />

## 🏷 프로미스의 에러 처리

```
- then 메서드의 두번째 콜백 함수로 처리 => 코드 복잡, 가독성 좋지 않음
- 또는 catch 메서드를 사용하여 처리 => 비동기 처리에서 발생한 에러 & then 메서드 내부 에러까지 모두 캐치
```

<br />

## 🏷 프로미스 체이닝

```
- 프로미스 체이닝: then, catch, finally 후속 처리 메서드는 언제나 프로미스 반환 => 연속 호출 가능
- 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과 전달받음 => 비동기 처리를 위한 콜백 패턴에서 콜백 헬 발생하지 않음
- 콜백 패턴은 가독성 좋지 않음 => ES8에서 도입된 async/await을 통해 해결 가능
```

<br />

## 🏷 프로미스의 정적 메서드

### 6.1 Promise.resolve / Promise.reject

```
- 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용
```

### 6.2 Promise.all

```
- 여러개의 비동기 처리를 병렬하게 처리할 때 사용
- 인수(배열)의 모든 프로미스가 모두 fulfilled 상태가 되면 종료
- 인수가 프로미스가 아닌 경우 Promise.resolve를 통해 프로미스로 래핑
```

```jsx
const request1 = () =>
	new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const request2 = () =>
	new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const request3 = () =>
	new Promise((resolve) => setTimeout(() => resolve(3), 1000));

Promise.all([request1(), request2(), requeest3()])
	.then(console.log)
	.catch(console.error);
```

### 6.3 Promise.race

```
- Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 이터러블을 인수로 전달받음
- 가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스 반환
- 프로미스가 rejected 상태가 되면 Promise.all과 동일하게 처리됨
```

```jsx
Promise.race([
	new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
	new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
	new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
])
	.then(console.log)
	.catch(console.log);
```

### 6.4 Promise.allSettled

```
- 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음
- 전달 받은 프로미스가 모두 settled 상태가 되면 처리 결과를 배열로 반환
```

```jsx
Promise.allSettled([
	new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error')), 2000)),
]).then(console.log);
```

<br />

## 🏷 마이크로태스크 큐

```
- 프로미스 후속 처리 메서드의 콜백 함수가 일시 저장됨
- 비동기 함수의 콜백 함수나 이벤트 핸들러 => 태스크 큐에 일시 저장
- 마이크로태스크 큐는 태스크 큐보다 우선순위 높음
- 순서
  1) 콜 스택이 비면
  2) 마이크로태스트 큐에서 대기하고 있던 함수를 가져와서 실행
  3) 마이크로태스크 큐가 비면
  4) 태스크 큐에서 대기하고 있는 함수를 가져와 실행
```

<br />

## 🏷 fetch

```
- HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API
- 비동기 처리를 위한 콜백 패턴의 단점에서 자유로움
- URI, HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등 전달
- fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체 반환
- 반환하는 프로미스는 HTTP 에러가 발생해도 reject하지 않고 ok의 상태를 false로 설정한 뒤 Response 객체를 resolve함
- 네트워크 장애 또는 CORS 에러에 의해 요청이 완료되지 못한 경우에만 reject
- axios는 모든 HTTP 에러를 reject하는 프로미스 반환, 인터셉터 / 요청 설정 등 기능 제공
```

```jsx
const url = 'https://naver.com';
fetch(url)
	.then((response) => response.json())
	.then((json) => console.log(json));
```

#### 1. GET 요청

```jsx
request
	.get(uri)
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todos) => console.log(todos))
	.catch((err) => console.error(err));
```

#### 2. POST 요청

```jsx
request
	.post(uri, {
		...data,
	})
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todos) => console.log(todos))
	.catch((err) => console.error(err));
```

#### 3. PATCH 요청

```jsx
request
	.patch(uri, {
		...data,
	})
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todos) => console.log(todos))
	.catch((err) => console.error(err));
```

#### 4. DELETE 요청

```jsx
request
	.delete(uri, {
		...data,
	})
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todos) => console.log(todos))
	.catch((err) => console.error(err));
```
