# 32. String

- 2024.8.1

## 🏷 String 생성자 함수

```
- new 연산자와 함께 호출하여 String 인스턴스 생성
- 인수 없이 new 연산자와 호출 시 [[StringData]] 내부 슬롯에 빈 문자열을 할당하여 String 래퍼 객체 생성
- String 래퍼 객체는 유사 배열 객체이자 이터러블 => 인덱스로 각 문자에 접근 가능
```

<br />

## 🏷 length 프로퍼티

```jsx
- 문자열의 문자 개수 반환
- 예) 'Hello'.length  // 5
```

<br />

## 🏷 String 메서드

```
- 문자열을 원시 값이기 때문에 문자열을 직접 변경하는 메서드는 없음
- 따라서 String 래퍼 객체는 읽기 전용 객체로 제공
```

### 3.1 String.prototype.indexOf

> 대상 문자열에서 전달받은 문자열을 검색하여 첫번째 인덱스 또는 -1 반환

```jsx
const str = 'Hello World!';
str.indexOf('l'); // 2
str.indexOf('x'); // -1
str.indexOf('l', 3); // 3
```

### 3.2 String.prototype.search

> 대상 문자열에서 전달받은 정규 표현식과 매치하는 문자열의 인덱스 또는 -1 반환

```jsx
const str = 'Hello World!';
str.search(/o/); // 4
str.search(/x/); // -1
```

### 3.3 String.prototype.includes

> 대상 문자열에 전달받은 문자열이 포함되어 있는지 확인하여 boolean 반환

```jsx
const str = 'Hello World';
str.includes('Hello'); // true
str.includes(''); // true
str.includes('x'); // false
```

### 3.4 String.prototype.startsWith

> 대상 문자열이 전달받은 문자열로 시작하는지 확인하여 boolean 반환

```jsx
const str = 'Hello World';
str.startsWith('He'); // true
str.startsWith('x'); // false
```

### 3.5 String.prototype.endsWith

> 대상 문자열이 전닯당느 문자열로 끝나는지 확인하여 boolean 반환

```jsx
const str = 'Hello World';
str.endsWith('ld'); // true
str.endsWith('x'); // false
```

### 3.6 String.prototype.charAt

> 대상 문자열에서 전달받은 인덱스에서 위치한 문자 반환 (인덱싱과 비슷)

```jsx
const str = 'Hello';
str.charAt(3); // l
```

### 3.7 String.prototype.substring

> 대상 문자열에서 첫번째 인수부터 두번째 인수까지의 부분 문자열 반환

```jsx
const str = 'Hello World!';
str.substring(1, 4); // ell
str.substring(6); // World!

// 인수가 0보다 작다면 0으로 취급
str.substring(-2); // Hello World!
```

### 3.8 String.prototype.slice

> substring 메서드와 동일, 단 음수를 인수로 전달 가능

```jsx
const str = 'Hello World!';
str.slice(0, 5); // Hello
str.slice(-5); // World
```

### 3.9 String.prototype.toUpperCase

> 대상 문자열을 모두 대문자로 변경하여 반환

```jsx
const str = 'Hello World';
str.toUpperCase(); // HELLO WORLD
```

### 3.10 String.prototype.toLowerCase

> 대상 문자열을 모두 소문자로 변경하여 반환

```jsx
const str = 'Hello World';
str.toLowerCase(); // hello world
```

### 3.11 String.prototype.trim

> 대상 문자열 앞뒤의 공백 문자를 제거하여 반환

```jsx
const str = '    foo     ';
str.trim(); // foo

// trimStart: 앞부분 공백 제거
str.trimStart(); // 'foo     '

// trimEnd: 뒷부분 공백 제거
str.trimEnd(); // '    foo'
```

### 3.12 String.prototype.repeat

> 대상 문자열을 전달받은 정수만큼 반복해 연결한 새로운 문자열 반환

```jsx
const str = 'abc';
str.repeat(); // ''
str.repeat(0); // ''
str.repeat(2); // abcabc
str.repeat(2.5); // abcabc
str.repeat(-1); // RangeError
```

### 3.13 String.prototype.replace

> 대상 문자열에서 첫번째 인수로 전달받은 문자열 또는 정규 표현식을 검색하여 두번째 인수로 치환하여 반환

```jsx
const str = 'Hello World!';
str.replace('World', 'Lee'); // Hello Lee!
str.replace(/hello/gi, 'Lee'); //Lee World!
```

### 3.14 String.prototype.split

> 첫번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리하여 배열로 반환

```jsx
const str = 'How are you doing?';
str.split(' '); // ['How', 'are', 'you', 'doing?']
str.split(''); // ['H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', ' ', 'd', 'o', 'i', 'n', 'g', '?']
str.split(' ', 3); // ['How', 'are', 'you']
```
