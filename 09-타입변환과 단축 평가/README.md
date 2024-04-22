

# 타입 변환과 단축 평가
개발자가 의도적으로 값의 타입을 변환하는 것: `명시적 타입 변환` or `타입캐스팅`

개발자의 의도와는 상관없이 표현식을 평가하는 도중에 **자바스크립트 엔진에 의해 암묵적으로 타입이 변환**되는 것: `암묵적 타입 변환` or `타입강제 변환`

## 암묵적 타입 변환
> 자바스크립트 엔진이 표현식을 평가할 때 개발자의 의도와는 상관없이 `코드의 문맥을 고려해 암묵적으로` 데이터 타입을 강제 변환(암묵적 타입 변환)할 때가 있다.

암묵적 타입 변환은 문자, 숫자, 불리언 같은 **원시 타입**중 하나로 타입을 자동 변환한다.

### 문자열 타입으로 변환
```js
// 숫자 타입 
0+'' // '0'
NaN + '' // 'NaN'
Infinity + '' // 'Infinity'

// 불리언 타입
true + '' // 'true'

// null 타입
null + '' // 'null'

// undefined 타입
undefined + '';       // "undefined"

// 심벌 타입
(Symbol()) + '';      // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + '';            // "[object Object]"
Math + '';            // "[object Math]"
[] + '';              // ""
[10, 20] + '';        // "10,20"
(function(){}_ + '';  // "function(){}"
Array + '';           // "function Array() { [native code] }"
```

### 숫자 타입으로 변환
```js
// 문자열 타입(+ 단항연산자는 피연산자가 숫자타입이 아니면 숫자 타입으로 암묵적 타입 변환 수행)
+'' // -> 0
+'0' // 0
+'string' // -> NaN

// 불리언 타입
+true // -> 1

// null 타입
+null // -> 0

//undefined 타입
+undefined // -> NaN

// 심벌 타입
+Symbol(); // TypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}; // -> NaN
+[]; // -> 0
+[10, 20]; // NaN
+function () {}; // NaN
```
**빈 문자열 (''), 빈 배열([]), null, false**는 **0**으로, **true**는 **1**로 변환된다. **객체와 빈 배열이 아닌 배열,undefined**는 변환되지 않아 **NaN**이 된다는 것에 주의하자

### 불리언 타입으로 변환
if문, for문과 같은 제어문 또는 삼항 조건 연산자의 조건식은 불리언 값, 즉 논리적 참/거짓으로 평가 되어야 하는 표현식이다.
```js
// 모두 코드 블록 실행됨
if(!false) console.log(false + ' is falsy value')
if(!undefined) console.log(undefined + 'is falsy value')
if(!NaN) console.log(NaN + 'is falsy value');
if(!null) console.log(null + 'is falsy value');
```
자바스크립트 엔진은 불리언 타입이 아닌 값을 `Truthy 값(참으로 평가되는 값)` or `Falsy 값(거짓으로 평가되는 값)` 으로 구분한다.

**자바스크립트 엔진이 Falsy 값으로 판단하는 값**
+ false
+ undefined
+ null
+ 0, -0
+ NaN
+ ''(빈 문자열)

## 명시적 타입 변환
**개발자의 의도에 따라 명시적으로 타입을 변환하는 것** 

**표준 빌트인 함수(String,Number,Boolean)**을  new연산자 없이 호출하는 방법과 **빌트인 메서드를 사용하는 방법**, 그리고 **암묵적 타입 변환**을 이용하는 방법이 있다.

### 문자열 타입으로 변환
```js
// String 생성자 함수를 new 연산 없이 호출
String(1); // -> "1"
String(NaN); // -> "NaN"
String(Infinity); // -> "Infinity"
String(false); // -> "false"

// Object.prototype.toString 메서드를 사용하는 방법
(NaN).toString() // -> 'NaN'
(true).toString() // -> 'true'

// 문자열 연결 연산자
NaN + '' // -> "NaN"
```

### 숫자 타입으로 변환
```js
// Number 생성자 함수를 new 연산자 없이 호출하는 방법
Number('0') // -> 0
Number(10.53) // -> 10.53

// parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
parseInt('0') // -> 0
parseFloat('10.53') // -> 10.53

// + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
+'0'
+'-1'
// 불리언 타입 => 숫자 타입
+true; //->1

// * 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
'0' * 1;
'-1' * 1; //1
// 불리언 타입 => 숫자 타입
true * 1; //->1
```

### 불리언 타입으로 변환
```js
// 1. Boolean 생성자 함수를 new 키워드 없이 호출하는 방법
Boolean('x'); // true

Boolean(0) // false
Boolean(NaN) // false
Boolean(Infinity) // true

Boolean(null) // false
Boolean(undefined) // false

Boolean({}) //True

//2. !(부정 논리 연산자) 두 번 사용
!!'x';  // true ( !(!'x') === !(false) -> true )
```

