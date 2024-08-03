# 37. Set과 Map

- 2024.8.3

## 🏷 Set

> 중복되지 않는 유일한 값들의 집합

| 구분                                | 배열 | Set |
| ----------------------------------- | ---- | --- |
| 동일한 값을 중복하여 포함할 수 있다 | O    | X   |
| 요소 순서에 의미가 있다             | O    | X   |
| 인덱스로 요소에 접근할 수 있다      | O    | X   |

### 1.1 Set 객체의 생성

```
- Set 생성자 함수로 Set 객체 생성, 이터러블을 인수로 받음
- 중복된 값은 Set 객체에 요소로 저장되지 않음
```

```jsx
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {'h', 'e', 'l', 'l', 'o'}

const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

### 1.2 요소 개수 확인

> Set.prototype.size 프로퍼티 사용

```
- size 프로퍼티: setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티
- 즉, Set 객체의 요소 개수를 변경할 수 없음
```

```jsx
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

### 1.3 요소 추가

> Set.prototype.add 메서드 사용

```
- 새로운 요소가 추가된 Set 객체 반환 => add 메서드를 연속적으로 호출 허용
- 중복된 요소 추가 안됨 (무시된다)
- NaN과 NaN, +0와 -0는 같다고 평가하여 중복 추가를 허용하지 않음
- 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장 가능
```

```jsx
const set = new SEt();
set.add(1).add(2);
console.log(set); // Set(2) {1, 2}
```

### 1.4 요소 존재 여부 확인

> Set.prototype.has 메서드 사용

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(5)); // false
```

### 1.5 요소 삭제

> Set.prototype.delete 메서드 사용

```
- 삭제하려는 요소 값을 인수로 전달
- 존재하지 않는 요소 삭제 시 무시
- 삭제 성공 여부를 나타내는 불리언 값 반환 => 연속적으로 호출 불가
```

```jsx
const set = new Set([1, 2, 3]);
set.delete(2);
console.log(set); // Set(2) {1, 3}

set.delete(1).delete(3); // TypeError
```

### 1.6 요소 일괄 삭제

> Set.prototype.clear 메서드 사용

```jsx
const set = new Set([1, 2, 3]);
set.clear();
console.log(set); // Set(0) {}
```

### 1.7 요소 순회

> Set.prototype.forEach 메서드 사용

```
- Array.prototype.forEach와 유사
- forEach 메서드의 콜백함수 내부에서 this로 사용될 객체를 인수로 전달
- Set.prototype.forEach(현재 순회 중 요소값1, 현재 순회 중 요소값2, Set 객체 자체)
- Set 객체는 이터러블 => for...of문으로 순회 가능, 스프레드 문법과 배열 디스트럭처링 가능
```

```jsx
const set = new Set([1, 2, 3]);
set.forEach((v, v2, set) => console.log(v)); // 1 2 3

console.log(Symbol.iterator in set); // true

for (const value of set) {
  console.log(value); // 1 2 3
}

console.log([...set]); // [1, 2, 3]
```

### 1.8 집합 연산

#### 교집합

```jsx
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB)); // Set(2) {2, 4}
```

#### 합집합

```jsx
Set.prototype.union = function (set) {
  const result = new Set(this);

  for (const value of set) {
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

#### 차집합

```jsx
Set.prototype.difference = function (set) {
  const result = new Set(this);

  for (const value of set) {
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // Set(2) {1, 3}
console.log(setB.difference(setA)); // Set(0) {}
```

#### 부분 집합과 상위 집합

> 집합 A가 집합 B에 포함되는 경우, 집합 A는 부분집합, 집합 B는 상위 집합

```jsx
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    if (!this.has(value)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.isSuperset(setB)); // true
console.log(setB.isSuperset(setA)); // false
```

<br />

## 🏷 Map

> 키와 값의 쌍으로 이루어진 컬렉션

| 구분             | 객체                    | Map 객체              |
| ---------------- | ----------------------- | --------------------- |
| 키로 사용하는 값 | 문자열 또는 심벌        | 객체를 포함한 모든 값 |
| 이터러블         | X                       | O                     |
| 요소 개수 확인   | Object.keys(obj).length | map.size              |

### 2.1 Map 객체의 생성

```
- 이터러블을 인수로 전달받아 Map 객체 생성 => 이터러블: 키와 값의 쌍으로 이루어진 요소로 구성
- 중복된 키를 갖는 요소 존재 시 값이 덮어씌워짐
```

```jsx
const map = new Map([
  ['key1', 'value'],
  ['key2', 'value2'],
]);
console.log(map); // Map(2) {'key1' => 'value1', 'key2' => 'value2'}
```

### 2.2 요소 개수 확인

> Map.prototype.size 프로퍼티 사용

```
- size 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티
- 따라서 Map 객체의 요소 개수를 직접 변경할 수 없음
```

```jsx
const { size } = new Map([
  ['key1', 'value'],
  ['key2', 'value2'],
]);
console.log(size); // 2
```

### 2.3 요소 추가

> Map.prototype.set 메서드 사용

```
- 새로운 요소가 추가된 Map 객체 반환 => 연속적 호출 가능
- 중복된 키를 갖는 요소 추가 시 에러 발생하지 않으나 값이 덮어쓰여짐
- NaN과 NaN, +0와 -0는 같은 값으로 평가하여 중복 추가를 허용하지 않음
- Map 객체는 키 타입에 제한 없음
```

```jsx
const map = new Map();

map.set('key1', 'value1').set('key2', 'value2');
console.log(map); // Map(2) {'key1' => 'value1', 'key2' => 'value2'}

const map1 = new Map()
map1.set('key1', 'value'1).set('key1', 'value2')
console.log(map)  // Map(1) {'key1' => 'value2'}
```

### 2.4 요소 취득

> Map.prototype.get 메서드 사용

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

### 2.5 요소 존재 여부 확인

> Map.prototype.has 메서드 사용

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

### 2.6 요소 삭제

> Map.prototype.delete 메서드 사용

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');
map.delete(kim);

console.log(map); // Map(1) { {name: 'Lee'} => 'developer' }
```

### 2.7 요소 일괄 삭제

> Map.prototype.clear 메서드 사용

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

map.clear();
console.log(map); // Map(0) {}
```

### 2.8 요소 순회

> Map.prototype.forEach 메서드 사용

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

map.forEach((v, k, map) => console.log(v, k));
/*
developer {name: 'Lee'}
designer {name: 'Kim'}
*/

console.log(Symbol.iterator in map); // true

for (const entry of map) {
  console.log(entry); // [{name: 'Lee'}, 'developer'] [{name: 'Kim'}, 'designer']
}

console.log([...map]); // [{name: 'Lee'}, 'developer'] [{name: 'Kim'}, 'designer']
```

| Map 메서드            | 설명                                                 |
| --------------------- | ---------------------------------------------------- |
| Map.prototype.keys    | Map 객체에서 요소키를 값으로 갖는 배열 반환          |
| Map.prototype.values  | Map 객체에서 요소값을 값으로 갖는 배열 반환          |
| Map.prototype.entries | Map 객체에서 요소키와 요소값을 값으로 갖는 배열 반환 |
