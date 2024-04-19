# 07장 연산자

**몰랐던 부분이나 주요한 부분 위주로 정리**

## 산술 연산자

- 이항 산술 연산자: 부수 효과 x (어떤 산술연산을 해도 피연산자 값 변경x, 새로운 값만 만듬)  (+,-,*,/,%)

- 단항 산술 연산자
 
| 단항 산술 연산자 | 의미   |  부수 효과    |
|--|--|--|
| ++   | 증가 | O  |
| - - |감소  |O   |
| +   | 어떠한 효과 x 단, 숫자타입이 아닌 거는 숫자타입으로  |x  |
|   -  |양수는 음수로 음수는 양수로 역시 숫자타입으로 변경|x|

### 암묵적 타입 변환
```js
// 문자열 연결연산자
"1" + 2; // '12'
1 + "2"; // '12'

// boolean + number 연산 경우
1 + true; // 2
1 + false; // 1

// number + null 연산 경우 null이 0으로 타입변환
1 + null; // 1

// number + undefined 연산 경우
1 + undefined; // NaN
```
> 위 예제에서 주목할 것은 개발자의 의도와는 상관없이 암묵적으로 타입이 자동 변환되기도 한다는 것이다.
	이를, **암묵적 타입 변환** 또는 **타입 강제 변환** 이라고 한다.

## 비교 연산자
### 동등/일치 비교 연산자
**비교하는 엄격성의 정도가 다르다!**

`동등 비교 연산자`는 좌항과 우항의 피연산자를 비교할 때 먼저 **암묵적 타입 변환을 통해 타입을 일치시킨후 같은 값인지 비교한다.**
(`==`,`!=`)
```js
	5==5; // true
	// 암묵적 타입변환으로 타입을 일치시킴
	5 == '5' //true
	false == '0' //true
```
이처럼 동등 비교 연산자는 예측하기 어렵다. 따라서 대신 일치 비교 연산자를 사용하는게 좋다.

`일치 비교 연산자`는 좌항과 우항의 피연산자가 **타입도 같고 값도 같은 경우에 한하여 true를 반환한다.**

```js
// EX) 일치비교
5 === 5 // true
// 값 & 타입을 비교하기 때문에, 암묵적 타입을 하지 않은 두 값은 같지 않다.
5 === '5' // false 동등이면 true
NaN === NaN // false
```

여기서 주의할 것은 `NaN`이다. `NaN`은 자신과 일치하는 유일한 값이므로, 숫자가 `NaN` 이니 조사하려면 빌트인 함수 `Number.isNaN`을 사용한다.
```js
Number.isNaN(NaN) // true
Number.isNaN(10) // false
Number.isNaN(1 + undefined) // true
```
숫자 0과 같은경우 `+0` `-0` 은 이들을 비교하면 동등/일치 비교연산자 모두 true를 반환한다.
`Object.is`메서드는 이와 관련하여 정확한 비교 결과를 반환한다. 그 외에는 일치 비교 연산자(===)와 동일하게 동작한다.
```js
-0 === +0 // true
Object.is(-0,+0) // false

NaN === NaN // false
Object.is(NaN,NaN) // true
```
## typeof 연산자
**typeof 연산자는 피연산자의 데이터 타입을 `문자열로 반환` 한다.**

총 7가지 문자열 형태로 반환
 -   `string`
 -   `number`
 -   `boolean`
 -   `undefined`
 -   `symbol`
 -   `object`
 -   `function`
    
```js
typeof ""; // "string"
typeof 1; // "number"
typeof NaN; // "number"
typeof true; // "boolean"
typeof undefined; // "undefined"
typeof Symbol(); // "symbol"
typeof null; // "object"
typeof []; // "object"
typeof {}; // "object"
typeof new Date(); // "object"
typeof /test/gi; // "object"
typeof function () {}; // "function"
```
❗ `null` 이 아닌 `object` 반환 주의

값이 null 타입인지 확인할 때는 typeof연산자가 아닌, 일치 연산자(===) 사용
```js
var foo = null;

typeof foo === null // false
foo === null // true 
```
❗ 선언하지 않은 식별자는 RefernceError가 아닌, `undefined` 반환
```js
// undeclared 식별자 선언 x
typeof undeclared; // undefined
```
## 지수연산자
**ES7에서 도입된 지수 연산자는 좌항의 피연산자를 밑으로, 우항의 피연산자를 지수로 거듭 제곱하여 숫자를 반환**
```jsx
2 ** 2; // 4
2 ** 0; // 1
2 ** -2; // 0.25
```

❗ 음수를 거듭제곱할 때는 밑을  `괄호` 로 묶어야 한다.

```jsx
-5 ** 2;  // SyntaxError: Unary operator used immediately before exponentiation expression.
(-5) ** 2;  // 25
```

## 연산자의 부수효과
다른코드에 영향을 주는지 여부로 `할당 연산자(=)`, `증가 감소 연산자(++/--)`, `delete 연산자`가 부수효과가 있는 연산자들 이다.
