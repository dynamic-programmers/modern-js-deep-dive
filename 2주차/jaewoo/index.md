# 8장 제어문

제어문은 조건에 따라 **코드 블록을 실행**하거나 **반복 실행**할 때 사용한다.

## 조건문

조건문은 주어진 조건식이 평가 결과에 따라 코드 블록의 실행을 결정한다.

### if.. else 문

논리적 참 또는 거짓에 따라 실행 코드 블록을 결정한다.

조건식의 평가 결과가 true 일 경우 if 문의 코드 블록이 실행되고 false일 경우 else 문의 코드 블록이 실행된다.

```jsx
if (조건식) {
 // 조건식이 참이면 이 코드 블록이 실행된다.
else {
 // 조건식이 거짓이면 이 코드 블록이 실행된다.
}
```

대부분의 if...else 문은 삼항 연산자로 바꿔 쓸 수 있다.

```jsx
var x = 2;
var result;

if(x % 2) {
 result = '홀수';
} else{
 result = '짝수';
}

// 삼항 연산자
var result = x % / 2 ? '홀수' : '짝수';
```

조건에 따라 단순히 값을 결정하여 변수에 할당하는 경우라면 if...else문 보다 삼항 조건 연산자가 가독성이 좋다. 

### switch 문

switch문은 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮긴다.

일치하는 case문이 없다면 default문이 실행된다.

```jsx
switch(표현식) {
	case 표현식1:
		표현식이 표현식1이 일치하면 실행되는 문
	break;
	case 표현식2:
		표현식이 표현식2이 일치하면 실행되는 문
	break;
	default:
		표현식이 일치하는 case문이 없을때 실행되는 문;
}
```

## 반복문

반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행하고, 그 후 조건식을 다시 평가하여 조건문을 실행한다. **조건식이 거짓일 때까지 반복**한다.

### for 문

for 문은 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행한다.

```jsx
for (변수 선언문 할당문; 조건식; 증감식) {
 조건식이 참인 경우 반복 실행될 문;
}
```

### while 문

while문은 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다.

for문은 반복 횟수가 명확할 때 사용하고 while문은 반복 횟수가 불명확할 때 사용한다.

```jsx
var count = 0;

while (count < 3) {
	console.log(count) // 0 1 2
	count ++;
}
```

조건식의 평가 결과가 **항상 참이면 무한루프**가 된다.

### do...while 문

do...while문은 코드 블록을 먼저 실행하고 조건식을 평가한다.

```jsx
var count = 0;
do {
	console.log(count) // 0 1 2
	count++;
} while (count < 3);
```

## break 문

break문은 코드 블록을 탈출한다.

- 레이블 문, 반복문, switch문의 코드 블록 외에 break문을 사용하면 Syntax에러가 발생한다.
- break문은 반복문을 더 이상 진행하지 않아도 될 때 불필요한 반복을 피할수 있다.

## continue 문

continue문은 반복문의 코드 블록 실행을 **현 지점에서 중단**하고 **반복문의 증감식으로 실행 흐름을 이동**시킨다.

```jsx
var string = 'Hello World.';
var search = 'l';
var count = 0;

for(var i = 0; i< string.length; i++) {
	// 'l'이 아니면 현 지점에서 실행을 중단하고 증감식으로 이동한다.
	if(string[i] !== search) continue;
	count++; // continue문이 실행되면 이 문은 실행되지 않는다.
}
console.log(count); // 3
```
---
# 9장 타입 변환과 단축 평가

## 타입 변환이란?

자바스크립트의 모든 값은 타입이 있다.

개발자가 의도적으로 값의 타입을 변환하는것을 `**명시적 타입 변환**` 또는 `**타입 캐스팅**`이라 한다.

개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입을 변환하는 것을 `**암묵적 타입**` 변환 또는 `**타입 강제 변환**`이라 한다.

## 암묵적 타입 변환

자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터의 타입을 강제 변환하는 경우가 있다.

- 암묵적 타입이 발생하면 원시타입 중 하나로 자동변환 된다.

## 명시적 타입 변환

### 문자열 타입으로 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법

1. String 생성자 함수를 new 연산자 없이 호출하는 방법

