# Javascript 소수점 처리

## 숫자로 처리

| Method         | Description |
| -------------- | ----------- |
| `Math.floor()` | 버림        |
| `Math.round()` | 반올림      |
| `Math.ceil()`  | 올림        |

주어진 숫자를 각자의 이름에 맞는 방법으로 정수로 만들어 반환. 모두 `Math`의 정적 메서드로 사용자가 생성한 객체의 메서드가 아닌 `Math.XXX`으로 호출해야함.

```javascript
const number = 1.2345;
console.log(Math.floor(number)); // 1
console.log(Math.round(number)); // 1
console.log(Math.ceil(number)); // 2
```

## 문자열로 처리

| Method                             | Description        |
| ---------------------------------- | ------------------ |
| `Number.prototype.toFixed()`       | 고정 소수점 표기법 |
| `Number.prototype.toExponential()` | 지수 표기법        |
| `Number.prototype.toPrecision()`   | 유효 자릿수 지정   |

표준 내장 객체 `Number`의 메서드들. `toString()`처럼 숫자를 문자열로 반환하며, 그 과정에서 메서드 이름에 맞게 소수점 표현을 바꿔서 반환함.

```javascript
const number = 1234.5678;
console.log(number.toFixed(2)); // 1234.56
console.log(number.toFixed(6)); // 1234.567800
console.log(number.toExponential(2)); // 1.23e+3
console.log(number.toExponential(6)); // 1.234568e+3
console.log(number.toPrecision(2)); // 1.2e+3
console.log(number.toPrecision(6)); // 1234.57
console.log(number.toPrecision(9)); // 1234.56780
```

# 참고자료

- [Math.floor() - Javascript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)
- [Math.round() - Javascript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round)
- [Math.ceil() - Javascript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/ceil)
- [Number.prototype.toFixed() - Javascript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed)
- [Number.prototype.toPrecision() - Javascript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/toPrecision)
- [Number.prototype.toExponential() - Javascript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/toExponential)
