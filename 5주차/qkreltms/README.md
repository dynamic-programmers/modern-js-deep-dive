# 20 strict mode
ES5부터 생김, 언어의 문법을 좀 더 엄격히 적용해 오류 방지.
# 20.2 strict mode의 적용
스크립트의 전역에 또는 함수 몸체의 선두에 `'use strict'`

스크립트 전역에 선언시 스크립트 단위로 적용된다.

함수 단위로 적용하는 것을 피하자. 번거롭고 함수 외부와 컨텍스트가 다르면 혼란스러움

## 20.5 strict mode가 발생시키는 에러
1. 암묵적 전역
```js
function x() {
  x=1
  console.log(x) // error
}
```
2. delete 문
delete 연산자로 변수, 함수, 매개변수를 삭제할 때
3. 매개변수 이름의 중복
4. with 문의 사용
## 20.6 strict mode 적용에 의한 변화
1. 일반 함수의 this
```js
function x() {
  'use strict'
  console.log(this) // undefined
}
```
2. arguments 객체
매개변수에 전달된 인수를 재할당해도 arguments 객체에 반영되지 않는다.
```js
function x(a) {
  'use strict'
  a=2
  console.log(arguments) // { 0:1, length: 1}
}
x(1)
```

# 21 빌트인 객체
# 21.1 자바스크립트 객체의 분류
1. 표준 빌트인 객체
ECMAScript에 정의된 객체이므로 실행 환경(브라우저, Node)와 관련없이 사용할 수 있다.
2. 호스트 객체
실행 환경에 따라 추가로 제공하는 객체
3. 사용자 정의 객체
# 21.3 원시값과 래퍼 객체
문자열은 객체가 아닌데 객체와 같이 프로퍼티를 호출할 수 있다.
```js
const str='str'
// 원시 타입인 문자열이 일시적으로 래퍼 객체인 String 인스턴스로 변환된다.
str.length
str.toUpperCase()
// 다시 원시 값으로 돌아간다.
typeof str // string
```
어떻게? => 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해 주기 때문이다. 

그 순간 문자열은 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.

식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.

