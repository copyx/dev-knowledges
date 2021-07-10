# TypeScript

https://www.typescriptlang.org/

자바스크립트에 타입을 추가한 언어. 컴파일 과정을 통해 실행가능한 자바스크립트 코드로 변환. 컴파일(Compile)한다고 하지만 전통적인 컴파일과 좀 다름. 그래서 트랜스파일(Transpile)이라는 용어를 많이 사용.

원래 자바스크립트는 Interpreted 언어. 쉽게 말하면 한 줄 한 줄 실행. 그래서 버그를 런타임에 찾아야함. 하지만 타입스크립트는 컴파일 과정을 통해 컴파일 타임에 버그를 찾아낼 수 있음.

## Install

대표적으로 두 가지 방법을 사용.

### By Package Manager(NPM, Yarn)

#### Global

```bash
# 설치
npm install typescript -g
yarn global add typescript

# 삭제
npm uninstall typescript -g
yarn global remove typescript
```

#### Project

```bash
# 프로젝트 초기화
npm init # 귀찮으면 -y 옵션
yarn init

# 설치
npm install typescript -D
yarn add typescript -D
```

타입스크립트는 보통 실행 환경이 아닌 개발환경에서 필요함. 그래서 devDependency에 추가.

## Compile

특정 파일을 컴파일 할 때는 파일을 명시

```bash
tsc source.ts
# source.js 생성됨
```

프로젝트 범위의 파일들을 한 번에 컴파일 하고 싶다면 프로젝트 디렉토리 내에 설정 파일(tsconfig.json)이 필요함. 이는 명령을 통해 생성 가능. 설정 파일이 있는 상태에서 `tsc` 명령만 쳐주면, 설정에 명시된 대로 컴파일 진행.

```bash
tsc --init # tsconfig.json 생성
tsc        # 디렉토리 내 .ts 파일 컴파일 진행
```

`-w`(`--watch`) 옵션을 통해 소스코드에 변화가 있을 시 자동 컴파일 가능.

```bash
tsc -w
```

## VS Code

타입스크립트 컴파일러가 내장되어있음. VS Code 버전이 올라가면 컴파일러 버전도 올라감. 내장 컴파일러를 사용할 것인지, 외부 컴파일러를 사용할 것인지 선택 가능.

> 쉘에서 `code` 명령어를 사용하려면 PATH를 추가해야함.<br/>
> VS Code의 커맨드 팔레트에서 `Shell Command: Install 'code' command in PATH`를 검색해 실행시키면 완료.<br/>
> 출처: https://code.visualstudio.com/docs/setup/mac

## [Types by Inference(추론)](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#types-by-inference)

타입스크립트는 타입을 지정하지 않으면, 처음 저장되는 데이터의 타입으로 변수 타입을 지정한다.

```typescript
let a = "Jake"; // 암시적으로
a = 1;
```

## Type Annotation

변수 뒤에 콜론(`:`)으로 타입을 표현할 수 있음.

## Basic Types

### TypeScript Types VS Javascript Types

타입스크립트는 Static Typing(Compile-time), 자바스크립트는 Dynamic Typing(Run-time).

타입스크립트는 자바스크립트의 타입들을 지원하며, 프로그래밍을 도울 몇 가지 타입이 더 제공됨.

- Javascript 기본 자료형 (From ECMAScript Standard)
  - Boolean
  - Number
  - String
  - Null
  - Undefined
  - Symbol (ECMAScript 6에서 추가됨)
  - Array : object 형
- TypeScript에서 추가로 제공하는 자료형
  - Any, Void, Never, Unknown
  - Enum
  - Tuple : object 형

### Primitive Types

Object, Reference 형태가 아닌 실제 값을 저장하는 자료형.

#### Literal(코드에서 값을 문자로 표시하는 방법)

Primitive 타입의 서브 타입을 나타낼 수 있음.

```typescript
true;
3.14;
("test string");
null;
undefined;
```

#### Wrapper Object

