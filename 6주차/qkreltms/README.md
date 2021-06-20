# 24 클로저
클로저는 자바스크립트 고유의 개념이 아니다. 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 중요한 특성이다.

클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

클로저는 중첩 함숙 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우가 일반적이다. 즉, 클로저여도 외부 식별자 참조하지 않으면 일반적으로 클로저라고 하지 않음

# 24.3 클로저와 렉시컬 환경
```js
const x=1
function outer() {
    const x = 10
    const inner = function() { console.log(x)}
    return inner
}
const innerFunc = outer()
innerFunc()
```
1. x, outer,innerFunc가 window에 등록된다.
2. global, outer 가 실행 컨텍스트에 등록된다.
3. outer가 실행되면 바로 실행 컨텍스트에서 없어진다.
4. innerFunc가 실행 컨텍스트에 등록된다.
5. 실행되고, innerFunc는 outer함수의 렉시컬 환경을 가지고 있기 때문에 10이 출력된다.(클로저)
6. 실행 컨텍스트 스택이 비어진다.
![클로저](클로저.png)

## 24.4 클로저의 활용
1. 상태를 안전하게 변경하고 유지
```js
const increase = (function () {
    let num=0
    return function() {
        return num+=1
    }
}())
console.log(increase())//1
console.log(increase())//2
console.log(increase())//3
```
* NOTE: new 사용시 독립된 렉시컬 환경을 갖는다.
```js
function makeCounter(p) {
    let counter=0
    return function() {
        // 인수인 p에 상태 변경 위임
        counter=p(counter)
        return counter
    }
}
function increase(n) {
    return n+=1
}
const a=makeCounter(increase)
a()//1
a()//2
const b=makeCounter(increase)
b()//1
b()//2
``` 
2. 캡슐화와 정보 은닉

## 24.6 자주 발생하는 실수
```js
// 아래는 3이 3번 출력된다.
var funcs=[]
for (var i=0;i<3;i++) {
    funcs[i]=function(){return i}
}
for (var j=0;j<funcs.length;j++) {
    console.log(funcs[j]())
}
```
문제
1. var i, var 사용으로 인한 전역 변수화
2. function 사용으로 전역 변수 i를 가져오는 점

해결
1. 즉시 실행함수로 인자 넘기기
```js
var funcs=[]
for (var i=0;i<3;i++) {
    funcs[i]=(function(i){return i}(i))
}
for (var j=0;j<funcs.length;j++) {
    console.log(funcs[j])
}
// 0,1,2
```
2. 클로저
```js
var funcs=[]
for (var i=0;i<3;i++) {
    funcs[i]=(function(i){return function() { return i }}(i))
}
for (var j=0;j<funcs.length;j++) {
    console.log(funcs[j]())
}
// 0,1,2
```
3. let i, let 키워드 사용으로 블록 스코프 환경을 만든다.
 ![let](let.jpg)

4. 고차 함수 사용
```js
// Array.from array를 shallow copy
const funcs = Array.from(new Array(3), (_,i)=>()=>i)
funcs.forEach(f=>console.log(f()))
```
# 25 클래스
JS에서 클래스는 함수이며 문법적 설탕이라고 보기보다 새로운 객체 생성 메커니즘으로 보는것이 합당하다.

클래스와 생성자 함수 차이점
1. 클래스는 new 연산자를 사용해 호출하지 않으면 에러, 반면 생성자 함수는 원하는 대로
2. 클래스는 extends, super지원
3. 클래스는 호이스팅이 발생하지 않는 것 처럼 보임
4. 클래스 내의 모든 코드는 암묵적으로 strict mode 지정
5. 클래스의 모든 프로퍼티는 [[Enumerable]]이 false이다, 즉 열거되지 않는다.

## 25.2 클래스 정의
```js
class Person {}

// 익명 클래스 표현식
const Person = class {}
// 기명 클래스 표현식
const Person = class MyClass {}
```
클래스는 일급 객체이다.
1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성 가능하다
2. 변수나 자료구조에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.


## 25.3 클래스 호이스팅
```js
console.log(Person) // ReferenceError: Cannot access 'Person' before initialization
class Person{}
``` 
**Chrome에서 확인해보니 not def 에러나옴 위에 내용 확인 필요.**
## 25.5.1 constructor
생략 가능하다. - 암묵적으로 constructor 정의 됨

중복되면 안된다.

## 25.5.2 프로토타입 메서드
클래스에 메서드 생성시 prototype을 추가하지 않아도 기본적으로 prototype 메서드가 된다.

class의 상위 프로토타입은 Object이다.

인스턴스는 class의 프로토타입 체인에 추가된다.

```js
class Person {
    constructor(name){
        this.name=name
    }
    sayHi(){
        console.log(`Hi! my name is ${this.name}`)
    }
}
const me = new Person('Lee')
me.sayHi() // Hi! my name is Lee

// me.__proto__, me는 Person의 프로토타입 체인에 연결되어있다.
Object.getPrototypeOf(me) === Person.prototype // true
me instanceof Person // true

// 클래스의 상위 프로토타입은 Object이다.
Object.getPrototypeOf(Person.prototype) === Object.prototype // true
me instanceof Object // true

me.constructor === Person // true
```
 ![classPrototype](classPrototype.jpg)
## 25.5.3 정적 메서드
정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.
```js
// 생성자 함수에서 정적 메서드
function Person(name) {
    this.name=name
}
Person.sayHi = function () {
    console.log('Hi!')
}
Person.sayHi() // Hi!
```
```js
class Person {
    constructor(name){
        this.name=name
    }
    static sayHi() {
        console.log('Hi')
    }
}
Person.sayHi() // Hi!
```
 ![static](static.jpg)
정적 메서드는 인스턴스로 호출할 수 없다. 왜냐면 정적 메서드가 바인딩된 클래스에는 인스턴스의 프로토타입 체인상에 존재하지 않기 때문이다. 또한 인스턴스 클래스의 메서드를 상속받을 수 없다.
```js
const me = new Person('Lee')
me.sayHi()// error
```