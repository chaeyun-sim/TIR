# 30.Date

- 2024.7.30

## 🏷 Date?

```
- 표준 빌트인 객체
- 날짜와 시간을 위한 메서드 제공 (year ~ ms)
- UTC(GMT): 국제 표준 시
- KST: UTC + 9시간
```

<br />

## 🏷 Date 생성자 함수

```
- 날짜와 시간을 나타내는 정수값을 가지는 Date 객체 생성
- 최소 날짜: 1970년 1월 1일 00:00:00
```

### 1.1 new Date

```jsx
new Date(); // Tue Jul 30 2024 22:10:14 GMT+0900 (한국 표준시)
```

```
- 인수 없이 new 연산자와 함께 호출 시 Date 객체 반환
- new 연산자 없이 호출하면 Date 객체 반환 없음
```

### 1.2 new Date(milliseconds)

```jsx
new Date(0);
new Date(86400000);
```

```
- 숫자 타입의 밀리초를 인수로 전달 => 1970년 1월 1일 00:00을 기점부터 경과한 날짜와 시간을 나타내는 Date 객체 반환
```

### 1.3 new Date(dateString)

```jsx
new Date('My 26, 2020 10:00:00');
```

```
- 문자열을 인수로 전달 시 Date 객체 반환
- 이때 전달된 문자열은 Date.parse 메서드에 의해 해석 가능해야함
```

### 1.4 new Date(year, month[, day, hour, minute, second, millisecond])

```jsx
new Date(2020, 2);
new Date('2020-02-02 12:12:00');
```

```
- 연, 월, 일, 시, 분, 초, 밀리초 전달 가능 (연, 월 필수)
```

<br />

## 🏷 Date 메서드

### 2.1 Date.now

> 1970년 1월 1일부터 현재 시간까지 경과한 밀리초(ms) 반환

```jsx
const now = Date.now(); // 1722345274794
```

### 2.2 Date.parse

> 1970년 1월 1일부터 인수로 전달된 시간까지의 밀리초(ms) 반환

```jsx
Date.parse('Jan 2, 1970 00:00:00 UTC'); // 86400000
```

### 2.3 Date.UTC

> 날짜와 시간을 각각 전달하여 현재 시간까지의 밀리초(ms) 반환

```jsx
// 단, month는 0-based index (0~11)

Date.UTC(1970, 0, 2); // 86400000
Date.UTC('1970/1/2'); // NaN
```

### 2.4 Date.prototype.getFullYear

> Date 객체의 연도 반환

```jsx
new Date('2020/07/24').getFullYear(); // 2020
```

### 2.5 Date.prototype.setFullYear

> Date 객체에 연도 설정

```jsx
const today = new Date();
today.setFullYear(2000);
today.getFullYear(); // 2000
```

### 2.6 Date.prototype.getMonth

> Date 객체의 월 반환

```jsx
new Date('2020/07/24').getMonth(); // 6
```

### 2.7 Date.prototype.setMonth

> Date 객체에 월 설정

```jsx
const today = new Date();
today.setMonth(0); // -> 1월
today.getMonth(); // 0
```

### 2.8 Date.prototype.getDate

> Date 객체의 날짜 반환

```jsx
new Date('2020/07/24').getDate(); // 24
```

### 2.9 Date.prototype.setDate

> Date 객체에 날짜 설정

```jsx
const today = new Date();
today.setDate(27);
today.getDate(); // 27
```

### 2.10 Date.prototype.getDay

> Date 객체의 요일 반환 (0~6)

```jsx
new Date('2020/07/24').getDay(); // 5
```

### 2.11 Date.prototype.getHours

> Date 객체의 시 반환

```jsx
new Date('2020/07/24/12:00').getHours(); // 12
```

### 2.12 Date.prototype.setHours

> Date 객체에 시 설정

```jsx
const today = new Date();
today.setHours(8);
today.getHours(); // 8
```

### 2.13 Date.prototype.getMinutes

> Date 객체의 분 반환

```jsx
new Date('2020/07/24/12:30').getMinutes(); // 30
```

### 2.14 Date.prototype.setMinutes

> Date 객체에 분 설정

```jsx
const today = new Date();
today.setMinutes(50);
today.getMinutes(); // 50
```

### 2.15 Date.prototype.getSeconds

> Date 객체의 초 반환

```jsx
new Date('2020/07/24/12:00:30').getSeconds(); // 30
```

### 2.16 Date.prototype.setSeconds

> Date 객체에 초 설정

```jsx
const today = new Date();
today.setSeconds(50);
today.getSeconds(); // 22:22:50
```

### 2.17 Date.prototype.getMilliseconds

> Date 객체의 밀리초 반환

```jsx
new Date('2020/07/24/12:00:30:150').getMilliseconds(); // 150
```

### 2.18 Date.prototype.setMilliseconds

> Date 객체에 밀리초 설정

```jsx
const today = new Date();
today.setMilliseconds(123);
today.getMilliseconds(); // 123
```

### 2.19 Date.prototype.getTime

> 1970년 1월 1일부터 인수까지 경과된 밀리초 반환

```jsx
new Date('2020/07/24/12:30').getTime(); // 15955614400000
```

### 2.20 Date.prototype.setTime

> 1970년 1월 1일부터 인수까지 경과된 밀리초 설정

```jsx
const today = new Date();
today.setTime(86400000);
today.getTime(); // Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

### 2.21 Date.prototype.getTimezoneOffset

> Date 객체에 지정된 locale 시간과의 차이를 분 단위로 반환

```jsx
const today = new Date();
today.getTimezoneOffset() / 60; // -9 (UTC와 KST의 차이는 -9시간)
```

### 2.22 Date.prototype.toDateString

> 사람이 읽을 수 있는 형식의 문자열로 Date 객체 반환

```jsx
const today = new Date('2020/7/24/12:30');
today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toDateString(); // Fri Jul 24 2020
```

### 2.23 Date.prototype.toTimeString

> 사람이 읽을 수 있는 형식의 문자열로 시간 반환

```jsx
const today = new Date('2020/7/24/12:30');
today.toTimeString(); // 12:30:00 GMT+0900 (대한민국 표준시)
```

### 2.24 Date.prototype.toISOString

> ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열 반환

```jsx
const today = new Date('2020/7/24/12:30');

today.toISOString(); // 2020-07-24T03:30:00.000Z
today.toISOString().slice(0, 10); // 2020-07-24
```

### 2.25 Date.prototype.toLocaleString

> 인수로 전달한 Locale을 기준으로 Date 객체의 날짜와 시간 반환

```jsx
const today = new Date('2020/7/24/12:30');

today.toLocaleString(); // 2020. 7. 24. 오후 12:30:00
today.toLocaleString('en-US'); // 7/24/2020, 12:30:00 PM
```

### 2.26 Date.prototype.toLocaleTimeString

> 인수로 전달한 Locale을 기준으로 Date 객체의 시간 반환

```jsx
const today = new Date('2020/7/24/12:30');

today.toLocaleTimeString(); // 오후 12:30:00
today.toLocaleTimeString('en-US'); // 12:300:00 PM
```
