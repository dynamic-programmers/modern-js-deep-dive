# 16 프로퍼티 어트리뷰트
## 16.1 내부 슬롯과 내부 메서드
응용 프로그래머가 접근할 수 있는 메서드와 그렇지 않은 것이 있다.

접근 불가한 것 => [[...]]
## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
JS엔진은 프로퍼티 상태를 나타내는 값을 각 프로퍼티마다 정의해 놓는다.
1. value: any = undefined
2. writable: boolean = false
3. enumerable: boolea = false
4. configurable: boolean = false
5. get: funcion = undefined
6. set: funcion = undefined
```js
const person = { name: 'Lee' }
Object.getOwnPropertyDescriptor(person, name) // { value: Lee, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptors(person) // 오브젝트의 모든 프로퍼티 상태 출력
```
## 16.3 데이터 프로퍼티와 접근자 프로퍼티
데이터 프로퍼티: [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]

접근자 프로퍼티: 값을 읽거나 저장할 때 호출되는 접근자 함수([[GET]], [[SET]])
```js
const person = { name: 'test', get getName() { return 'test' }, set setName(name) { this.name=name}}
```
## 16.4 프로퍼티 정의
```js
const person = {}
// 여러개라면 Object.defineProperties() 사용
Object.defineProperty(person, 'name', {
  value: 'test', writable: false, enumerable: false, configurable: false
})
Object.keys(person) // [], enumerable 기본값이 false이므로 나오지 않음
person.name='test2' // strict 모드라면 에러 발생
delete person.name // configurable=false 이므로 삭제 되지 않음, strict 모드라면 에러 발생하지 않고 무시됨
```
## 16.5 객체 변경 방지
```js 
                          프로퍼티 추가, 삭제, 읽기, 쓰기, 재정의
Object.perventExtensions        X         O     O    O      O     프로퍼티 추가만 불가함
Object.seal                     X         X     O    O      X     프로퍼티 읽기 쓰기만 가능
Object.freeze                   X         X     O    X      X     프로퍼티 읽기만 가능

// 주의: 위의 함수는 재귀적으로 수행하지 않는다.

Object.isExtensible(obj) // 추가 가능한지 여부
Object.isSealed(obj) // 읽기와 쓰기만 가능한지 여부
Object.isFrozen(obj) // 읽기만 가능한지 여부
```
# 17 생성자 함수에 의한 객체 생성
## 17.2 생성자 함수
```js
// 객체 리터럴에 사용시 내부 값이 공유됨
const obj = { x: 10, getSomething: () => console.log("test")}
const o1 = obj
const o2 = obj
const o3 = new obj() // Uncaught TypeError: obj is not a constructor

o1.x=20
o2.x // 20으로 변경됨

function obj() {
  this.x = 10,
  this.getSomething = () => { console.log("test")}
  // 암묵적으로 바인딩된 this가 반환된다.
  // 바인딩: 식별자와 값을 연결하는 것
  // return 100, 과 같이 원시 값 반환시 무시되고 암묵적으로 this가 반환된다.
}
const o1=new obj() // new 사용시 this가 window가 아닌 obj를 가리킴
const o2=new obj()
o1.x=20
o2.x // 10, 변경되지 않음
```

함수는 객체가 가지고 있는 속성을 갖고 있지만 추가적으로 내부 메서드 [[Call]]과 [[Construct]]를 갖고있다.

new 키워드를 사용하면 [[Construct]]를 호출한다.
```js
function foo() {}
foo() // [[Call]] 호출
new foo() // [[Construct]] 호출

// [[Call]], [[Construct]] 둘 다 되는 것
const baz={ x: function () {} } // method아님! 일반함수 취급됨, 단 편의상 method라 부름
function x() {}

// [[Call]] 만 되는 것, new 사용시 에러 발생
const foo = () => {} // 에로우 함수
const baz={ x: () => {} } // method
const obj = { x() {}} // method
```
### 17.2.7 new.target
new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 알 수 있다.

단, ES6이후만 가능하므로 instanceof 를 사용하길 권장
```js
function Circle(radius) {
  // ES6 이후
  if (!new.target) {
    return new Circle(radius)
  }
  // IE 지원 가능 버전
  if (!(this instanceof Circle)) {
    return new Circle(radius)
  }
}
```
Object, Function 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작

```js
let obj=new Object()
obj// {}
obj = Object()
obj// {}

let f=new Function('x','return x**x')
f // f annoymouse(x) { return x**x }
f = Function('x', 'return x**x')
f // f annoymouse(x) { return x**x }
```
String, Number, Boolean 생성자 함수는 new 연산자와 함께 호출했을 때 해당 객체를 반환하지만, 
없으면 문자열, 숫자, 불리언 값을 반환한다. 이를 이용해 타입변환 함.

# 18 함수와 일급 객체
일급 객체의 조건
1. 무명의 리터럴로 생성한다. 즉, 런타임에 생성 가능
2. 변수나 자료구조(객체, 배열)에 저장 가능
3. 함수의 매개변수로 사용 가능
4. 함수의 반환값 사용 가능

