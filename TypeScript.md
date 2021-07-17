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
  - any, void, never, unknown
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

원시 타입이 아닌 타입(Non-primitive type)을 나타내고 싶을 때 사용하는 타입.

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

## Type System

- 코드 작성자가 타입을 명시적으로 지정하는 시스템
- 컴파일러가 자동으로 타입을 추론하는 시스템

타입스크립트는 둘 모두 포함. 타입을 명시적으로 지정할 수 있고, 그렇지 않은 경우 컴파일러가 자동으로 타입을 추론.

### 작성자와 사용자의 관점으로 코드 바라보기

타입이란 해당 변수가 할 수 있는 일을 결정.

```typescript
function f(a) {
  // a는 any로 추론됨
  return a * 37;
}
console.log(f(10)); // 380
console.log(f("test")); // NaN
```

위 함수는 number 타입 데이터 계산해 반환. 자바스크립트에서든 타입스크립트에서든, 위 코드는 정확한 사용법을 알기위해 함수의 내부나 문서를 확인해야함. NaN이라는 결과값은 원하지 않는 결과일 수 있음.

Typescript의 `noImplicitAny` 옵션(암시적으로 any 타입이 추론되면 에러를 발생)을 켜면 위 코드에서 a 파라미터 부분에 에러가 발생해 컴파일이 되지 않음. 따라서 a에 타입을 지정해야됨.

```typescript
// noImplicitAny: true
function f(a: number) {
  if (a > 0) {
    return a * 37;
  }
}
console.log(f(10)); // 380
console.log(f(-10) + 5); // NaN
```

양수는 정상 동작을 하지만 음수는 undefined를 반환하고, 여기에 수학적 계산을 하게되며 NaN을 출력함. f 함수는 추론을 통해 결과값의 타입이 number임을 알고 있음. 하지만 기본적으로 undefined도 number에 포함되어있어서 에러는 발생하지 않음.

`strictNullChecks` 옵션(모든 타입에 자동으로 포함되어 있는 `null`과 `undefined` 제거)을 켜면, 함수의 리턴타입이 `number | undefined`로 추론됨. 그러면 뒤에 이어지는 f 함수의 결과값에 + 5를 더하는 연산은 불가능해짐.

```typescript
// noImplicitAny: true, strictNullChecks: true
function f(a: number) {
  if (a > 0) {
    return a * 37;
  }
}
console.log(f(-10) + 5); // error TS2532: Object is possibly 'undefined'.
```

이 때 함수의 리턴 타입을 number로 선언하면, 로직 상 undefined로 반환될 수 있기 때문에 에러 발생

```typescript
// noImplicitAny: true, strictNullChecks: true
function f(a: number): number {
  // error TS2366: Function lacks ending return statement and return type does not include 'undefined'
  if (a > 0) {
    return a * 37;
  }
}
```

`noImplicitReturns`: 함수 내에 리턴을 하지 않는 경로가 있으면 컴파일 에러를 발생.

```typescript
// noImplicitReturns: true
function f(a: number): number {
  if (a > 0) {
    return a * 37;
  }
  // error TS7030: Not all code paths return a value.
}
```

파라미터 타입이 object인 경우 해당 타입을 매번 명시하는 것이 불편쓰. 그래서 이를 따로 뽑아서 선언해놓을 수 있음.

```typescript
interface PersonInterface {
  name: string;
}

type PersinTypeAlias = {
  name: string;
};

function f(a: PersonInterface): string {
  return a.name;
}

f("Jake"); // error TS2345: Argument of type 'string' is not assignable to parameter of type 'PersonInterface'.
```

> 개인적으로 다른 옵션들 다 괜찮은데 `noImplicitReturns`는 안켜도 되지 않을까 생각됨.

### Structural Type System VS Nominal Type System

타입스크립트는 Structural Type System을 따르고 있음.

#### Structural Type System = 구조가 같으면 같은 타입

```typescript
interface IPerson {
  name: string;
}

type PersonType = {
  name: string;
};

let personInterface: IPerson = {} as any;
let personType: PersonType = {} as any;

personInterface = personType;
personType = personInterface;
```

#### Nominal Type System = 구조가 같아도 이름이 다르면 다른 타입 (예: C, Java 등)

