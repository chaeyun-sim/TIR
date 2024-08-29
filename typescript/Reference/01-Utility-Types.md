# Utility Types

## Awaited\<Type>

- 버전: 4.5 ~

이 타입은 `async` 함수 내의 `await` 연산자나 `Promise`의 `.then()`메서드를 재귀적으로 처리하는 방법을 모델링한다.

```typescript
type A = Awaited<Promise<string>>;
// type A = string

type B = Awaited<Promise<Promise<number>>>;
// type B = number

type C = Awaited<boolean | Promise<number>>;
// type C = number | boolean;
```

<br />

> 🤔 <Awaited<Promise\<string>>>과 string의 차이점이 뭘까?

string은 단순한 문자열 타입을 나타내는 반면, <Awaited<Promise\<string>>> Promise<string>이 비동기적으로 해결되었을 때의 타입을 나타낸다.

```typescript
let message: string = 'Hello, world!';
async function getMessage(): Promise<string> {
  return 'Hello World';
}
```

```typescript
async function fetchData(): Promise<string> {
  return 'Hello, world!';
}
type Result = Awaited<ReturnType<typeof fetchData>>;
// 이때 Result는 string
```

비동기 함수의 반환 타입이 복잡할 때 `Awaited`를 사용하면 실제 반환 타입을 알 수 있다.

<br />

## Partial\<Type>

- 버전: 2.1 ~

`Type`의 모든 프로퍼티를 optional하게 만든다. 이 유틸리티는 주어진 타입의 모든 부분집합을 표시하는 타입을 반환한다.

```typescript
interface Todo {
  title: string;
  description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
  title: 'organize desk',
  description: 'clean clutter',
};

const todo2 = updateTodo(todo1, {
  description: 'throw out trash',
});
```

<br />

## Required\<Type>

- 버전: 2.8 ~

`Type`의 모든 프로퍼티를 필수 사항으로 구성한다. `Required` 유틸리티의 반대는 `Partial`이다.

```typescript
interface Props {
  a?: number;
  b?: string;
}

const obj: Props = { a: 5 };
const obj2: Required<Props> = { a: 5 };
// Property 'b' is missing in type '{ a : number; }' but required in type 'Required<Props>'.
```

<br />

## Readonly\<Type>

- 버전: 2.1 ~

`Type`의 모든 프로퍼티를 `읽기 전용`으로 만든다. 즉, 타입의 프로퍼티를 재할당할 수 없다.

```typescript
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = {
  title: 'Delete inactive users',
};

todo.title = 'Hello';
// Cannot assign to 'title' because it is a read-only property.
```

이 유틸리티는 런타임에서 할당식이 실패하는 경우를 나타내는 데 유용하다. (예: freeze된 객체를 재할당할 때)

```typescript
function freeze<Type>(obj: Type): Readonly<Type>;
```

<br />

## Record<Keys, Type>

- 버전: 2.1 ~

`Keys`가 프로퍼티 키이고 `Type`이 프로퍼티 값인 객체 타입을 생성한다. 이 유틸리티를 사용하여 다른 타입에 매핑할 수 있다.

```typescript
type CatName = 'miffy' | 'boris' | 'mordred';

interface CatInfo {
  age: number;
  breed: string;
}

const cats: Record<Catname, CatInfo> = {
  miffy: { age: 10, breed: 'Persian' },
  boris: { age: 5, breed: 'Maine Coon' },
  mordred: { age: 16, breed: 'british Shorthair' },
};

cats.boris; // const cats: Record<CatName, CatInfo>
```

<br />

## Pick<Type, Keys>

- 버전: 2.1 ~

`Type`에서 `Keys`에 해당하는 일련의 프로퍼티를 추출하여 사용한다.

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, 'title' | 'completed'>;

const todo: TodoPreview = {
  title: 'Clean room',
  completed: false,
};

todo; // const todo: TodoPreview;
```

<br />

## Omit<Type, Keys>

- 버전: 3.5 ~

`Type`의 모든 프로퍼티를 선택한 다음 `Keys`에 해당하는 프로퍼티를 제거하여 사용한다. `Omit`의 반대는 `Pick`이다.

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}

type TodoPreview = Omit<Todo, 'description'>;

const todo: TodoPreview = {
  title: 'Clean room',
  completed: false,
  createdAt: 1615544252770,
};

todo; // const todo: TodoPreview

const newTodo: TodoPreview = {
  title: 'Clean room',
  description: 'this is a clean room',
  completed: false,
  createdAt: 1615544252770,
};

newTodo; // Object literal may only specify known properties, and 'description' does not exist in type 'TodoPreview'.
```

<br />

## Exclude<UnionType, ExcludedMembers>

- 버전: 2.8 ~

`UnionType`에서 `ExcludedMembers`의 타입을 제외하여 생성한다.

