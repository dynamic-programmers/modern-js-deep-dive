# 16장 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 pseudo property와 pseudo method다.

자바스크립트 엔진의 내부 로직이므로 직접 접근 가능한 공개된 객체의 프로퍼티가 아니다.

일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근이 가능하다.

```jsx
const o = {}

// 내부 슬롯은 직접 접근 불가능
o.[[Prototype]] // Uncaught SyntaxError
// 일부에 한하여 간접적으로 접근
o.__proto__ // Object.prototype
```

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.

## 데이터 프로퍼티와 접근자 프로퍼티

프로퍼티는 **데이터 프로퍼티**와 **접근자 프로퍼티**로 구분된다.

### 데이터 프로퍼티

키와 값으로 구성된 일반적인 프로퍼티다.

### 접근자 프로퍼티

자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티다.

## 프로퍼티 정의

프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

## 객체 변경 방지

객체는 변경 가능한 값이므로 재할당 없이 직접 변경 가능하다.

|구분|메서드|프로퍼티 추가|프로퍼티 삭제|프로퍼티 값 읽기|프로퍼티 값 쓰기|프로퍼티 어트리뷰트 재정의|
|---|---|---|---|---|---|---|
|객체 확장 금지	|Object.prevetnExtensions	|X	|O	|O	|O	|O
|객체 밀봉	|Object.seal	|X	|X	|O	|O	|X
|객체 동결	|Object.freeze	|X	|X	|O	|X	|X
---

# 17장 생성자 함수에 의한 객체 생성

**생성자 함수**란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다.

## 생성자 함수

생성자 함수에 의한 객체 생성 방식은 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체를 여러개 간편하게 생성 할 수 있다.

```jsx
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius
	}
}

const circle1 = new Circle(5)
const circle2 = new Circle(10)
```

this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. 

**this가 가리키는 값(`this 바인딩`)**은 함수 호출 방식에 따라 동적으로 결정된다.

**함수를 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.**

### 생성자 함수의 인스턴스 생성 과정

```jsx
// 생성자 함수
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
	console.log(this) // Circle {}

	// 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius
	}

	// 3. 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.
}

//
const circle = new Circle(5)
```

1. 인스턴스 생성과 this 바인딩
    - 암묵적으로 빈 객체가 생성되고 this에 바인딩 된다.
    - 생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 가리키는 이유다.
    - 런타임 이전에 실행된다.
2. 인스턴스 초기화
    - 생성자 함수에 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화 한다.
    - 개발자가 직접 필요한 프로퍼티나 메서드를 추가한다.
3. 인스턴스 반환
    - 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this로 암묵적으로 반환된다.

### new.target

생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 ES6에서 new.target을 지원한다.

- new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수의 자신을 가리킨다.
- new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.

```jsx
// 생성자 함수
function Circle(radius) {
	// new 연산자와 함게 호출되지 않으면 new.target은 undefined다.
	if(!new.target) {
		// new연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
		return new Circle(radius);
	}
	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius
	}
}

// new 연산자 없이 호출해도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5)
```  
----

# 18장 함수와 일급 객체

## 일급 객체

다음 조건을 만족하는 객체를 `**일급 객체**`라 한다.

1. 무명의 리터럴로 생성할 수 있다. 즉 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

## 함수 객체의 프로퍼티

### argumetns 프로퍼티

함수 객체의 arguments 프로퍼티 값은 arguments 객체다.

- arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용 된다.
- 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않으므로 함수 호출 시 매개변수 개수만큼 인수를 전달하지 않아도 에러가 발생하지 않는다.
    
---
# 19장 프로토타입

자바스크립트는 **프로토타입** 기반의 객체지향 프로그래밍 언어다.

## 객체지향 프로그래밍

객체의 집합으로 프로그램을 표현하는 프로그래밍 패러다임이다.

실세계의 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작한다.

객체의 상태를 나타내는 `데이터`와 상태 데이터를 조작할 수 있는 `동작`을 하나의 논리적인 단위로 묶어 생각한다.

## 상속과 프로토타입

상속은 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용하는 것을 말한다.