## 단축 평가
> `단축 평가` : 논리 연산의 결과를 결정하는 피연산자를 `타입 변환하지 않고 그대로 반환`
-  단축 평가는 표현식을 평가하는 도중에 평가결과가 확정 경우 → 나머지 평가 과정을 생략한다.
단축 평가 표현식

| 단축 평가 표현식 | 평가 결과 |
|--|--|
|true ll anything  | true |
|false ll anything | anything |
| true && anything |  anything|
| false && anything | false |

### 논리 연산자를 단축 평가
`논리곱(&&)` 에서 논리 연산의 결과를 결정하는 것은 **두 번째 피연산자**
```js
'Cat' && 'Dog'; // Dog
```

`논리곱(&&) 연산자` 는 두 개의 피연산자가 `모두 true로 평가될 때 true를 반환`, `좌항 -> 우항으로 평가가 진행` 


`논리합(||)` 에서  논리 연산의 결과를 결정하는 것은 **첫 번째 피연산자**  그러므로 첫 번째 연산자 그대로 반환
```js
'Cat' || 'Dog' // Cat
```

`논리합 (||) 연산자` 는 `두 개의 피연자 중 하나만 true로 평가되어도 true로 반환`하며, 역시 `좌항에서 우항으로 평가가 진행`

 ```js
 // 논리합(||) 연산
"Cat" || "Dog"; // "Cat"
false || "Dog"; // "Dog"
"Cat" || false; // "Cat"

// 논리곱(&&) 연산
"Cat" && "Dog"; // "Dog"
false && "Dog"; // "false"
"Cat" && false; // "false"
 ```
 **단축평가 사용 예시**
 - **if 문대체**
논리 연산자 단축평가를 통해서 if문도 대체할 수 있으며, 값 할당시 보다 더 깔끔할 수 있다.
	```js
	var done = false;
	var message = "";

	// 조건문으로 값 할당
	if (!done) message = "미완료";

	meessage = done || "미완료";
	console.log(message) // 미완료
	```
- **객체를 가리키기를 기대하는 변수의 값이 객체가 아니라 null또는 undefined일 경우** 

	이러한 경우 타입 에러가 발생한다. 하지만, 단축평가를 사용하면 에러 발생 x
	```js
	// elem이 null 또는 undefined같은 Falsy값이면 elem으로 평가
	// elem이 Truthy값이면 elem.value 값으로 평가
	var elem = null;
	var value = elem && elem.value // null
	```
- **함수 매개변수에 기본값을 설정**
함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다.  단축평가 사용해 매개변수에 기본값을 설정한다면 에러 방지 가능
	```js
	// 단축 평가를 사용한 매개변수의 기본값 설정
	function getStringLength(str) {
	  str = str || "";
	  return str.length;
	}
	getStringLength(); // 0
	getStringLength('hi'); //2
	//  Es6의 매개변수 default parameter 설정
	function getStringLength(str = "") {
	  return str.length;
	}
	getStringLength(); // 0
	```

## 옵셔널 체이닝 연산자
`?` : 좌항의 피 연산자가 null 또는 undefined인 경우 `undefined 반환`, 그렇지않으면 `우항의 포로퍼티 참조`
```js
var elem = null;
var value = elem?.value; // undefined
```
-   객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 안전하게 참조할 때 유용
-   옵셔널 체이닝 도입 이전에는  `논리곱(&&)을 사용한 단축 평가`를 통해 변수가 null 또는 undefined 인지 확인했음
```js
// 좌항이 Falsy라면 좌항 그대로 반환(0이나 ''반환 할 때도)
var str = ""; //
var length = str && str.length; // ''

// 좌항 피연산자가 Falsy값이라도 null 또는 undefined 만 아니면, 우항의 프로퍼티를 참조한다.
var str = "";
var length = str?.length; // 0
```

## null 병합 연산자 
`??` : 좌항의 피연산자가 **null또는 undefined인 경우** `우항의 피연산자 반환` 그렇지 않으면 `좌항 피연산자 반환`한다.
```js
var foo = null ?? "default string"; // "default string"
```
- 변수에 기본값을 설정할 때 유용하며, 그전에는 논리합(||)을 사용한 단축평가로 변수에 기본값 설정
	```js
	// 좌항의 피연산자가 Falsy값이면, 우항의 피연산자를 반환 
	//(Falsy 값인 0 이나 ''도 기본값으로서 유효하다면 예기치 않은 동작이 발생한다.)
	var foo = "" || "default string"; // "default string"

	//  좌항의 피연산자가 Falsy값이라도 null 또는 undefined 가 아니면, 좌항의 피연산자를 그대로 반환한다.
	var foo = "" ?? "default string"; // ''
	```