```typescript
type T0 = Exclude<'a' | 'b' | 'c', 'a'>;
// type T0 = "b" | "c"

type T1 = Exclude<'a' | 'b' | 'c', 'a' | 'b'>;
// type T1 = "c"

type T2 = Exclude<string | number | (() => void), Function>;
// type T2 = string | number

type Shape =
  | { kind: 'circle'; radius: number }
  | { kind: 'square'; x: number }
  | { kind: 'triangle'; x: number; y: number };

type T3 = Exclude<Shape, { kind: 'circle' }>;
// type T3 = {
// 	kind: "square";
// 	x: number;
// } | {
// 	kind: "triangle";
// 	x: number;
// 	y: number;
// }
```

> 🤔 Omit vs. Exclude

Omit: 특정 프로퍼티 제거하여 새로운 객체 타입을 생성<br />Exclude: 유니온 타입에서 특정 타입을 제거하여 새로운 유니온 타입 생성

<br />

## Extract<Type, Union>

- 버전: 2.8 ~

`Type`에서 `Union` 멤버를 추출하여 새로운 `Type`을 생성한다. `Extract`의 반대는 `Exclude`이다.

```typescript
type T0 = Extract<'a' | 'b' | 'c', 'a' | 'f'>;
// type T0 = "a"

type T1 = Extract<string | number | (() => void), Function>;
// type T1 = () => void

type Shape =
  | { kind: 'circle'; radius: number }
  | { kind: 'square'; x: number }
  | { kind: 'triangle'; x: number; y: number };

type T2 = Extract<Shape, { kind: 'circle' }>;
// type T2 = {
// 	kind: "circle",
// 	radius: number;
// }
```

<br />

## NonNullable\<Type>

- 버전: 2.8 ~

`Type`에서 `null`과 `undefined`을 제외하여 생성한다.

```typescript
type T0 = NonNullable<string | number | undefined>;
// type T0 = string | number;

type T1 = NonNullable<string[] | null | undefined>;
// type T1 = string[]
```

> 🤔 NonNullable vs. 타입 단언 연산자 (!) vs. 널 병합 연산자 (??) vs. as 키워드의 타입 단언

- NonNullable: null과 undefined를 제거하여 타입 시스템이 자동으로 처리하도록 한다. (주로 타입 정의에서 사용)
- 타입 단언 연산자 (!): null 또는 undefined가 아님을 강제로 타입 시스템에 알린다.
- 널 병합 연산자(??): null 또는 undefined일 때 기본 값을 제공한다.
- 타입 단언(as 키워드): 타입을 강제로 지정할 때 사용한다.