## 18.2.1 arguments 프로퍼티
함수 객체의 arguments 프로퍼티 값은 객체이다. 

arguments 객체는 유사 배열이다. 즉, length 프로퍼티를 가졌고 for문으로 순회 가능한 객체

ES3부터 표준에서 폐지되었으므로 사용 권장하지 않는다, 따라서 가변 인자 사용시 ...rest를 사용한다.

## 18.2.2 caller 프로퍼티
ECMAScript에 포함되어있지 않은 비표준이다. 함수 자신을 호출한 함수를 가리킨다.
## 18.2.3 length 프로퍼티
함수를 정의할 때 매개변수의 개수를 가리킨다.
```js
function foo() {}
foo.length // 0
```
## 18.2.4 name 프로퍼티
ES5,6에 따라 값이 다르므로 주의

ES5에서는 빈 문자열, 6에서는 함수 객체를 가리키는 식별자를(자신의 이름)을 반환한다.
## 18.2.5 __proto__
모든 객체가 갖는다, prototype과 같다.

protohain이 Cycle로 되는 것을 예외처리 해준다.
```js
const p={}
const c={}
c.__proto__=p
p.__proto__=c // error

```
사용 권장하지 않는다. 왜냐면 __proto__가 없는 객체도 있다. 
```js
const obj=Object.create(null) //Object는 프로토타입의 종점이므로 __proto__가 없다.
obj.__proto__ // undefined
Object.getPrototypeOf(obj) // null
``` 
## 18.2.6 prototype
오직 [[constructor]]만이 소유하는 프로퍼티이다. 이후 설명

# 19 프로토타입
자바스크립트는 클래스 기반 객체지향 프로그래밍 언어보다 효율적이며 더 강력한 객체지향 프로그래밍
능력을 지니고 있는 프로토타입 기반의 객체지향 프로그래밍 언어이다.

필요한 속성만 간추리는 것을 추상화라고 한다.

자바스크립트는 프로토타입을 기반으로 상속을 구현한다.

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가진다. 

prototype의 최상위는 Object.prototype이다.
```js
{}.__proto__ === Object.prototype // true
```

모든 prototype은 생성자 함수와 연결되어 있다.

Object.prototype == Object.constructor
```js
function Circle(radius) {
  this.radius=radius
}
Circle.prototype.getArea=function() { return 'test' }
const circle1 = new Circle(1)
const circle2 = new Circle(1)
// getArea를 호출할 때 객체의 프로퍼티에 함수가 없다면 prototype 체인을 올라가며 찾는다. 없다면 undefined를 반환한다.
circle1.getArea()
circle2.getArea()

// 아래와 같이 상속 관계를 동적으로 변경하는것 권장하지 않음!!
// object로 프로토타입 변경
Circle.prototype = {
  // 아래 주석해제하면 Persone으로 다시 바인딩됨
  // constructor: Person
  sayHello() { console.log ('test2')}
}
// 또는 다음 함수사용 Object.setPrototypeOf(circle1, Circle)
circle1=new Circle()
circle2=new Circle()
circle1.constructor === Object // true
circle2.constructor === Object // true
```
## 19.3.2 함수 객체의 prototype 프로퍼티
```js
const Person = name => {
  this.name=name
}
console.log(Person.hasOwnProperty('prototype')) // false, 생성자 함수가 아니므로 prototype 프로퍼티가 존재하지 않는다.
const obj = {
  foo() {}
}
console.log(obj.foo.hasOwnProperty('prototype')) // false
```
```js
            소유        사용 주체    사용 목적
__proto__   모든 객체   모든 객체    객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용
prototype   constructor 생성자 함수  생성자 함수가 자신이 생성할 인스턴스의 프로토타입을 할당하기 위해 사용
```

*NOTE: __proto__는 [Mozila에서 deprecated 됐다.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto) 대신 Object.getPrototypeOf 사용

**함수는 객체가 가지고 있는 속성을 갖고 있지만** 추가적으로 내부 메서드 [[Call]]과 [[Construct]]를 갖고있다.

```js
function foo(){}
console.log(foo.__proto__ === Object.getPrototypeOf(foo))
console.log(Object.getPrototypeOf(foo).constructor) // function Function() { [native code] }
// 함수는 객체가 갖고있는 __proto__ 프로퍼티또한 갖고 있다.
console.log(foo.__proto__)  // function Function() { [native code] }
console.log(foo.prototype.constructor) // function foo() {}
```

