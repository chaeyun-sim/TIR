# 31. RegExp

- 2024.7.31

## 🏷 정규 표현식이란?

```
- 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어
- 대부분읜 프로그래밍 언어와 코드 에디터에 내장
- 패터 매칭 기능: 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환
```

<br />

## 🏷 정규 표현식의 생성

```
- 정규 표현식 리터럴: /regexp/i => (시작기호 패턴 종료기호 플래그)
- RegExp 생성자 함수를 사용하여 RegExp 객체를 생성하여 사용할 수도 있음
```

```jsx
const target = 'Is this all right?';
const regexp = /this/i;
regexp.test(target); // true

const target = 'Is this all right?';
const regexp = new Regexp(/this/i);
regexp.test(target); // true
```

<br />

## 🏷 RegExp 메서드

### 3.1 RegExp.prototype.exec

> 전달받은 문자열에 대해 정규 표현식 패턴을 검색하여 배열로 반환, 없는 경우 null

```jsx
const target = 'Is this all right?';
const regExp = /this/;
regExp.exec(target);
// ['this', index: 3, input: "Is this all right?", groups: undefined]
```

### 3.2 RegExp.prototype.test

> 전달받은 문자열에 대해 정규 표현식 패턴을 검색하여 boolean 값으로 반환

```jsx
const target = 'Is this all right?';
const regExp = /this/;
regExp.test(target); // true
```

### 3.3 String.prototype.match

> String 표준 빌트인 객체, 대상 문자열과 전달받은 정규 표현식과의 매칭 결과를 배열로 반환

```jsx
const target = 'Is this all right?';
const regExp = /is/;
target.match(regExp); // -> exec와 결과값은 동일
```

<br />

## 🏷 플래그

> 정규 표현식의 검색 방식 설정

1. `i`gnore case: 대소문자 구별 없이 패턴 검색
2. `g`lobal: 패턴과 일치하는 모든 문자열을 전역 검색
3. `m`ulti line: 문자열 행이 바뀌더라도 패턴 검색 지속

```
- 플래그는 선택적으로 사용 가능
- 순서와 관계없이 동시에 설정할 수도 있음
```

```jsx
const target = 'This is a simple example that includes this word three times: this.';

target.match(/this/gi); // ['This', 'this', 'this']
target.match(/this/g); // ['this', 'this']
target.match(/this/i);
// ['This', index: 0, input: 'This is a simple example that includes this word three times: this.', groups: undefined]
target.match(/this/m);
// ['this', index: 39, input: 'This is a simple example that includes this word three times: this.', groups: undefined]
```

<br />

## 🏷 패턴

> 형태: /regexp/

### 5.1 문자열 검색

```
- 플래그를 생략한 경우 첫번째 결과만 반환
```

### 5.2 임의의 문자열 검색

```
- (.) => 문자 한 개를 의미
- 예) ... => 문자의 내용과 상관 없이 3자리 문자열
```

```jsx
const target = 'Is there anything you want?';
const regExp = /.../g;

target.match(regExp);
// ['Is ', 'the', 're ', 'any', 'thi', 'ng ', 'you', ' wa', 'nt?']
```

### 5.3 반복 검색

```
- {m, n} => 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열
- + => 앞선 패턴이 최소 1번 이상 반복되는 문자열 (+ == {1,})
- 예) A+ => A, AA, AAA ...
- ? => 앞선 패턴이 0 또난 1번 반복되는 문자열 (? == {0, 1})
- 예) /colou?r/ => color, colour
```

```jsx
const target = '010-1234-5678';
const regExp = /[0-9]{3}-[0-9]{3,4}-[0-9]{4}/g;
// ['010-1234-5678']
```

### 5.4 OR 검색

```jsx
const target = 'A AA B BB Aa Bb';
const regExp = /A|B/g;
target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']

const regExp2 = /A+|B+/g;
target.match(regExp2); // ['A', 'AA', 'B', 'BB', 'A', 'B']

const regExp3 = /[AB]+/g;
target.match(regExp3); // ['A', 'AA', 'B', 'BB', 'A', 'B']

const regExp4 = /[A-Za-z]+/g;
target.match(regExp4); // ['AA', 'BB', 'Aa', 'Bb']
```

- `\d`: 숫자 (0-9)
- `\D`: 숫자가 아닌 문자
- `\w`: 알파벳, 숫자, 언더스코어 ([A-Za-z0-9_])
- `\W\`: 언더스코어를 제외한 특수문자

### 5.5 NOT 검색

```jsx
const target = 'AA BB 12 Aa Bb';
const regExp = /[^0-9]+/g;
target.match(regExp); // ['AA BB ', ' Aa Bb']
```

### 5.6 시작 위치로 검색

```jsx
const target = 'https://pokemonweb.com';
const regExp = /^https/;
regExp.test(target); // true
```

### 5.7 마지막 위치로 검색

```jsx
const target = 'https://pokemonweb.com';
const regExp = /com$/;
regExp.test(target); // true
```

<br />

## 🏷 자주 사용하는 정규표현식

### 6.1 특정 단어로 시작하는지 검사

```jsx
const url = 'https://example.com'
/^https?:\/\//.test(url)  // true
```

### 6.2 특정 단어로 끝나는지 검사

```jsx
const fileName = 'index.html'
/html$/.test(fileName)  // true
```

### 6.3 숫자로만 이루어진 문자열인지 검사

```jsx
const target = '12345';
/^\d+$/.test(target); // true
```

### 6.4 하나 이상의 공백으로 시작하는지 검사

```jsx
const target = ' Hi!'
/^[\s]+/.test(target)  // true
```

### 6.5 아이디로 사용 가능한지 검사

```jsx
const id = 'abc123';
/^[A-Za-z0-9]{4,10}$/.test(id); // true
```

### 6.6 메일 주소 형식에 맞는지 검사

```jsx
const email = 'user@gmail.com'
/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email)  // true
```

### 6.7 핸드폰 번호 형식에 맞는지 검사

```jsx
const cellphone = '010-1234-5678'
/^\d{3}-\d{3, 4}-\d{4}$/.test(cellphone)
```

### 6.8 특수문자 포함 여부 검사

```jsx
const target = 'abc#123'(/[^A-Za-z0-9]/gi).test(target); // true
```