자바스크립트는 상속을 통한 구현으로 코드를 재사용하여 불필요한 중복을 제거한다.

```jsx
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getArea = function() {
		return Math.PI * this.radius ** 2
	}
}

// circle1과 circle2의 getArea 메서드는 같은 동작을 하지만, 각각의 생성자 함수에 새롭게 생성된다.
const circle1 = new Circle(5)
const circle2 = new Circle(10)

// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
// Circle 인스턴스를 반복적으로 생성하더라도 모든 인스턴스는 하나의 getArea를 공유한다.
Circle.prototype.getArea = function() {
	return Math.PI * this.radius ** 2
}
```

## 프로토타입 객체

프로토타입 객체는 객체 간 상속을 구현하기 위해 사용된다.

어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티를 제공한다.

모든 객체는 `[[Prototype]]`이라는 내부 슬롯을 가지며, 내부 슬롯의 값은 프로토타입의 참조다.

### __proto__ 접근자 프로퍼티

**모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, `[[Prototype]]` 내부 슬롯에 간접적으로 접근할 수 있다.**

### 함수 객체의 prototype 프로퍼티

**함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**

```jsx
// 함수 객체는 prototype 객체를 소유한다.
(function () {}).hasOwnProperty('prototype') // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype') // false
```

## 프로토타입의 생성 시점

프로토타입은 생성자 함수가 생성되는 시점에 생성된다.

내부 메서드 [[Construct]]를 갖는 함수 객체, 일반 함수로 정의한 함수 객체는 new연산자와 함께 생성자 함수로서 호출할 수 있다.

non-constructor는 프로토타입이 생성되지 않는다.

## 객체 생성 방식과 프로토타입의 결정

객체 생성 방식은 다양한데 세부적인 생성 방식의 차이는 있지만, 추상 연산 `**OrdinaryObjectCreate**`에 의해 생성되는 공통점이 있다.

## 오버라이딩과 프로퍼티 섀도잉

프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 인스턴스 프로퍼티로 추가되며, 오버라이딩 되어 프로토타입 프로퍼티는 섀도잉된다.

```jsx
const Person = (function() {
	function Person(name){
		this.name=name
	}
	Person.prototype.sayHello = function(){
		console.log('Hi')
	}
	// 생성자 함수 반환
	return Person
}())

const me = new Person()
me.sayHello = function() {
	console.log('Hey')
}

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello() // Hey
```

## 프로토타입의 교체

### 생성자 함수에 의한 프로토타입 교체

프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없다.

```jsx
const Person = (function() {
	function Person(name){
		this.name=name
	}
	// 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
	Person.prototype.sayHello = function(){
		console.log('Hi')
	}

	return Person
}())

const me = new Person()

me.constructor === Person // false
me.constructor === Object // true
```

### 인스턴스에 의한 프로토타입 교체

프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없다.

```jsx
function Person(name){
	this.name=name
}

const me = new Person()
// 프로토타입으로 교체할 객체
const parent = {
	sayHello(){
		console.log('Hi')
	}
}

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent)
me.__proto__ = parent
me.sayHello() // Hi

me.constructor === Person // false
me.constructor === Object // true
```

## instanceof 연산자

**`객체 instanceof 생성자 함수`**

우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true 그렇지 않으면 false를 반환한다.

```jsx
function Person(name){
	this.name=name
}

const me = new Person()

console.log(me instanceof Person) // true
console.log(me instanceof Object) // true
```

## 프로퍼티 존재 확인

`**key in object**`

```jsx
const person = {
	name: 'Han',
	adress: 'Seoul'
}
console.log('name' in person) // true
console.log('address' in person) // true
console.log('age' in person) // false
```

`**hasOwnProperty**`

```jsx
console.log(person.hasOwnProperty('name') // true
console.log(person.hasOwnProperty('age') // false
```

## 프로퍼티 열거

`**for (변수선언문 in 객체) {...}**`

```jsx
const person = {
	name: 'Han',
	adress: 'Seoul'
}
for(const key in person) {
	console.log(key + ':' +person[key]
}
// name : Han
// address : Seoul
```

`**for... in**` 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거한다.