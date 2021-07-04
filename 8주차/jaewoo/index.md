# 32장 String

## String 메서드

### indexOf

문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다.

검색에 실패하면 -1을 반환한다.

```jsx
const str = 'Hello world'

str.indexOf('l') // 2
// 인덱스 3부터 'l'을 검색하여 첫번째 인덱스를 반환한다.
str.indexOf('l', 3) // 3
```

### search

인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.

```jsx
const str = 'Hello world'

str.search(/0/) // 4
```

### includes

es6에 도입된 메서드로 인수로 전달받은 문자열이 포함되어 있는지 확인하여 boolean값으로 반환한다.

```jsx
const str = 'Hello world'

str.includes('Hello') // true
// 인덱스 3부터 'l'이 포함되어 있는지 확인
str.includes('l', 3) // true
```

### startsWith

es6에 도입된 메서드로 인수로 전달받은 문자열로 시작하는지 확인하여 boolean값으로 반환한다.

```jsx
const str = 'Hello world'

str.startsWith('He') // true
// 인덱스 5부터 시작하는 문지열이 ' '로 시작하는지 확인
str.startsWith(' ', 5) // true
```

### endsWith

es6에 도입된 메서드로 인수로 전달받은 문자열로 끝나는지 확인하여 boolean값으로 반환한다.

```jsx
const str = 'Hello world'

str.endsWith('ld') // true
// str의 처음부터 5자리까지가 'lo'로 끝나는지 확인
str.endsWith('lo', 5) // true
```

### substring

첫 번째 인수의 인덱스부터 두 번째 인수의 인덱스에 위치하는 문자의 바로 이전 문자까지 부분 문자열을 반환한다.

```jsx
const str = 'Hello World'

str.substring(1, 4) // ell
str.substring(1) // ello World
// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
str.substring(4, 1) // ell
// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-3) // Hello World
```

### slice

substring메서드와 동일하게 동작한다. 

slice 메서드에는 음수인 인수를 전달할 수 있다. 

음수인 인수를 전달하면 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

```jsx
const str = 'hello world'

str.substring(0, 5) // hello
str.slice(0, 5) // hello

str.substring(2) // llo world
str.slice(2) // llo world

str.substring(-5) // hello world
str.slice(-5) // wolrd
```

### replace

첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

```jsx
const str = 'hello world'

str.replace('world', 'Hi') // Hello Hi

const str = 'Hello Hello'
str.replace(/hello/gi, 'Hi') // Hi Hi
```
---
# 33장 데이터 타입 Symbol

## 심벌이란?

ES6에 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.

**심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다.**

```jsx
const mySymbol = Symbol()
console.log(typeof mySymbol) // symbol

// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol('mySymbol')
const mySymbol2 = Symbol('mySymbol')
console.log(mySymbol1 === mySymbol2) // false
```

### Symbol.for / Symbol.keyFor 메서드

Symbol.for 메서드는 인수로 전달받은 문자열을 키로 키와 심벌값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.
- 검색에 실패하면 새로운 심벌 값을 생성하여 전역 심벌 레지스트리에 저장한 후 생성된 심벌 값을 반환한다.

```jsx
const s1 = Symbol.for('mySymbol')
const s2 = Symbol.for('mySymbol')

console.log(s1 === s2) // true
```

Symbol.keyFor 메서드는 전역 레지스트리에 저장된 심벌 값의 키를 추출 할 수 있다.

```jsx
const s1 = Symbol.for('mySymbol')
Symbol.keyFor(s1) // mySymbol

const s2 = Symbol('foo')
Symbol.keyFor(s2) // undefined
```

## 심벌과 프로퍼티

**심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.**

```jsx
const obj = {
	// 심벌 값으로 프로퍼티 키를 생성
	[Symbol.for('mySymbol')]: 1
}

obj[Symbol.for('mySymbol') // 1
```
---
# 34장 이터러블

## 이터레이션 프로토콜

ES6에 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션을 만들기 위해 정의한 규칙이다.

이터러블 프로토콜과 이터레이터 프로토콜이 있다.

### 이터러블

이터러블은 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다.

```jsx
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function'

isIterable([]) // true
isIterable('') // true
isIterable(new Map()) // true
isIterable(new Set()) // true
isIterable({}) // false
```

### 이터레이터

이터러블의 Symbol.iterator 메서드가 반환한 이터레이너틑 next 메서드를 갖는다.

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array  = [1, 2, 3]

// Symbol.iterator 메서드는 이터레이터를 반환한다.
Symbol.iterator = array[Symbol.iterator]()

// next 메서드를 호출하면 이터레이터 result 객체를 반환한다.
console.log(iterator.next()) // {value:1, done:false}
console.log(iterator.next()) // {value:1, done:false}
console.log(iterator.next()) // {value:1, done:false}
console.log(iterator.next()) // {value:undefined, true}
```

## 이터러블과 유사 배열 객체

유사 배열 객체는 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다.

```jsx
const arrayLike = {
	0:1,
	1:2,
	2:3,
	length:3
}
// for문으로 순회가능
for (let i = 0; i < arrayLike.length; i++) {
	 console.log(arrayLike[i]) // 1 2 3 
}
```

유사 배열 객체는 이터러블이 아닌 일반 객체다.

Symbol.iteraotr 메서드가 없기 때문에 for...of 문으로 순회할 수 없다.

arguments, NodeList, HTMLCollection은 유사 배열 객체이면서 이터러블이다.
---
# 35장 스프레드 문법

es6에서 도입된 스프레드 문법은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.

## 함수 호출문의 인수 목록에서 사용하는 경우

```jsx
// rest 파라미터는 인수들의 목록을 배열로 전달받는다.
function foo(...rest) {
	console.log(rest) // 1, 2, 3 -> [1, 2, 3]
}

// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
foo(...[1,2,3])
```

## 배열 리터럴 태부에서 사용하는 경우

### concat

```jsx
// es5
var arr = [1,2].concat([3,4])
console.log(arr) // [1, 2, 3, 4]

// es6
const arr = [...[1, 2], ...[3, 4]
console.log(arr) // [1, 2, 3, 4]
```

### splice

어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거할때 사용한다.

```jsx
// es5
var arr1 = [1, 4]
var arr2 = [2, 3]

// 세번째 인수에 arr2를 해체해서 전달해야 한다.
// 그렇지 않으면 arr1에 arr2 배열 자체가 추가된다.
arr1.splice(1, 0, arr2)
console.log(arr1) // [1, [2, 3], 4]

// 
Array.prototype.slice.apply(arr1, [1, 0].concat(arr2))
console.log(arr1) // [1, 2, 3, 4]

// es6
arr1.splice(1, 0, ...arr2)
console.log(arr1) // [1, 2, 3, 4]
```

### 배열 복사

```jsx
// es5
var origin = [1, 2]
var copy = origin.slice()

console.log(copy) // [1, 2]
console.log(origin === copy) // false

// es6
const copy = [...origin]
console.log(copy) // [1, 2]
console.log(origin === copy) // false
```

### 이터러블을 배열로 변환

```jsx
// es5
function sum() {
	// 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
	var args = Array.prototype.slice.call(arguments)
	return args.reduce(function (pre, cur){
		return pre + cur
	}, 0)
}

console.log(sum(1,2,3)) // 6

// es6
function sum(){
	// 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
	return [...arguments].reduce((pre, cur)=>pre+cur, 0)
}

// rest 파라미터
const sum = (...args) => args.reduce((pre, cur)=> pre+cur, 0)
```