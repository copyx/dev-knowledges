# ES6 변경사항

## 문법

### `let` & `const`

`var`와 다른 변수 선언 약속어.

`var`와 다르게 변수 선언 전에는 변수를 사용할 수 없음. `var`는 호이스팅될 때 초기값이 없으면 자동으로 `undefined`가 할당됨. `let`은 아무 것도 할당되지 않음. 그래서 메모리 할당이 안되므로 사용하려하면 에러가 발생함. 이 에러가 나는 구간을 TDZ(Temporal Dead Zone)라고 함.

`var`는 함수 레벨 스코프, `let`, `const`는 블럭 레벨 스코프임. `var`는 같은 이름으로 변수 선언이 가능하지만 `let`, `const`는 에러 발생.

### Destructuring Assignment

### Spread Operator

### `new.target`

### `for...of`

#### `.iterator()` & `.next()`

#### `"@@iterator"` Attribute

#### `Symbol.iterator` Attribute

### Default Parameter

### Rest Parameter

### Arrow Function Expression

### Generator Function

#### `yield`

#### `yield\*`

### `arguments[@@iterator]`

## 표준 라이브러리

너무 많아...

## 표준 내장객체

### Function

#### [Function.name](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/name)

Function 객체에서 name 필드를 통해 함수의 이름을 가져올 수 있음. 유명함수는 이름을 잘 가져오지만 익명함수는 빈 문자열을 반환함.

단, ES6 함수를 구현한 브라우저는 익명함 수 이름을 구문상 위치로부터 추측 가능.

읽기 전용 필드지만 [Object.defineProperty()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)를 사용해 변경 가능.

# 참고자료

- [ECMAScript 2015 support in Mozilla](https://developer.mozilla.org/ko/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla)
- [ES6 (ECMAScript 2015) 변경사항 요약정리](https://im-nc2u.tistory.com/entry/ES6-ECMAScript-2015-%EB%AC%B8%EB%B2%95-%ED%8F%AC%EC%9D%B8%ED%8A%B8)
