# 43. Ajax

- 2024.8.7

## 🏷 Ajax란?

```
- Asynchronous JavaScript and XML
- 서버가 응답한 데이터로 웹페이지를 동적으로 갱신하는 프로그래밍 방식
- 브라우저에서 제공하는 XMLHttpRequest 객체를 기반으로 동작
```

<br />

## 🏷 JSON

```
- 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷
- 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷
```

### 2.1 JSON 표기 방식

```
- 키와 값으로 구성된 순수한 텍스트
- 반드시 큰따옴표로 그룹화 (작은 따옴표 불가)
```

```json
{
	"name": "Lee",
	"age": 20,
	"alive": true,
	"hobby": ["traveling", "tennis"]
}
```

### 2.2 JSON.stringify

```
- 객체를 JSON 포맷의 문자열로 변환
- 직렬화: 클라이언트가 서버로 객체 전송 시 객체를 문자열화하는 것
- 객체뿐만 아니라 JSON 포맷의 문자열도 변환 가능
```

```jsx
const obj = {
	name: 'Lee',
	age: 20,
	alive: true,
	hobby: ['traveling', 'tennis'],
};

const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {'name': 'Lee', 'age': 20, 'alive': true, 'hobby': ['traveling', 'tennis']}
```

### 2.3 JSON.parse

```
- JSON 포맷의 문자열을 객체로 변환
- 역직렬화: JSON의 포맷의 문자열을 객체화하는 것
- 배열의 요소가 객체인 경우 요소까지 객체로 변환
```

```jsx
const obj = {
	name: 'Lee',
	age: 20,
	alive: true,
	hobby: ['traveling', 'tennis'],
};

const json = JSON.stringify(obj);
const result = JSON.parse(json);
console.log(typeof result, result);
// object {'name': 'Lee', 'age': 20, 'alive': true, 'hobby': ['traveling', 'tennis']}
```