실제 만들어진 데이터의 타입은 object. **타입스크립트에서는 이런 래퍼 객체 사용을 권장하지 않음.**

```typescript
new Boolean(false);
new String("test string");
new Number(3.14);
```

#### Type Casing

타입스크립트의 핵심 primitive 타입은 모두 소문자. Number, String 등 대문자로 시작하는 객체 래퍼들은 Primitive 타입이 아니므로 되도록이면 타입으로 사용해서는 안됨.

### Primitive 타입의 종류

#### boolean

true/false

```typescript
let isDone: boolean = false;
isDone = true;
console.log(typeof isDone); // 'boolean'

let isOk: Boolean = true;
// let isNotOk: boolean = new Boolean(true); // 래퍼 객체라서 할당 불가능. 타입이 안맞음.
```

#### number

- 모든 숫자는 부동 소수점 값
- 2, 8, 10, 16진수 모두 지원
- NaN
- 1_000_000 같은 underscore 포함 표현 가능

```typescript
let decimal: number = 10;
let hex: number = 0x1a;
let binary: number = 0b101101;
let octal: number = 0o7171;

let notANumber: number = NaN;

let underscoreNum: number = 1_000_000;
```

#### string

- '', ""
- Template Literals

```typescript
let quote: string = "Quote";
let doubleQuote: string = "Double quote";
let templateLiterals: string = `Template literals: ${quote}`;

console.log(quote, doubleQuote, templateLiterals);
```

#### symbol ([MDN Link](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol))

- ECMAScript 6에서 추가됨
- `Symbol('symbol')`로 생성(`new Symbol`로 생성 불가)
- Symbol 함수를 사용하려면 타입스크립트 설정(`lib: [ES2015]`) 필요
- 원시타입의 값을 담아서 사용.
- 고유하고 수정불가능한 값으로 만들어줌. (그래서 주로 접근 제어에 많이 사용)

```typescript
console.log(Symbol("foo") === Symbol("foo"));

const sym = Symbol();

const obj = {
  sym: "value",
  [sym]: "symbol value",
};

console.log(obj["sym"]);
console.log(obj[sym]);
```

#### null & undefined

타입도 값도 모두 소문자. 각각 null과 undefined라는 값 밖에 가지지 못함.

아무 설정도 하지 않은 상태에서 둘은 **모든 다른 타입들의 서브타입**임. (예: number에 null이나 undefined 할당 가능)<br/>
컴파일 시 `--strictNullChecks` 옵션을 추가하면, **void(undefined 만 가능)나 자기 자신들에게만 할당 가능**. 이 경우 다른 타입들에 둘을 할당하고 싶으면 `union type`을 이용해야함.

```typescript
// let myName: string = undefined;

// let u: undefined = null;
let v: void = undefined;

let union: string | null = null; // union type

union = "Mark";
```

자바스크립트 런타임에서 typeof 연산자로 확인해보면, null은 'object', undefined는 'undefined'라고 나옴.

> 왜 null의 타입은 'object'일까?
>
> https://curryyou.tistory.com/183<br/>
> 결론: typeof 연산자 버그. null의 타입을 체크하는 항목이 누락.

#### object

원시 타입이 아닌 타입(Non-primitive type)이 아닌 것을 나타내고 싶을 때 사용하는 타입.

```typescript
const obj1: object = { a: 1, b: "ss" }; // 가능하지만 거의 안씀
const obj2: { a: number; b: string } = { a: 1, b: "ss" };
```

##### [Optional Properties](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#optional-properties)

프로퍼티가 없어도 괜찮다는 표현 가능.

```typescript
function optionalProps(obj: { a: string; b?: string }) {}

// 둘 다 괜춘쓰
optionalProps({ a: "aa" });
optionalProps({ a: "aa", b: "bb" });
```

#### Array

`number[]` 방식을 더 많이 사용. `Array<number>` 방식은 JSX, TSX에서 충돌이 발생할 가능성 있음.

```typescript
let list1: number[] = [1, 2, 3];
let list2: Array<number> = [1, 2, 3];
```