> 💡 [Partial<T>와 NonNullable<T>](https://medium.com/@gabrielairiart.gi/the-power-of-nonnullable-t-in-typescript-9cf156beb8da)

```typescript
interface User {
  id: number;
  name: string;
  email: string | null;
}

type SafePartialUser = Partial<NonNullable<User>>;

const user: SafePartialUser = {
  id: 1,
  name: 'John Doe',
  // email은 선택사항이지만 값이 있다면 반드시 string이어야 한다.
};
```

<br />

## Parameters\<Type>

- 버전: 3.1 ~

함수 타입 `Type`의 파라미터 타입으로 튜플 타입을 구성한다. 오버로드된 함수의 경우, `Parameters<Type>`이 반환하는 타입은 마지막에 정의된 함수 시그니처의 매개변수 타입이다.

```typescript
declare function f1(arg: { a: number; b: string });

type T0 = Parameters<() => string>;
// type T0 = []

type T1 = Parameters<(s: string) => void>;
// type T1 = [s: string]

type T4 = Parameters<any>;
// type T4 = unknown[]

type T5 = Parameters<never>;
// type T5 = never
```

- 실제 예시

```typescript
// 매개변수 타입 추출하기
function add(a: number, b: number): number {
  return a + b;
}

type AddParameters = Parameters<typeof add>; // [number, number]

// 매개변수 타입으로 새로운 함수 타입 정의
function greet(name: string, age: number): string {
  return `Hello ${name}, you are ${age} years old.`;
}

type GreetParams = Parameters<typeof greet>;

type GreetFn = (...args: GreetParams) => string;
```

<br />

## ConstructorParameters\<Type>

- 버전: 3.1 ~

생성자 함수 타입 `Type`의 매개변수 타입으로 튜플이나 배열 타입을 선택한다.

```typescript
type T0 = ConstructorParameters<ErrorConstructor>;
// type T0 = [message?: string]

class C {
  constructor(a: number, b: string) {}
}

type T3 = ConstructorParameters<typeof C>;
// type T3 = [a: number, b: string]

type T4 = ConstructorParameters<any>;
// type T4 = unknown[]
```

<br />

## ReturnType\<Type>

- 버전: 2.8 ~

함수 `Type`의 반환 타입으로 새 타입을 생성한다. 오버로드된 함수의 경우 `ReturnType<Type>`이 반환하는 타입은 마지막에 정의된 함수 시그니처의 타입이다.

```typescript
declare function f1(): { a: number; b: string };

type T0 = ReturnType<() => string>;
// type T0 = string

type T1 = ReturnType<(s: string) => void>;
// type T1 = void

type T4 = ReturnType<typeof f1>;
// type T4 = {
// 	a: number;
// 	b: string;
// }
```

<br />

## InstanceType\<Type>

- 버전: 2.8 ~

`Type`의 인스턴스 타입을 추출한다.

```typescript
class C {
  x = 0;
  y = 0;
}

type T0 = InstanceType<typeof C>;
// type T0 = C

type T1 = InstanceType<any>;
// type T1 = any

type T2 = InstanceType<string>;
// Type 'string' does not satisfy the constraint 'abstract new (...args: any) => any'.
```

> 🤔 any, never은 에러가 발생하지 않는데 string, Function은 에러가 발생하는 이유가 뭘까?

- 주어진 타입이 생성자 함수일 때 인스턴스 타입을 추출할 수 있다.
- any, never은 생성자 함수가 아니지만 특수한 처리로 평가함
  - any: 모든 타입을 포함할 수 있는 타입 (제약 우회 가능)
  - never: 어떤 값도 가질 수 없는 타입 (타입 자체가 존재하지 않음)
  - undefined: 정의되지 않은 값 (everything / nothing도 아닌 none)

<br />

## NoInfer\<Type>

- 버전: 5.4 ~

보유한 타입에 대한 추론을 막는다. 그 외에는 `Type`과 동일하다.

```typescript
function createStreetLight<C extends string>(color: C[], defaultColor?: NoInfer<C>) {
  // ...
}

createStreetLight(['red', 'yellow', 'green'], 'red'); // OK
createStreetLight(['red', 'yellow', 'green'], 'blue'); // Error
```

<br />

## ThisParameterType\<Type>

- 버전: 3.3 ~

함수 타입의 `this` 파라미터의 타입을 추출하거나 함수 타입에 `this` 매개변수가 없는 경우 `unknown`을 반환한다.

```typescript
function toHex(this: Number) {
  return this.toString(16);
}

function numberToString(n: ThisParameterType<typeof toHex>) {
  return toHex.apply(n);
}

// typeof toHex는 toHex 함수의 타입을 가져온다. 이때 toHex는 this: Number를 포함한 함수 타입
// ThisParameterType<typeof toHex>는 toHex 함수의 this 매개변수 타입을 추출한다.
```

<br />

## OmitThisParameter\<Type>

- 버전: 3.3 ~

`Type`에서 `this` 파라미터를 제거한다. 만약 `Type`에 명시적으로 정의된 `this` 파라미터가 없다면 `Type`을 반환하고, 그렇지 않으면 `this` 파라미터를 제외한 새 함수 타입이 생성된다.

```typescript
function toHex(this: Number) {
  return this.toString(16);
}

const fiveToHex: OmitThisParameter<typeof toHex> = toHex.bind(5);
// 기존 this 값을 삭제하고 5를 this 값으로 바인딩한다.

console.log(fiveToHex()); // "5"
```

<br />

## ThisType\<Type>

- 버전: 2.3 ~

이 유틸리티 함수는 변환된 타입을 반환하지 않는다. 그 대신, 문맥적 `this` 타입의 마커 역할을 한다. `noImplicitThis` 플래그는 반드시 활성화 되어있어야 한다.

```typescript
type ObjectDescriptor<D, M> = {
  data?: D;
  methods?: M & ThisType<D & M>;
};

function makeObject<D, M>(desc: ObjectDescriptor<D, M>): D & M {
  let data: object = desc.data || {};
  let methods: object = desc.methods || {};
  return { ...data, ...methods } as D & M;
}

let obj = makeObject({
  data: { x: 0, y: 0 },
  methods: {
    moveBy(dx: number, dy: number) {
      this.x += dx;
      this.y += dy;
    },
  },
});

obj.x = 10;
obj.y = 20;
obj.moveBy(5, 5);
```

위의 예시에서, makeObject 함수에 대한 methods 객체의 매개변수는 `ThisType<D & M>`을 포함하는 컨텍스트 타입을 가지며, `methods` 객체 내의 메서드에서 `this` 타입은 `{x : number, y : number } & {moveBy(dx: number, dy: number): void}`이다. 주목할 점은 `methods` 속성의 타입이 동시 추론 대상이자 this 타입의 출처라는 것이다.

`ThisType<T>` 마커 인터페이스는 `lib.d.ts`에 정의된 빈 인터페이스이다. 개체 리터럴의 컨텍스트 타입에서 인식되는 것을 넘어 이 인터페이스는 빈 인터페이스처럼 동작한다.

<br />

## 내장 문자열 조작을 위한 타입

- Uppercase<StringType>
- Lowercase<StringType>
- Capitalize<StringType>
- Uncapitalize<StringType>