## 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수
constructor는 생성자 함수를 가리킨다.
```js
function Person(name) { this.name=name }
const me=new Person('test')
me.constructor === Person // true, me에는 constructor가 없으므로 프로토타입 체인을 따라가 Person의 constructor 가리킴
```
## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다.
```js
const obj=new Object()
obj.constructor === Object // true
new Function('a','return a').constructor === Function // true
const obj2={}
obj.constructor === Object // true
```
객체가 어떻게 생성됐냐에 따라 동작의 차이가 있다.
```js
// 인수가 전달되지 않았을 때 빈 객체를 생성한다.
let obj = new Object()
obj // {}
// 인수가 전돨되면 인수를 객체로 변환한다.
obj = new Object(123)
obj//Number {123}
obj = new Object('123')
obj// String {'123'}

// new.target(new 로 생성시 this 반환) 이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Object.prototype 순으로 프로토 타입 체인 생성
class Foo extends Object{}
new Foo() // Foo {} 

function foo() {}
foo.constructror === Function // true
```
## 19.5.5 프로토타입의 생성시점
프로토타입은 생성자 함수가 생성되는 시점에 같이 생성된다.

프로토타입 자신도 프로토타입을 같는다. 단, 이경우에는 Object.getPrototypeOf()를 통해 가져올 수 있음

## 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점
전역 객체 window 등이 생성될 때 빌트인 생성자 함수, 프로토타입이 생성된다.
Object, String, etc...

## 19.8 오버라이딩과 프로퍼티 섀도잉
아래와 같은 행동을 오버라이딩, 프로퍼티 섀도잉이라고 한다.

me.sayHello()를 호출했을 때 Person 함수를 덮어쓰는 것이 아니라 

new로 생성한 인스턴스에만 해당, 먼저 me에서 sayHello를 찾고 없을시 프로토타입 체인을 올라가 Person에서 찾는다. 삭제할 때도 마찬가지이다.
```js
const Persion = (function() {
  // 생성자 함수
  function Person(name) { 
    this.name = name;
  }
  Person.prototype.sayHello=function(){
    console.log('HI')
  }
})()
const me = new Person('lee')
me.sayHello=function() {
  console.log('Hey!!')
}
me.sayHello() // 'Hey!!'
```

오버라이딩: 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식

오버로딩: 함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다르게 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식
javascript에서는 오버로딩 지원하지 않지만 arguments객체를 사용하여 구현할 수는 있다.

## 19.10 instanceof 연산자
우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하는지 유무 판별
```js
[객체] instanceof [생성자 함수]
```
```js
function Person(){}
const me = new Person()
me instanceof Person // true
me instanceof Object // true
```
## 19.11 직접 상속
Object.create에 의한 직접 상속

명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다.

new 연산자 없이 객체 생성가능
```js
// param1: 생성할 객체의 프토로토타입으로 지정할 객체
// param2: 생성할 객체가 갖을 프로퍼티
let obj = Object.create(Object.prototype, { x: { value: 1, writable: true, enumerable: true, configurable: true}})
obj.x // 1

let obj2 = Object.create(null)
obj2.__proto__ // undefined
```
## 19.12 정적 프로퍼티/메서드
Object.craete와 같이 인스턴스를 생성하지 않아도 호출할 수 있는 Object에 속해있는 프로퍼티/메서드, 프로토타입 체인 상에 존재함 => 정적 메서드

```js
function Foo() {
  Foo.prototype.x=() => 'x'
}
const foo=new Foo()
// 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 한다. 
foo.x() // 'x'

Foo.x=()=>'y'
// 인스턴스를 생성하지 않고 호출 가능 -> 정적 메서드
Foo.x()//y
```
## 19.13 프로퍼티 존재 확인
1. in 연산자

in 연산자는 확인 대상 객체의 프로퍼티뿐만 아니라 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
```js
let person={}
'toString' in person // true

// ES6에서는 in 과 기능이 같은 Reflect.has를 써도 된다.
Reflect.has(person, 'toString') // true
```
2. Object.prototype.hasOwnProperty 메서드
in과 기능이 같지만 상속받은 프로토타입의 프로퍼티인 경우 false를 반환한다.

```js
person.hasOwnProperty('toString') // false
```
## 19.14 프로퍼티 열거
1. for ... in 문
in과 같이 상속받은 포로토타입의 프로퍼티까지 열거한다.

[[Enumerable]]이 true인 값만 순회한다. Symbol 프로퍼티는 열거하지 않는다.

모던 브라우저에서는 순서를 보장하고 숫자인 키에 대해서는 정렬을 실시한다.
```js
const obg ={ 2:2,3:3 }
for (const key in obj) {
  if (!obj.hasOwnProperty(key)) continue
  console.log(key+':'+obj[key])
}
/*
2: 2
3: 3
*/
```

2. forEach, for...of, for 사용해 상속받은 프로퍼티 열거하지 않음
```js
const arr=[1,2,3]
arr.x=10
for (const i in arr) {
  console.log(arr[i]) //1,2,3,10
}
arr.forEach(v=> console.log(v)) // 1,2,3
```

3. Object.keys, values, entires 사용해 상속받은 프로퍼티 열거하지 않음
```js
const person = {
  name: 'junghoon',
  __proto__: { age: 20 }
}
Object.keys(person) // ['name']
```