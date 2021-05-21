# 1주차 정리
## 4장 변수
변수 또는 식별자란?

메모리에서 어떤 값이 저장되어있는 주소를 가리키는 상징적인 이름이다.

데이터는 메모리에 2비트로 저장된다.

JS는 개발자의 직접적인 메모리 제어를 허용하지 않는다.

변수는 실행 컨텍스트에 등록된다.

소스코드 실행시 변수 선언 이나 모든 선언문(함수, 클래스 등)은 런타임 이전에 실행된다. 따라서 변수가 어디에서 선언이 되던지 접근이 가능하다. 이것을 전문용어로 호이스팅이라 부름.(단, let,const 등은 제한이 있음) 이 때에 변수는 메모리의 쓰레기 값을 보여주지 않기 위해 undefined로 자동 초기화 된다.

변수 네이밍 규칙: 예약어, 숫자, 특수문자를 제외한 문자, (_), ($)로 시작해야한다.

## 5장 표현식과 문
표현식(expression): 평가되어 값을 생성할 수 있는 문

문(statement): 프로그램을 구성하는 기본 단위이자 최소 실행 단위. 

예: 
```js
var score = 50 + 50 // 문 + 표현식

console.log(score) // 문 + 표현식.

a=b=c=0 // 문 + 표현식

if (true) {} // 제어문(제어 + 문)
```
리터럴(literal): 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법 예: 3

세미콜론은 Optional이나 ASI(automatic semicolon insertion)에 의해 자동 삽입된다.

## 6장 데이터 타입
원시 타입(총 6개)

1. 숫자, 문자열, 불리언, undefined, null, Symbol

객체 타입

1. 객체, 함수, 배열 등

숫자 타입은 64bit를 double(8bytes)형을 지원하도록 설계되어있다. 그러므로 모든 숫자는 실수로 취급된다.[참고](https://262.ecma-international.org/5.1/#sec-8)

숫자 타입은 세 가지의 특별한 값도 표현할 수 있다.
Infinity, -Infinity, NaN

문자열 타입은 UTF-16을 지원하는 0~2 bytes unsigned integer의 연속된 형태이다.

문자열은 C,JAVA와 다르게 원시 타입이며, 변경 불가능한 값이다.

JAVA에서는 String builder를 사용하여 문자열 재활용을 할 수 있는데 JS에서는 원시형이다. 대안으로 array를 통하여 이와 비슷하게 가능하다.

undefined은 JS 엔진이 변수를 초기화하 데 사용하므로 의도적으로 변수에 undefined를 넣는 것은 본래 취지에 어긋난다. 그러므로 null을 넣자

데이터 타입이 필요한이유

1. 값을 저장할 때 확보할 메모리 공간 크기 결정
2. 값을 참조할 때 읽을 메모리 공간 크기 결정
3. 메모리에서 읽어드린 2진수 어떻게 해석할지

자바스크립트 변수는 선언이 아닌 할당에 의해 타입이 결정된다.(타입 추론) 재할당을 통해 언제든지 동적으로 변할수 있다.

## 7장 연산자
연산자란?

하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 지수 연상 등을 수행해 하나의 값을 만든다. 이때 연산의 대상을 피연산자(operand)라 한다.

숫자 타입이 아닌 피연산자에 + 단항 연산자를 사용하면 피연산자를 숫자 타입으로 암묵적으로 변환하여 반환한다.

```js
+'1' = 1

+true = 1

+'hello' = NaN 

+undefined = NaN
```

피연산자 중 하나 이상이 문자열인 경우 문자열 연산자로 피연산자를 문자열 타입으로 암묵적 변환하여 동작한다.
```js
'1'+2 = '12'
// true 1로 암시적 변환된다.
1+true = 2
```

비교연산자

동등 비교 연산자(==)는 암묵적으로 피연산자의 타입을 변환해 비교한다.

반면 일치 비교 연산자(===) 는 타입과 값을 비교한다.

예외
```js
NaN === NaN // false
isNaN(NaN) // true
isNaN(1+undefined) // true

-0 === +0 // true
// ES6, 더 예측가능한 비교를 위해서 Object.is 사용
Object.is(-0,+0) // false
Object.is(NaN,NaN) // true
```

js는 python과 다르게 set에 대해 논리 연산자(||, &&, !)를 쓸 수 없다.
```py
set([1,2,3]) | set([3,4,5]) # {1,2,3,4,5}
set([1,2,3]) & set([3,4,5]) # {3}
set([1,2,3]) - set([3,4,5]) # {1,2}
```

```js
const union = (first, second) => { // first: set, second: set
  const union = [...first]; 
  second.forEach((value) => { if (!union.includes(value)) union.push(value); }) 
  // 없을 때에만 추가 해준다. (합집합 중복 방지)
  return union;
}

const intersect = function(first, second) { // first: set, second: set
  return first.filter(value => second.includes(value)); // 둘 다 있으면 교집합
}

const complement = function(first, second) { // first: set, second: set
  return first.filter(value => !second.includes(value)); // 중복되는 것 제거하면 차집합
}


const setA = new Set([1,2,3])
const setB = new Set([3,4,5]);

union(setA, setB); // Set([1,2,3,4,5])
intersect(setA, setB); // Set([3])
complement(setA, setB); // Set([1,2])
complement(setB, setA); // Set([4,5])
```

js 버그

```js
typeof null // null이 아닌 object가 나온다.
// 따라서 null 타입 확인시 일치 연산자(===)를 사용
foo = null
foo === null // true
```

지수 연산자
```js
2**2 = 4 // 2^2
Math.pow(2,2) = 4// 2^2
// 주의!!
-5 ** 2 = error // 이와 반해 c 계열, python은 -25가 나온다. 왼쪽 피연산자를 양수로 암묵적 변환 후 5^2 계산, 음수를 입혀준다. 아래의 방법으로 해야 잘 나온다.

(-5) ** 2 - 1 = -25 
```