```jsx
String(1) // "1"
String(NaN) // "NaN"
String(Infinity) // "Infinity"
String(true) // "true"
String(false) // "false"
```

2. Object.prototype.toString 메서드를 사용하는 방법

```jsx
(1).String() // "1"
(NaN).String() // "NaN"
(Infinity).String() // "Infinity"
(true).String() // "true"
(false).String() // "false"
```

3. 문자열 연결 연산자를 이용하는 방법

```jsx
1 + '' // "1"
NaN + '' // "NaN"
Infinity + '' // "Infinity"
true + '' // "true"
false + '' // "false"
```

### 숫자 타입으로 변환

숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법

1. Number 생성자 함수를 new 연산자 없이 호출하는 방법

```jsx
Number('0') // 0
Number('-1') // -1
Number(true) // 1
Number(false) // 0 
```

2. parseInt, parseFlot 함수를 사용하는 방법

```jsx
parseInt('0') // 0
parseFloat('10.53') // 10.53
```

3. +단항 산술 연산자를 이용하는 방법

```jsx
+'0' // 0
+'-1' // -1
+true // 1
+false // 0
```

4. parseInt, parseFlot 함수를 사용하는 방법

```jsx
'0' * 1  // 0
'-1' * 1; // -1
true * 1 // 1
false * 1 // 0
```

### 불리언 타입으로 변환

1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법

```jsx
Boolean('x') //true
Boolean('') //false
Boolean('false') //true
Boolean(0) //false
Boolean(1) //true
Boolean(NaN) //false
Boolean(Infinity) //true
Boolean(null) //false
Boolean(Infinity) //true
Boolean(undefined) //false
```

2. ! 부정 논리 연산자를 두번 사용하는 방법

```jsx
!!'x' // true
!!'' // false
!!0 // false
!!1 // true
!!NaN // false
!!Infinity // true
!!null // false
!!undefined // false
```

## 단축 평가

### 논리 연산자를 사용한 단축 평가

**논리합(||)** 또는 **논리곱(&&)** 연산자 표현식은 2개의 피연산자 중 어느 한쪽으로 평가된다.

논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다.

- true || anything → true
- false || anything → anything
- true && anything → anything
- false && anything → true

### 옵셔널 체이닝 연산자

ES11에서 도입된 연산자(`**?.**`)는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

```jsx
var elem = null;
var value = elem?.value;
console.log(value) // undefined
```

`**?.**` 연산자는 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티 참조할때 유용하다. 

### null 병합 연산자

ES11에서 도입된 연산자(**`??`)**는 좌항의 피연산자가 null또는 undefined인 경우 우항의피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

- 변수에 기본값을 설정할때 유용하다.

```jsx
var foo = null ?? 'default string';
console.log(foo) // "default string"
```

`||` 연산자를 사용한 단축 평가의 경우 좌항의 피연산자가 `falsy값`(false, undefined, null, 0, -0, NaN, '')이면 우항의 피연산자를 반환한다.

`0`이나 `''`이 기본값으로 유효하면 의도치 않은 동작이 발생할 수 있다.

---
# 10장 객체 리터럴

## 객체란?

자바스크립트는 객체 기반의 프로그래밍 언어이면서 구성하는 거의 모든 것이 객체다. 

객체는 0개 이상의 프로퍼티로 구성된 집합이며, `키(Key)`와 `값(Value)`으로 구성된다.

자바스크립트의 함수는 `일급 객체`이므로 값으로 취급 하며, 프로퍼티 값으로 사용 할 수 있다. 

## 객체 리터럴에 의한 생성

리터럴 방식으로 객체를 생성하는 방식은 자바스크립트에서 지원하는 객체 생성 방식 중 가장 일반적이고 간단한 방법이다. 

```jsx
var person = {
	name: 'Lee',
	sayHello: function (){
		console.log(`Hello my name is${name}`);
	}
}

console.log(typeof person) // object
```

## 프로퍼티

**객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.**

프로퍼티 키는 프로퍼티 값에 접근할수 있는 식별자 역할을 한다. 

프로퍼티 키는 식별자 네이밍 규칙을 준수하면 따옴표를 생략할 수 있다. 

