# 29. Math

- 2024.7.29

## 🏷 Math 프로퍼티

### 1.1 Math.PI

> 원주율 값 반환

```jsx
Math.PI; // 3.141592...
```

<br />

## 🏷 Math 메서드

### 2.1 Math.abs

> 인수로 전달된 숫자의 절대값 반환 (반드시 0 또는 0 이상의 양수)

```jsx
Math.abs(-1); // 1
Math.abs('-1'); // 1
Math.abs(''); // 0
```

### 2.2 Math.round

> 인수로 전달된 숫자의 소수점 이하를 반올림한 정수 반환

```jsx
Math.round(1.4); // 1
Math.round(-1.6); // -2
Math.round(); // NaN
```

### 2.3 Math.ceil

> 인수로 전달된 숫자의 소수점 이하를 올림한 정수 반환

```jsx
Math.ceil(1.4); // 2
Math.ceil(-1.6); // -1
Math.ceil(); // NaN
```

### 2.4 Math.floor

> 인수로 전달된 숫자의 소수점 이하를 내림한 정수 반환

```jsx
Math.ceil(1.9); // 1
Math.ceil(9.1); // 9
Math.ceil(); // NaN
```

### 2.5 Math.sqrt

> 인수로 전달된 숫자의 제곱근 반환

```jsx
Math.sqrt(9); // 2
Math.sqrt(-9); // NaN
Math.sqrt(2); // 1.4142....
```

### 2.6 Math.random

> 임의의 난수 반환 (0 이상 1 미만의 실수)

```jsx
Math.random; // 예: 0.8208720231391746

Math.floor(Math.random() * 10 + 1); // 1에서 10 범위의 정수
```

### 2.7 Math.pow

> 첫 번째 인수를 base으로, 두 번째 인수를 exponent로 거듭제곱한 결과 반환

```jsx
Math.pow(2, 8); // 256

// ES7에서 도입된 지수 연산자를 사용하면 가독성이 더 좋음
2 ** (2 ** 2); // 16
Math.pow(Math.pow(2, 2), 2); // 16
```

### 2.8 Math.max

> 전달받은 인수 중에서 가장 큰 수 반환, 인수가 없다면 -Infinity 반환

```jsx
Math.max(1, 2, 3); // 3
Math.max(); // -Infinity

const arr = [1, 2, 3, 4];
Math.max(...arr); // 4
```

### 2.9 Math.min

> 전달받은 인수 중에서 가장 작은 수 반환, 인수가 없다면 Infinity 반환

```jsx
Math.min(1, 2, 3); // 1
Math.min(); // Infinity

const arr = [1, 2, 3, 4];
Math.min(...arr); // 1
```
