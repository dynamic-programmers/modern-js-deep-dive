# 20장 strict mode

## strict mode란?

자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 간으성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시키도록 하는 환경을 제공한다.

```jsx
'use strict'
function foo() {
	x = 10; // ReferenceError: x is not defined
}
foo()
```

## 전역에 strict mode를 적용하는 것은 피하자

전역에 적용된 strict mode는 스크립트 단위로 적용된다.

strict mode와 non-strict mode 스크립트가 혼용되어 있으면 오류가 발생할 경우가 높다.

외부 서드파트 라이브러리를 사용하는 경우 non-strict mode 인경우가 있으므로 다음과 같이 즉시 실행함수를 감싸서 strict mode를적용한다.

```jsx
(function () {
'use strict'
//
}());
```
---
# 21장 빌트인 객체

자바스크립트는 크게 3가지의 객체로 분류된다.

- 표준 빌트인 객체
- 호스트 객체
- 사용자 정의 객체

## 원시값과 래퍼 객체

문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라한다.

```jsx
const str = 'Hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length) // 5
console.log(str.toUpperCase()) // HELLO
```

## 전역 객체

전영 객체(global object)는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않는 최상위 객체다.

### 빌트인 전역 프로퍼티

- Infinity는 무한대를 나타내는 숫자값 `Infinity`를 갖는다.
- NaN은 숫자가 아님을 나타내는 숫자값 `NaN`을 갖는다.
- undefined는 원시 타입 undefined를 값으로 갖는다.

### 빌트인 전역 함수

**isFinite**

- 전달받은 인수가 정상적인 유한수인지 검사하여 유한수이면 true, 무한수이면 false를 반환한다.
- 인수가 숫자 타입이 아니라면, 숫자로 타입을 변환한 후 검사한다.

**isNaN**

- 전달받은 인수가 NaN인지 검사하여 boolean 타입으로 반환한다.
- 인수가 숫자 타입이 아니라면, 숫자로 타입을 변환한 후 검사한다.

**parseFloat**

- 전달받은 문자열 인수를 부동 소수점 숫자, 실수로 해석하여 반환한다.

**parseInt**

- 전달받은 문자열 인수를 정수로 해석하여 반환한다.

**encodeURI / decodeURI**

URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩 한다.

- 이스케이프 처리는 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 ASCII 문자 셋으로 변환하는 것이다.
---
# 22장 this

## this 키워드

자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.

**this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.**

**this가 가리키는 값인 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**

```jsx
// 객체 리터럴
const circle = {
	radius:5,
	getDiameter(){
		// this는 메서드를 호출한 객체를 가리킨다.
		return 2 * this.radius;
	}
}

console.log(circle.getDiameter());

function Circle(radius) {
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
}
const circle = new Circle(5);
```

## 함수 호출방식과 this 바인딩

### 일반 함수 호출

**기본적으로 this에는 전역 객체가 바인딩된다.**

```jsx
var value = 1;
const obj = {
	value: 100,
	foo() {
		// 중첩 함수도 일반함수로 호출되면 this에는 전역객체가 바인딩된다.
		function bar() {
			console.log(this) // window
			console.log(this.value) // 1
		}
		console.log("foo this:", this) // {value: 100, foo:f}
		// 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
		setTimeout(function() {
			console.log(this); // window
			console.log(this.value) // 1
		}, 100)	
	}
}
```

**메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법** 

```jsx
var value = 1;
const obj = {
	value: 100,
	foo() {
		// this 바인딩(obj)을 변수 that에 할당한다.
		const that = this;

		// 콜백 함수 내부에서 this 대신 that을 참조한다.
		setTimeout(function() {
			console.log(that.value); // 100
		}, 100)	
	}
}
```

```jsx
var value = 1;
const obj = {
	value: 100,
	foo() {
		// 콜백 함수에 명시적으로 this를 바인딩한다.
		setTimeout(function() {
			console.log(this.value); // 100
		}.bind(this), 100)	
	}
}
```

### 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체가 바인딩된다.

```jsx
const person = {
	name: 'Jaewoo',
	getName() {
		// 메서드 내부의 this는 메서드를 호출한 객체에 바인딩 된다.
		return this.name
	}
}

const anotherPerson = {
	name: 'Han'
}
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName

// getName 메서드를 호출한 객체는 anotherPerson
console.log(anotherPerson.getName()) // Han

// getName 메서드를 변수에 할당
const getName = person.getName

// getName 메서드를 변수에 할당
console.log(getName()) // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
```

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출

apply, call, bind 메스드는 Function.prototype의 메서드로 모든 함수가 상속받아 사용할 수 있다.

```jsx
function getThisBinding() {
	return this;
}
// this로 사용할 객체
const thisArg={a:1};

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a:1}
console.log(getThisBinding.apply(thisArg)); // {a:1}
```

**apply와 call 메서드의 본질적인 기능은 함수를 호출하는것이다.**

```jsx

```

Function.prototype.bind 메서드는 함수를 호출하지 않고 this로 사용할 객체만 전달한다.