```jsx
var person = {
	firstName: 'Jaewoo', // 네이밍 규칙을 준수한 프로퍼티 키
	'last-name': 'Han'   // 네이밍 규칙을 준수하지 않은 프로퍼티 키
}

var obj = {};
var key = 'hello';

// ES5 프로퍼티 키 동적 생성
obj[key] = 'world';
// ES6 
// var obj = {[key]: 'world'};

console.log(obj); // {hello: "world"}
```

## 프로퍼티 접근

프로퍼티 접근 방식은 두 가지가 있다.

- 마침표 프로퍼티 접근 연산자(`.`)를 사용하는 **마침표 표기법**
- 대괄호 프로퍼티 접근 연산자(`[...]`)를 사용하는 **대괄호 표기법**

```jsx
var person = {
	name: 'Jaewoo'
}
// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Jaewoo
// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person["name"]); // Jaewoo
```

- 만약, 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.

## ES6에 추가된 객체 터리털의 확장 기능

### 프로퍼티 축약 표현

프로퍼티 값으로 변수를 사용하는 경우 **변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할** 수 있다.

```jsx
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log( obj ); // {x:1, y:2}
```

### 계산된 프로퍼티 이름

문자열을 이용해 프로퍼티 키를 동적으로 생성 할 수 있다.

```jsx
const prefix = 'prop';
const obj = {
 [`${prefix}-${++i}`] : i,
 [`${prefix}-${++i}`] : i,
 [`${prefix}-${++i}`] : i,
}
console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

### 메서드 축약 표현

메서드를 정의할 때 function 키워드를 생략하여 표현할 수 있다.

```jsx
const obj = {
 name: 'Jaewoo',
 sayHi() {
   console.log(name);
 }
};

obj.sayHi();
```
---
# 11장 원시 값과 객체의 비교

원시타입과 객체 타입은 크게 세 가지 측면에서 다르다.

- **원시 타입의 값은 변경 불가능한 값**이지만, **객체 타입의 값은 변경 가능한 값**이다.
- **원시 값을 변수에 할당하면 변수에는 `실제 값`이 저장**되지만, **객체를 변수에 할당하면 변수에는 `참조 값`이 저장**된다.
- 원시 값을 갖는 변수를 다른 변수에 할당하면 원본의 **원시 값이 복사되어 전달**되고, 이를 **값에 의한 전달(pass by value)**이라 한다. 이에 반해 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 **참조 값이 복사되어 전달** 되고, 이를 **참조에 의한 전달(pass by reference)**이라한다.

## 원시 값

### 변경 불가능한 값

- 원시 타입의 값은 변경 불가능한 값이다.
- 한번 생성된 원시 값은 읽기 전용값으로서 변경할 수 없다.
- 불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없다.

```jsx
// const 키워드를 사용해 선언한 변수는 재할당이 금지된다. 상수는 재할당이 금지된 변수이다.
const o = {};

// const 키워드를 사용해 선언한 변수에 할당한 원시 값은 변경할 수 없다.
// 하지만 const 키워드를 사용해 선언한 변수에 할당한 객체는 변경할 수 있다.
o.a = 1;
console.log(o) // {a:1}
```

### 값에 의한 전달

```jsx
var score = 80
var copy = score

console.log(score, copy) // 80, 80

score = 100

console.log(score, copy) // 100, ?
```

- 위의 copy의 값은 재할당된 score의 값에 영향을 받지 않는다.
- copy 변수에는 score의 값 80이 복사되어 할당 된 것이다.
- score 변수와 copy 변수의 값은 다른 메모리 공간에 저장된 별개의 값이다.

## 객체

### 변경 가능한 값

- 객체 타입의 값은 변경 가능한 값이다.
- 객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 `참조값`에 접근할 수 있다.
- 여러 개의 식별자가 하나의 객체를 공유할 수 있다.

### 참조에 의한 전달

```jsx
var person = {
	name: 'Jaewoo'
}

// 참조 값을 복사
var copy = person
```

- person과 copy는 메모리 주소는 다르지만 동일한 참조 값을 갖는다.
- **두 개의 식별자가 하나의 객체를 공유**한다는 것을 의미한다.