다만, null과 nudefined는 래퍼 객체를 생성하지 않는다.
## 21.4.1 빌트인 전역 프로퍼티
1. Infinity
2. NaN
3. undefined
## 21.4.2 빌트인 전역 함수
1. eval
문자열을 인수로 전달받는다. 다만, [보안에 취약하므로 사용 권장하지 않는다.](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/eval#eval%EC%9D%84%20%EC%A0%88%EB%8C%80%20%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80%20%EB%A7%90%20%EA%B2%83!)

eval 함수는 기존의 스코프를 런타임에 동적으로 수행한다. 단, strict mode에서는 eval 함수 자신의 자체적인 스코프를 생성한다.

전달받은 문자열 코드가 let, const 키워드를 사용할 경우 암묵적으로 strict mode가 적용된다.
```js
const x=1
function foo() {
  'use strict'
  eval('var x=2; console.log(x)') // 2
  console.log(x)//1
}
```
2. isFinite
3. isNaN
4. parseFloat
```js
parseFloat('10.00') // 10
// 처음으로 찾는 숫자 값을 반환한다.
parseFloat('34 35 66') // 34
parseFloat('40 years') // 40
parseFloat('years 40') // NaN
parseFloat('  40  ') // 40
```
5. parseInt
주의: 전달받은 인수가 문자열이 아니면 문자열 => 정수로 해석

진법 변환이 가능하다.

변환 불가하다면 NaN을 반환한다.
```js
parseInt(10,2) // 2, 이진법이 아닌 정수로 표현
const x=15
x.toString(2) // '1111'
// 단, 아래 값은 변환되지 않는다. 왜(?)
'15'.toString(2) // '15'

parseInt('0xf') // 15
// 단 2,8 진수 리터럴은 해석하지 못한다.
// ES5에서는 0으로 시작시 8진수로 해석했지만 ES6부터는 2번째 파라메터 제공해야 됨
parInt('10', 8) // 8
```
6. encodeURI, decodeURI
URI를 받아 문자열을 이스케이프 처리한다.

인코딩이란 URI의 문자들을 이스케이프 처리하는 것을 의미한다.

이스케이프 처리: 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋

1. URL: 아스키 문자 셋으로 구성됨, query 이전 URI
2. URI: 인터넷에 있는 자원 위치를 나타냄
3. URN: 쿼리 이후 URI
7. encodeURIComponent, decodeURIComponent
쿼리의 값만 이스케이프 처리한다.
```js
const uriComp='name=테스트&job=prog&teacher'  
decodeURIComponent(encodeURIComponent(uriComp))//테스트&job=prog&teacher, 단, 실제 실행시 name=포함됨 (??)

decodeURI(encodeURI(uriComp)) //name=테스트&job=prog&teacher
```

# 22 this
자신이 속한 객체를 가리키는 식별자를 참조한다.

this는 어떻게 함수가 호출되냐에 따라서 바인딩 값, 즉 식별자와 연결되는 과정이 달라진다.

함수 정의가 된 곳을 기준으로 상위 스코프가 결정되는 렉시컬 스코프의 함수와 다르게 this 바인딩은 함수 호출 시점에 결정된다.
```js
console.log(this) // window

// 인스턴스 생성 전
test() // window
function test() {
  // 단, strict mode의 함수 안의 this는 undefiend가 호출된다.
  console.log(this)
  this.inner=function() {
    console.log(this)
  }
}
var c = new test() // test
c.inner()// test

// 객체 리터럴은 인스턴스 생성 안해도 this가 자기자신 가리킴
const Person = {
  getThis() {
    console.log(this)
  }
}
Person.getThis() // Person
```

## 22.2 함수 호출 방식과 this 바인딩
## 22.2.1 일반 함수 호출
기본적으로 function에서는 전역 객체가 this에 바인딩 된다.

단, strict mode에서는 undefined

콜백 함수 this 바인딩을  메서드의 this와 일치하기 위한 방법
1. (1)
2. (2), 화살표 함수의 this는 상위 스코프의 this를 가리킨다.
3. 메서드 호출, 메서드 내부의 this는 메서드를 호출한 객체에 바인딩 된다. (화살표 함수와 같음)
4. new 사용해 생성자 함수 호출
5. Function.prototype.apply/call/bind 메서드 사용
```js
const obj = {
  foo() {
    // const that=this (1)
    function bar() {
      // console.log(that)) (1)
      console.log(this)
    }
    // (2)
    // const bar = () => {
    //   console.log(this)
    // }
    // (3)
    // bar() {
    //   console.log(this)
    // }
    bar()
  }
}
obj.foo() //window
```
```js
function Person() {}
Person.prototype.getName=function() {return this}
// Person 인스턴스 생성없이 prototype 프로퍼티 사용 불가
new Person().getName() // Person
window.Person().getName() // Person 
```
```js
Function.ptototype.apply(this로 사용될 객체, [인수들])
Function.ptototype.call(this로 사용될 객체, 인수1,인수2,...)
Function.ptototype.bind(this로 사용될 객체)//함수를 바인딩만 하고 호출은 하지 않는다.
function test() { return this}
const thisArg={a:1}
// 이 순간에만 thisArg에 바인딩 된다.
test.apply(thisArg) // thisArg

// 단, arrow function은 바인딩 안된다.
const test2 = () => this
test2.apply(thisArg) // window
```

this 바인딩 정리
```
함수 호출 방식                      this 바인딩
일반 함수 호출                      window
메서드 호출                         메소드를 호출한 객체
생성자 함수 호출                    생성자 함수가(미래에) 생성할 인스턴스
Function.prototype.apply/call/bind  인수로 전달한 객체
객체 안의 화살표 함수               window
객체 안의 프로토타입 화살표함수     window
객체의 정적 프로퍼티 함수           window
객체의 프로토타임 프로퍼티 함수     생성할 인스턴스
일반 함수 안의 화살표 함수          일반 함수
```
# 23 실행 컨텍스트
## 23.1 소스코드 타입
1. 전역 코드
2. 함수 코드: 함수 내부에 존재하는 소스코드, 중첩 함수, 클래스 등의 내부 코드는 포함되지 안흥ㅁ
3. eval 코드
4. 모듈 코드
## 23.2 소스코드의 평가와 실행
```js
var x
x=1
```
1. 위와 같은 코드를 선언하면 JS 엔진은 var x를 먼저 실행하고
2. x=1일 때 x가 먼저 선언된 변수인지 확인한다.
3. 이를 위해 실행 컨텍스트가 관리하는 스코프에서 확인한다. 
4. 등록되어 있다면 값을 할당한다.
![실행컨텍스트1](context1.jpg)
## 23.3 실행 컨텍스트의 역할
코드가 실행되려면 스코프, 식별자, 코드 순서 등의 관리가 필요한데 이것의 환경과 결과를 관리하는 영역이다.

모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.

다음의 코드를 설명한다.
```js
var x=1
const y=2
function foo(a){
  var x= 3
  const y=4
  function bar(b){
    const z=5
    console.log(a+b+x+y+z)
  }
  bar(10)
}
foo(2) //42
```

![실행컨텍스트2](context2.jpg)
1. 전역 객체 생성: window, 빌트인 객체, 함수가 생성된다.
2. 전역 코드 평가: 먼저 전역 코드를 평가한다.
3. 전역 실행 컨텍스트 생성
4. 전역 렉시컬 환경 생성
4.1 전역 변수, 스코프, 빌트인 함수, 객체를 제공한다.
4.2 그림에서 볼수있듯이 전역 `const y`과 같은 선언전 환경 레코드 코드는 영역이 따로 만들어진다.
4.3 전역 this 바인딩
4.4 외부 렉시컬 환경에 대한 참조 결정
5. 전역 코드 실행
만약 중복된 식별자가 있다면 실행 컨텍스트를 검색하며 식별자 결정을 한다.
6. foo 함수 코드 평가
7. foo 함수 실행 컨텍스트 생성
8. foo 함수 렉시컬 환경 생성
8.1 매개변수, 변수, arguments 객체, 등 생성
8.2 this 바인딩
8.3 외부 렉시컬 환경에 대한 참조 결정
9. foo 함수 실행
10. bar 함수 코드 평가
이하 foo함수와 같음

console.log를 설명하자면, 먼저, bar 컨텍스트 확인, foo 확인, 전역 렉시컬 환경으로 이동하여 console 식별자 검색

표현식 a+b+x+y+z의 평가를 설명하자면, a,x,y는 foo에서, b,z는 bar에서 검색한다.

11. bar 함수 코드 실행 종료
12. foo 함수 코드 실행 종료
13. 전역 코드 실행 종료
주기적으로 GC가 돌 때 누군가에 의해 참조되지 않은 컨택스트는 소멸한다.

## 23.7 실행 컨텍스트와 블록 레벨 스코프
let, const, if, for, while, try, catch 등 지역 스코프는 블록 레벨 스코프를 따른다.

![블록실행컨텍스트](block.jpg)
