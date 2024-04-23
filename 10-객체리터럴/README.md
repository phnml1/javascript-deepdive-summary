
# 객체 리터럴

  

## 객체란?

> 자바스크립트는 `객체`기반의 프로그래밍 언어이며 자바스크립트를 구성하는 거의 `모든 것`이 객체이다.

- 원시타입의 값, 원시 값은 변경 불가능 한 값이지만, 객체 타입의 값, 즉 객체는 변경가능한 값이다.

- 객체는 0개 이상의 `프로퍼티`로 구성된 집합, 프로퍼티는 `키(key): 값(value)`로 구성(**함수도 프로퍼티 값으로 사용 가능!**)
- - 프로퍼티: 객체의 상태를 나타내는 값(data)
	
  - 메서드: 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작

객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 `객체지향 프로그래밍` 이라고 함.

## 객체리터럴에 의한 객체 생성
```
 인스턴스: 클래스에 의해 생성되어 메모리에 저장된 실체, 객체는 클래스와 인스턴스를 포함한 개념, 클래스는 인스턴스를 생성하기 위한 템플릿의 역할을 한다.
 ```
자바스크립트는 **프로토타입 객체지향 언어** 로서, 클래스 기반 객체지향 언어와는 달리  다양한 객체 생성방법을 지원한다.

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

객체 리터럴은 **중괄호**(  `{ ... }`  ) 내에 0개 이상의 프로퍼티를 정의한다. **변수에 할당되는 시점**에 자바스크립트 엔진은 **객체 리터럴을 해석**해 객체를 생성한다.
```js
var person = {
  name: "Wi",
  sayHello: function () {
    console.log(`Hello My name is ${this.name}`);
  },
};

console.log(typeof person); // object
console.log(person); // { name: 'Wi', sayHello: [Function: sayHello] }
```
중괄호 내에 프로퍼티 정의하지 않으면 **빈 객체** 생성

## 프로퍼티
**객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.**
```js
var person = {
  name: "LEE", // 프로퍼티 키는 name, 프로퍼티 값은 "LEE"
  age: 26, // 프로퍼티 키는 age, 프로퍼티 값은 26
};
```
-   `프로퍼티 키(key)`  : 빈 문자열(  `''`  ) 을 포함하는 모든  `문자열(string)`  또는  `심벌(symbol) 값`
-   `프로퍼티 값(value)`  : 자바스크립트에서 사용할 수 있는 모든 값
<br/>

**식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야 한다** (규칙 준수하면 사용안해도 ㄱㅊ)
```js
var person = {
	firstName: 'Joo-young',  // 식별자 네이밍 규칙을 준수한 프로퍼티 키
	'last-name': 'Lee',       // 식별자 네이밍 규칙을 준수하지 않은 프로퍼티 키 ( 따옴표를 사용해 문자열 형태 유지 )
  last-name: 'Lee'          // SyntaxError: Unexpected token ( 식별자 네이밍 규칙을 준수하지 않은 프로퍼티 키
};
console.log(person) // {firstName: 'Joo-young', last-name: 'Lee'}
```
<br/>

프로퍼티에 문자열이나 심벌 값 외의 값을 사용하면  **암묵적 타입 변환** 을 통해 문자열이 된다.
```js
var foo = {
  0: 1,
  1: 2,
  2: 3,
};

console.log(foo); // { 0: 1, 1: 2, 2: 3 } <- 프로퍼티 키로 숫자 리터럴을 사용하면 따옴표는 붙지 않지만 내부적으로는 문자열로 변환된다. 
```
<br/>
이미 존재하는 프로퍼티 중복 선언시 나중에 선언한 프로퍼티가 덮어씌워진다. (에러가 발생하지 않는 것 주의하자)
<br/>

```js
var foo = {
name: 'Lee',
name: 'Kim',
}
console.log(foo); // {name: 'Kim'}
```