```typescript
// 자주 사용되는 방식은 아니지만 알아두면 좋은 예시
type PersonID = string & { readonly brancd: unique symbol };
function PersonID(id: string): PersonID {
  return id as PersonID;
}
function getPersonById(id: PersonID) {}

getPersonById(PersonID("id-aaa"));
getPersonById("id-aaa"); // error TS2345: Argument of type 'string' is not assignable to parameter of type 'PersonID'. Type 'string' is not assignable to type '{ readonly brand: unique symbol; }'.
```

#### [Duck Typing](https://ko.wikipedia.org/wiki/%EB%8D%95_%ED%83%80%EC%9D%B4%ED%95%91)

Python에서 사용하는 타입 시스템. Structural Type System과 비슷하지만 약간 다름.

> 객체가 어떤 타입에 걸맞은 변수와 메소드를 지니면 객체를 해당 타입에 속하는 것으로 간주한다.

Structural Type System은 Duck Typing을 컴파일 타임에 적용한 것으로 이해하면 쉬울 듯.

```typescript
interface IPerson {
  name: string;
  say(): void;
  cry(): string;
}

type PersonType = {
  name: string;
  say(): void;
};

let personInterface: IPerson = {} as any;
let personType: PersonType = {} as any;

personInterface = personType; // cry가 PersonType에 없으므로 할당 불가
personType = personInterface; // name과 say를 모두 만족하므로 할당 가능
```

참고: https://soopdop.github.io/2020/12/09/duck-typing/

#### 추가 실험: 리턴 타입 void 함수와 다른 리턴 타입 함수의 비교

```typescript
interface IPerson {
  name: string;
  say(): void;
}

type PersonType = {
  name: string;
  say(): number;
};

let personInterface: IPerson = {} as any;
let personType: PersonType = {} as any;

personInterface = personType;
// personType = personInterface; // say()에서 반환하는 형식이 달라 에러. 'void'는 'number'에 할당할 수 없다고 나옴.

personInterface.say = function (): void {
  console.log("void");
};
personType.say = function (): number {
  console.log("number");
  return 3;
};

const resultVoid = personInterface.say();
const resultNumber = personType.say();

console.log(resultVoid, resultNumber); // 3, 3
```

리턴 타입 void 함수 타입에는 리턴 타입 number 함수를 할당 가능. 반대는 불가능. 왜지?

https://www.typescriptlang.org/docs/handbook/2/functions.html#return-type-void

> Contextual typing with a return type of void does **not** force functions to **not** return something. Another way to say this is a contextual function type with a void return type (type vf = () => void), when implemented, can return any other value, but it will be ignored.

무언가를 반환하지 말라고 강제하지는 않음.

### 타입 호환성 (Type Compatibility)

https://docs.microsoft.com/ko-kr/dotnet/standard/generics/covariance-and-contravariance

#### 1. 같거나 서브 타입인 경우, 할당이 가능하다. => 공변

#### 2. 함수의 매개변수 타입만 같거나 슈퍼타입인 경우, 할당이 가능하다. => 반병 (`strictFunctionTypes` 옵션을 켠 경우)

```typescript
class Person {}
class Developer extends Person {
  coding() {}
}
class StartupDeveloper extends Developer {
  burning() {}
}

function tellme(f: (d: Developer) => Developer) {}

tellme((d: Developer): Developer => {
  return new Developer();
});

tellme((d: Person): Developer => {
  return new Developer();
});

tellme((d: StartupDeveloper): Developer => {
  // 에러 발생
  return new Developer();
});
```

함수 내에서 콜백 호출한다고 생각했을 때, 콜백의 파라미터가 콜백을 호출하는 곳에서 전달하는 인자보다 서브타입이면 에러가 발생할 수 있음.

### 타입 별칭 (Type Alias)

이미 만들어진 타입에 이름만 붙여주는 것. Interface랑 비슷해 보임.

```typescript
type myString = string;
type myStringAndNumber = string | number;
type myTuple = [string, number];
type myFunction = (a: string) => number;
```

> Alias와 Interface를 어떻게 구분해서 사용?<br/>
> 강사님: 타입의 목적에 따라 구분. 이 타입이 사용되는 역할이 따로 있으면 Interface, 없으면 Alias

## TypeScript Compiler

[TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
[TypeScript Deep Dive 한글 번역](https://radlohead.gitbook.io/typescript-deep-dive/)

### Compilation Context

컴파일 할 때 필요한 타입스크립트 설정 정보의 논리적인 묶음으로 구성된 맥락. 이를 선언할 때 좋은 방법은 tsconfig.json 파일을 이용하는 것.

### tsconfig schema

https://www.typescriptlang.org/tsconfig
https://json.schemastore.org/tsconfig

최상위 프로퍼티

- compileOnSave
- extends
- compileOptions (가장 많이 사용하게될 설정)
- files
- include
- exclude
- references
- ~~typeAcquisition~~
- ~~tsNode~~

### compileOnSave

```json
{
  "compileOnSaveDefinition": {
    "properties": {
      "compileOnSave": {
        "description": "Enable Compile-on-Save for this project.",
        "type": "boolean"
      }
    }
  }
}
```

저장 시 컴파일해주는 옵션. 단, 에디터에서 지원해야함.

- Visual Studio 2015 with TypeScript 1.8.4 이상
- atom-typescript 플러그인

별로 중요한 옵션은 아니랰ㅋㅋ 왜 가르쳐주는거야=\_=

### extends

```json
{
  "extendsDefinition": {
    "properties": {
      "extends": {
        "description": "Path to base configuration file to inherit from. Requires TypeScript version 2.1 or later.",
        "type": "string"
      }
    }
  }
}
```

상속받을 설정 파일을 명시하면 해당 파일을 상속.

https://github.com/tsconfig/bases 에서 각 환경별로 tsconfig.json 파일을 제공함.

### files, include, exclude

```json
{
  "filesDefinition": {
    "properties": {
      "files": {
        "description": "If no 'files' or 'include' property is present in a tsconfig.json, the compiler defaults to including all files in the containing directory and subdirectories except those specified by 'exclude'. When a 'files' property is specified, only those files and those specified by 'include' are included.",
        "type": "array",
        "uniqueItems": true,
        "items": {
          "type": "string"
        }
      }
    }
  },
  "excludeDefinition": {
    "properties": {
      "exclude": {
        "description": "Specifies a list of files to be excluded from compilation. The 'exclude' property only affects the files included via the 'include' property and not the 'files' property. Glob patterns require TypeScript version 2.0 or later.",
        "type": "array",
        "uniqueItems": true,
        "items": {
          "type": "string"
        }
      }
    }
  },
  "includeDefinition": {
    "properties": {
      "include": {
        "description": "Specifies a list of glob patterns that match files to be included in compilation. If no 'files' or 'include' property is present in a tsconfig.json, the compiler defaults to including all files in the containing directory and subdirectories except those specified by 'exclude'. Requires TypeScript version 2.0 or later.",
        "type": "array",
        "uniqueItems": true,
        "items": {
          "type": "string"
        }
      }
    }
  }
}
```

- files, include가 없으면 기본적으로 디렉토리 내 모든 ts파일을 컴파일함.
- exclude, include는 Glob 패턴으로 작성 가능.
- files에 명시된 파일들은 exclude와 겹쳐도 컴파일에 포함. include에 명시된 파일들은 exclude에 명시되면 컴파일에서 제외.
- include 설정 시 \*를 사용하면, .ts / .tsx / .d.ts 확장자만 컴파일에 포함됨. (.js 파일은 안됨)
- exclude 미설정 시 node_modules, bower_components, jspm_packages, \<outDir\>을 기본적으로 컴파일에서 제외

### compileOptions - typeRoots, types

타입을 사용하지 않는 외부 모듈(예: React) 사용 시 타입을 지정해주는 도구가 필요함. 기존에는 타입스크립트에서 자체 제공이 안됐고, 써드파티를 통해 이런 외부 모듈의 타입을 제공해주고 있었음.

2.0 부터 @typs/react 같은 모듈을 통해 제공해주고 있음.

```json
{
  "compileOptions": {
    "typeRoots": {
      "description": "Specify multiple folders that act like `./node_modules/@types`.",
      "type": "array",
      "uniqueItems": true,
      "items": {
        "type": "string"
      },
      "markdownDescription": "Specify multiple folders that act like `./node_modules/@types`.\n\nSee more: https://www.typescriptlang.org/tsconfig#typeRoots"
    },
    "types": {
      "description": "Specify type package names to be included without being referenced in a source file.",
      "type": "array",
      "uniqueItems": true,
      "items": {
        "type": "string"
      },
      "markdownDescription": "Specify type package names to be included without being referenced in a source file.\n\nSee more: https://www.typescriptlang.org/tsconfig#types"
    }
  }
}
```

- 아무 설정 안하면 @types(가 설치된 경우)에서 찾아서 사용
- typeRoots를 사용하면, 배열의 경로들 아래에서만 가져옴
- types를 사용하면, 배열 내 모듈 또는 @types 안의 모듈 이름에서 찾아옴
  - 빈 배열([])을 넣으면 types 설정을 사용하지 않겠다는 의미
- typesRoots와 types를 같이 사용하지 않음.

### compileOptions - target, lib

#### target

- 어떤 런타임에서 실행할 수 있도록 컴파일 해줄지 결정하는 설정
- 기본값은 ES3

```json
{
  "compileOptions": {
    "target": {
      "description": "Set the JavaScript language version for emitted JavaScript and include compatible library declarations.",
      "type": "string",
      "default": "ES3",
      "anyOf": [
        {
          "enum": [
            "ES3",
            "ES5",
            "ES6",
            "ES2015",
            "ES2016",
            "ES2017",
            "ES2018",
            "ES2019",
            "ES2020",
            "ES2021",
            "ESNext"
          ]
        },
        {
          "pattern": "^([Ee][Ss]([356]|(20(1[56789]|2[01]))|[Nn][Ee][Xx][Tt]))$"
        }
      ],
      "markdownDescription": "Set the JavaScript language version for emitted JavaScript and include compatible library declarations.\n\nSee more: https://www.typescriptlang.org/tsconfig#target"
    }
  }
}
```

#### lib

- 실행환경 따라 가져와야하는 기본 타입을 명시하는 설정
- target에 따라 기본적으로 설정되는 것들이 있음
  - if target = 'ES3', default lib = [lib.d.ts]
  - if target = 'ES5', default lib = [dom, es5, scripthost]
  - if target = 'ES6', default lib = [dom, es6, dom.iterable, scripthost]
- lib를 설정하면 명시된 라이브러리만 사용

```json
{
  "compileOptions": {
    "lib": {
      "description": "Specify a set of bundled library declaration files that describe the target runtime environment.",
      "type": "array",
      "uniqueItems": true,
      "items": {
        "type": "string",
        "anyOf": [
          {
            "enum": [
              "ES5",
              "ES6",
              "ES2015",
              "ES2015.Collection",
              "ES2015.Core",
              "ES2015.Generator",
              "ES2015.Iterable",
              "ES2015.Promise",
              "ES2015.Proxy",
              "ES2015.Reflect",
              "ES2015.Symbol.WellKnown",
              "ES2015.Symbol",
              "ES2016",
              "ES2016.Array.Include",
              "ES2017",
              "ES2017.Intl",
              "ES2017.Object",
              "ES2017.SharedMemory",
              "ES2017.String",
              "ES2017.TypedArrays",
              "ES2018",
              "ES2018.AsyncGenerator",
              "ES2018.AsyncIterable",
              "ES2018.Intl",
              "ES2018.Promise",
              "ES2018.Regexp",
              "ES2019",
              "ES2019.Array",
              "ES2019.Object",
              "ES2019.String",
              "ES2019.Symbol",
              "ES2020",
              "ES2020.BigInt",
              "ES2020.Promise",
              "ES2020.String",
              "ES2020.Symbol.WellKnown",
              "ESNext",
              "ESNext.Array",
              "ESNext.AsyncIterable",
              "ESNext.BigInt",
              "ESNext.Intl",
              "ESNext.Promise",
              "ESNext.String",
              "ESNext.Symbol",
              "DOM",
              "DOM.Iterable",
              "ScriptHost",
              "WebWorker",
              "WebWorker.ImportScripts",
              "Webworker.Iterable",
              "ES7",
              "ES2021",
              "ES2020.SharedMemory",
              "ES2020.Intl",
              "ES2021.Promise",
              "ES2021.String",
              "ES2021.WeakRef",
              "ESNext.WeakRef"
            ]
          },
          {
            "pattern": "^[Ee][Ss]5|[Ee][Ss]6|[Ee][Ss]7$"
          },
          {
            "pattern": "^[Ee][Ss]2015(\\.([Cc][Oo][Ll][Ll][Ee][Cc][Tt][Ii][Oo][Nn]|[Cc][Oo][Rr][Ee]|[Gg][Ee][Nn][Ee][Rr][Aa][Tt][Oo][Rr]|[Ii][Tt][Ee][Rr][Aa][Bb][Ll][Ee]|[Pp][Rr][Oo][Mm][Ii][Ss][Ee]|[Pp][Rr][Oo][Xx][Yy]|[Rr][Ee][Ff][Ll][Ee][Cc][Tt]|[Ss][Yy][Mm][Bb][Oo][Ll].[Ww][Ee][Ll][Ll][Kk][Nn][Oo][Ww][Nn]|[Ss][Yy][Mm][Bb][Oo][Ll]))?$"
          },
          {
            "pattern": "^[Ee][Ss]2016(\\.[Aa][Rr][Rr][Aa][Yy].[Ii][Nn][Cc][Ll][Uu][Dd][Ee])?$"
          },
          {
            "pattern": "^[Ee][Ss]2017(\\.([Ii][Nn][Tt][Ll]|[Oo][Bb][Jj][Ee][Cc][Tt]|[Ss][Hh][Aa][Rr][Ee][Dd][Mm][Ee][Mm][Oo][Rr][Yy]|[Ss][Tt][Rr][Ii][Nn][Gg]|[Tt][Yy][Pp][Ee][Dd][Aa][Rr][Rr][Aa][Yy][Ss]))?$"
          },
          {
            "pattern": "^[Ee][Ss]2018(\\.([Aa][Ss][Yy][Nn][Cc][Ii][Tt][Ee][Rr][Aa][Bb][Ll][Ee]|[Ii][Nn][Tt][Ll]|[Pp][Rr][Oo][Mm][Ii][Ss][Ee]|[Rr][Ee][Gg][Ee][Xx][Pp]))?$"
          },
          {
            "pattern": "^[Ee][Ss]2019(\\.([Aa][Rr][Rr][Aa][Yy]|[Oo][Bb][Jj][Ee][Cc][Tt]|[Ss][Tt][Rr][Ii][Nn][Gg]|[Ss][Yy][Mm][Bb][Oo][Ll]))?$"
          },
          {
            "pattern": "^[Ee][Ss]2020(\\.([Bb][Ii][Gg][Ii][Nn][Tt]|[Pp][Rr][Oo][Mm][Ii][Ss][Ee]|[Ss][Tt][Rr][Ii][Nn][Gg]|[Ss][Yy][Mm][Bb][Oo][Ll].[Ww][Ee][Ll][Ll][Kk][Nn][Oo][Ww][Nn]))?$"
          },
          {
            "pattern": "^[Ee][Ss]2021(\\.([Ii][Nn][Tt][Ll]|[Pp][Rr][Oo][Mm][Ii][Ss][Ee]|[Ss][Tt][Rr][Ii][Nn][Gg]|[Ww][Ee][Aa][Kk][Rr][Ee][Ff]))?$"
          },
          {
            "pattern": "^[Ee][Ss][Nn][Ee][Xx][Tt](\\.([Aa][Rr][Rr][Aa][Yy]|[Aa][Ss][Yy][Nn][Cc][Ii][Tt][Ee][Rr][Aa][Bb][Ll][Ee]|[Bb][Ii][Gg][Ii][Nn][Tt]|[Ii][Nn][Tt][Ll]|[Pp][Rr][Oo][Mm][Ii][Ss][Ee]|[Ss][Tt][Rr][Ii][Nn][Gg]|[Ss][Yy][Mm][Bb][Oo][Ll]|[Ww][Ee][Aa][Kk][Rr][Ee][Ff]))?$"
          },
          {
            "pattern": "^[Dd][Oo][Mm](\\.[Ii][Tt][Ee][Rr][Aa][Bb][Ll][Ee])?$"
          },
          {
            "pattern": "^[Ss][Cc][Rr][Ii][Pp][Tt][Hh][Oo][Ss][Tt]$"
          },
          {
            "pattern": "^[Ww][Ee][Bb][Ww][Oo][Rr][Kk][Ee][Rr](\\.[Ii][Mm][Pp][Oo][Rr][Tt][Ss][Cc][Rr][Ii][Pp][Tt][Ss])?$"
          }
        ]
      },
      "markdownDescription": "Specify a set of bundled library declaration files that describe the target runtime environment.\n\nSee more: https://www.typescriptlang.org/tsconfig#lib"
    }
  }
}
```

### compileOptions - outFile, outDir, rootDir

```json
{
  "compileOptions": {
    "outFile": {
      "description": "Specify a file that bundles all outputs into one JavaScript file. If `declaration` is true, also designates a file that bundles all .d.ts output.",
      "type": "string",
      "markdownDescription": "Specify a file that bundles all outputs into one JavaScript file. If `declaration` is true, also designates a file that bundles all .d.ts output.\n\nSee more: https://www.typescriptlang.org/tsconfig#outFile"
    },
    "outDir": {
      "description": "Specify an output folder for all emitted files.",
      "type": "string",
      "markdownDescription": "Specify an output folder for all emitted files.\n\nSee more: https://www.typescriptlang.org/tsconfig#outDir"
    },
    "rootDir": {
      "description": "Specify the root folder within your source files.",
      "type": "string",
      "markdownDescription": "Specify the root folder within your source files.\n\nSee more: https://www.typescriptlang.org/tsconfig#rootDir"
    }
  }
}
```

- outFile: 하나의 파일로 만들어줌. 단, system이나 amd 방식의 모듈만 가능.
- outDir: 설정한 디렉토리에 컴파일된 파일들이 동일한 구조로 생성됨
- rootDir: 컴파일 대상 파일들의 루트 디렉토리 설정. 설정하지 않으면 컴파일 대상이 되는 파일의 가장 바깥 디렉토리를 루트 디렉토리로 인식. tsconfig.json이 있는 프로젝트 최상단 폴더가 항상 루트 디렉토리가 되는 것이 아님.

### compileOptions - strict

항상 strict를 true로 켜는 것이 좋음.

```json
{
  "compileOptions": {
    "strict": {
      "description": "Enable all strict type checking options.",
      "type": "boolean",
      "default": false,
      "markdownDescription": "Enable all strict type checking options.\n\nSee more: https://www.typescriptlang.org/tsconfig#strict"
    }
  }
}
```

#### noImplicitAny

추론으로 any 타입이 지정되는 것을 방지

- suppressImplicitAnyIndexErrors
  - object에 없는 프로퍼티를 추가할 때, 해당 프로퍼티가 any로 추론되서 생기는 오류를 발생시키지 않음

#### noImplicitThis

this에 대해 타입을 설정하지 않을 때 오류 발생. 타입스크립트 전용 문법으로 this에 타입 적용 가능.

```typescript
// 원래 자바스크립트에서 this는 예약어라 에러 발생
function fn(this: { a: number; b: string }, a: number, b: string) {
  this.a = a;
  this.b = b;
  return this;
}
```

단, class에서는 this를 사용해도 noImplicitThis 에러가 발생하지 않음.

#### strictNullChecks

모든 타입이 기본적으로 갖고있는 null과 undefined를 제거하는 옵션. 단, void에는 undefined를, any는 null과 undefined를 가질 수 있음.

#### strictFunctionTypes

Bivariant 매개변수 검사를 비활성화.

함수의 반환 타입은 공변적이어야 하고, 인자 타입은 반 공변적이어야 함.

> 왜지? 콜백을 설정하는 상황에서 생각해보자
>
> 콜백은 인자도, 반환값도 다른 함수에 의해 처리됨.
> 인자의 경우 콜백에 전달하기로 한 것보다 더 많은 것을 가진 타입을 바랄 수 없음. 따라서 반 공변적일 수 밖에 없음.
> 반환값의 경우 콜백에서 반환하기로 약속한 것보다 더 적은 것을 가진 타입을 반환하면 안됨. 그래서 공변적일 수 밖에 없는 것임.
>
> ```typescript
> function fn(cb: argDerived => retBase) {
>   const arg: argDerived;
>   const ret = cb(argDerived); // ret: retBase;
>   // ret로 무언가 연산을 진행
> }
> ```

#### strictPropertyInitialization

undefined가 아닌 클래스 속성들이 초기화됐는지 확인하는 옵션. --strictNullChecks이 설정되어 있어야 사용 가능.

#### strictBindCallApply

bind, call, apply를 사용할 때 더 엄격하게 체크하도록 하는 설정

- bind: this와 인자를 함수와 묶어주는 역할
- call: this와 인자를 함수와 묶어서 호출. 여러 인자를 call의 인자로 나열.
- apply: this와 인자를 함수와 묶어서 호출. 여러 인자를 배열 하나에 넣어서 apply에 배열 하나만 전달.

#### alwaysStrict

엄격 모드로 코드를 분석하고 모든 소스 파일에 "use strict"를 추가해줌.

#### [useUnknownInCatchVariables](https://www.typescriptlang.org/tsconfig#useUnknownInCatchVariables)

catch 구문의 변수 타입을 any에서 unknown으로 변경해주는 설정. 타입스크립트 4.0부터 지원