여러 타입이 섞인 배열은 함께 표현

```typescript
let list3: (number | string)[] = [1, 2, 3, "a"];
```

#### Tuple

배열 내 데이터 타입의 순서가 정해져있는 경우는 Array 타입 만으로 표현 부족. 그래서 Tuple이 나옴.

```typescript
let x: [string, number];
x = ["a", 12];
// x = [10, "jake"]; // 에러

x[1] = 10;
// x[2] = 10;  // 에러

const person: [string, number] = ["jake", 33];
// Destructuring
const [first, second, third] = person; // 순서에 맞게 타입이 추론됨. 튜플에 세 번째 값이 없어서 third는 에러
```

#### any

어떤 타입이어도 상관없는 타입.<br/>
컴파일 타임에 타입 체크가 제대로 이뤄지지 않으므로, **any를 최대한 사용하지 않는 것이 핵심**.<br/>
`noImplicitAny`라는 옵션을 사용하면, 타입을 명시하지 않은 경우 오류 발생

```typescript
function returnAny(message /* : any */): any {
  console.log(message);
}

const any1 = returnAny("리턴은 아무거나");
any1.toString(); // 에러 발생
```

any는 계속 전파됨. 한 곳에서 사용되면 다른 곳에 퍼지게 되어있음.<br/>
(강사님은 존재하지 않는 속성에 대한 참조가 가능해지는 문제에 대해 설명. 전파에 대해 이해 못하신 듯.)

```typescript
let looselyTyped: any = { a: 1 };
const d = looselyTyped.a; // a라는 속성은 number인데도 d의 타입은 any가 됨.

function leakingAny(obj: any) {
  // const a = obj.num;
  const a: number = obj.num;
  const b = a + 1;
  return b;
}

const c = leakingAny({ num: 0 });
c.indexOf("0");
```

#### unknown

컴파일 타임에 타입을 알 수 없는 변수도 있음. (유저로부터 입력되는 동적인 값이거나 의도적으로 모두 수용하고 싶은 경우)

```typescript
declare const maybe: unknown;

const aNumber: number = maybe;

if (maybe === true) {
  // Type Guard
  const aBoolean: boolean = maybe;
  // const aString: string = maybe; // 에러
}

if (typeof maybe === "string") {
  // typeof Type Guard
  const aString: string = maybe;
  // const aBoolean: boolean = maybe; // 에러
}
```

Typescript 3.0 부터 지원<br/>
any와 짝. any보다 Type-safe한 타입<br/>
any처럼 아무거나 할당 가능. **그러나 아무데나 할당은 불가능**. <br/>
타입을 한정(예: 타입 가드)시켜야 다른 곳에 할당 가능.

any보다 unknown을 사용하면 런타임 에러를 줄일 수 있음.

#### never

모든 타입의 서브타입. 모든 타입에 할당 가능.<br/>
하지만 never에는 어떤 값(any 포함)도 할당 불가능.<br/>
잘못된 타입을 넣는 실수를 막고자 할 때 사용하기도 함.

```typescript
function error(message: string): never {
  throw new Error(message);
}

function fail() {
  return error("failed");
}

function infiniteLoop(): never {
  while (true) {}
}

declare const b: string | number;

if (typeof b !== "string") {
  // union type과 타입 가드를 통해 타입을 number로 한정지음.
  b;
}
```

조건에 따라 타입을 다르게 가져갈 수 있음

```typescript
type Indexable<T> = T extends string ? T & { [index: string]: any } : never;

type ObjectIndexable = Indexable<{}>;
type StringIndexable = Indexable<string>;
```

#### void

값은 없고 타입만 있음. 값을 반환하지 않는 함수의 리턴 타입으로 사용됨.

```typescript
function returnVoid(message: string): void {
  console.log(message);
  // return 1; // 리턴 타입이 void인데 다른 타입을 반환하면 에러
  return;
}

const r = returnVoid("without return"); // r은 void 타입. void 타입은 아무것도 못함.
```