## 메서드
자바스크립트에서 사용할 수 있는 모든 값은 **프로퍼티 값**으로 사용 가능, 자바스크립트의 함수는 객체(일급 객체) 이다. 따라서 함수는 값으로 취급할 수 있기 때문에 **프로퍼티 값으로 사용할 수 있다.** 

프로퍼티 값이 함수인 경우, 일반 함수와 구분하기 위해, `메서드`라고 부른다. 즉 메서드는 객체에 묶여있는 함수를 의미한다.

```js
var circle= {
  // 프로퍼티
  radius: 5,

  // 메서드
  getDiameter: function () {
    return 2 * this.radius;
  },
};

console.log(person.getDiameter()); // 10
```
## 프로퍼티 접근
- 마침표 프로퍼티 접근 연산자(.)로 사용하는 **마침표 표기법**
- 대괄호 프로퍼티 접근 연산자([...]) 사용하는 **대괄호 표기법**

	```js
	var person = {
		name: 'Lee'
	};

	// 마침표 표기법에 의한 프로퍼티 접근
	console.log(person.name); // Lee

	// 대괄호 표기법에 의한 프로퍼티 접근
	console.log(persone['name']); // Lee
	```
- 대괄호 표기법을 사용하는 경우 **접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열**이어야 한다.
<br/>
- 대괄호 프로퍼티 접근 연산자 내에 문자열 형태가 아닌 프로퍼티 키로 사용하면 자바스크립트 엔진은 **식별자로 해석**한다.
-  객체에 존재하지 않는 프로퍼티에 접근하면 **undefined**를 반환한다.

	```js
	// 문자열 형태가 아닌 프로퍼티 키로 사용하면 자바스크립트 엔진은 식별자로 해석함
	console.log(person[name]); // ReferenceError: name is not defined
	// 객체에 존재하지 않는 프로퍼티에 접근하면 undefined반환
	console.log(person.age); // undefined
	```

## 프로퍼티 동적 생성 & 삭제
```js
var person = {
  name: "Lee",
};
// person 객체에 age는 존재x
// 따라서 person객체에 age프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20; // { age: 20 } 
console.log(person); // { name: 'Lee', age: 20 }

delete person.age; // age 라는 프로퍼티 키가 존재하므로 해당 프로퍼티가 삭제된다.
delete person.address; // address이라는 프로퍼티 키는 없으므로 삭제 할 수 없지만, 에러는 발생하지 않는다.

console.log(person); // { name: 'Lee' }
```
## ES6에서 추가된 객체 리터럴의 확장 기능


### 프로퍼티 축약 표현
```js
// ES5
var x = 1, y=2;

var obj = {
	x:x,
	y:y
};
console.log(obj); // {x:1, y:2}

// ES6(프로퍼티 축약 표현)
let x = 1, y = 2;
const obj = {x,y};
console.log(obj);
```
### 계산된 프로퍼티 이름
`ES5`
```js
var prefix = "prop";
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
// '객체 외부'에서만 가능
obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;

console.log(obj); // { prop-1: 1, prop-2: 2, prop-3: 3 }
```
`ES6`
```js
const prefix = "prop";
let i = 0;

// '객체 리터럴 내부'에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
};

console.log(obj); // { prop-1: 1, prop-2: 2, prop-3: 3 }
```

### 메서드 축약 표현
`ES5`
```js
var obj = {
	name: "Lee",
	sayHi: function () {
		console.log('Hi!' + this.name);
	},
};

obj.sayHi(); // Hi! Lee
```
`ES6`
```js
const obj = {
	name: "JY",
	 // 메서드 축약 표현
	sayHi() {
		console.log(`Hi! ${this.name}`);
	},
};

obj.sayHi(); // Hi! JY
```
메서드 축약 표현 으로 정의한 메서드는 프로퍼티에 할당한 함수 와 다르게 동작한다.
(이후 자세히 살펴볼 예정)