```jsx
function getThisBinding() {
	return this;
}
// this로 사용할 객체
const thisArg={a:1};

// bind 메서드는 함수에 this로 사용할 객체를 전달한다.
// bind 메서드는 함수를 호출하지 않는다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a:1}
```
---
# 23장 실행 컨텍스트

## 소스코드의 타입

ECMAScript 사양은 소스코드를 4가지 타입으로 구분하고 이것들은 실행 컨텍스트를 생성한다.

- 전역코드
    - 전역 변수를 관리하기 위해 최상위 스코프인 전역 스코프를 생성해야 한다.
- 함수코드
    - 지역 스코프를 생성하고 지역변수, 매개변수, arguments 객체를 관리해야 한다.
    - 생성한 지역 스코프를 전역 스코프에서 시작하는 스코프 체인의 일원으로 연결해야 한다.
- eval 코드
    - strict mode에서 자신만의 독자적인 스코프를 생성한다.
- 모듈 코드
    - 모듈별로 독립적인 모듈 스코프를 생성한다.

## 실행 컨텍스트의 역할

1. 전역 코드 평가
    - 선언문만 먼저 실행한다.
    - 생성된 전역변수와 전역 함수가 실행 컨텍스트가 관리하는 전역 스코프에 등록된다.
2. 전역 코드 실행
    - 런타임이 시작되어 전역 코드가 순차적으로 실행된다.
    - 전역 변수에 값이 할당되고 함수가 호출된다.
    - 함수가 호출되면 순차적으로 실행되던 전역 코드의 실행이 중지되고 함수 내부를 실행한다.
3. 함수 코드 평가
    - 내부 함수의 문들을 실행하기 전에 코드 평가 과정을 거친다.
    - 매개변수와 지역 변수 선언문이 먼저 실행되고, 실행 컨텍스트가 관리하는 지역 스코프에 등록된다.
4. 함수 코드 실행
    - 함수 코드가 순차적으로 실행된다.

**실행 컨텍스트는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.**

## 렉시컬 환경

식별자와 식별자에 바인딩된 값, 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트다. 

**렉시컬 환경은 스코프와 식별자를 관리한다.**

## 실행 컨텍스트의 생성과 식별자 검색 과정

```jsx
var x = 1;
const y = 2;

function foo(a) {
	var x = 3;
	const y =4;

	function bar(b) {
		const z = 5;
		console.log(a + b + x+ y+ z);
	}
	bar(10);
}
foo(20) // 42
```

### 1. 전역 객체 생성

전역 객체는 전역 코드가 평가되기 이전에 생성된다.

### 2. 전역 코드 평가

소스코드가 로드되면 자바스크립트 엔진은 전역 코드를 평가한다.

1. 전역 실행 컨텍스트 생성
2. 전역 렉시컬 환경 생성
    1. 전역 환경 레코드 생성
        1. 객체 환경 레코드 생성
        2. 선언적 환경 레코드 생성
    2. this 바인딩
    3. 외부 렉시컬 환경에 대한 참조 결정

### 3. 전역 코드 실행

전역 코드가 순차적으로 실행되기 시작한다.

변수 할당문이 실행되어 전역 변수 x,y에 값이 할당되고 foo함수가 호출된다.

### 4. foo 함수 코드 평가

foo 함수가 호출되면 전역 코드의 실행을 일시 중단하고 foo 함수 내부로 코드의 제어권이 이동한다.

그리고 함수 코드를 평가하기 시작한다.

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
    1. 함수 환경 레코드 생성
    2. this바인딩
    3. 외부 렉시컬 환경에 대한 참조 결정

### 5. foo 함수 코드 실행

런타임이 시작되어 foo 함수의 소스코드가 순차적으로 실행된다.

**식별자 결정을 위해 실행 중인 실행 컨텍스트의 렉시컬 환경에서 식별자를 검색하기 시작한다.**

### 6. bar 함수 코드 평가

bar 함수가 호출되면 bar  함수 내부로 코드의 제어권이 이동한다.

bar 함수 코드를 평가하기 시작한다.

### 7. bar 함수 코드 실행

런타임이 시작되어 bar 함수의 소스코드가 실행된다.

매개변수에 인수가 할당되고, 변수 할당문이 실행되어 지역 변수 z에 값이 할당된다.

### 8. bar 함수 코드 실행 종료

console.log 메서드가 호출되고 bar 함수코드의 실행이 종료된다.

실행 컨텍스트 스택에서 bar 함수 실행 컨텍스트가 팝되어 제거되고 foo 실행 컨텍스트가 실행중인 실행 컨텍스트가 된다.

### 9. foo 함수 코드 실행 종료

bar함수가 종료되면 foo 함수 코드의 실행이 종료된다.

실행 컨텍스트 스택은 foo 함수 실행 컨텍스트가 제거되고 전역 실행 컨텍스트가 실행중인 실행 컨텍스트가 된다.

### 10. 전역 코드 실행 종료

foo함수가 종료되고 전역 실행 컨텍스트도 실행컨텍스트 스택에서 팝되어 아무것도 남아있지 않는다.