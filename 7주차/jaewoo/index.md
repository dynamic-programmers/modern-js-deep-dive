# 28장 Number

## 생성자 함수

표준 빌트인 객체인 Number 객체는 생성자 함수 객체다.

생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 **Number 래퍼 객체**를 생성한다.

new 연산자를 사용하지 않고 Number생성자를 호출하면 숫자를 반환한다.

```jsx
const num = new Number();
console.log(num); // Number {[[PrimitiveValue]]:0}

const num = new Number(10);
console.log(num) // Number {[[PrimitiveValue]]: 10}

Number('0'); // 0
```

## Number 프로퍼티

```jsx
Number.MAX_VALUE // 자바스크립트에서 표현할 수 있는 가장 큰 양수값
Infinity > Number.MAX_VALUE // true

Number.MIN_VALUE // 자바스크립트에서 표현할 수 있는 가장 작은 양수값
Number.MIN_VALUE > 0 // true

Number.MAX_SAFE_INTEGER // 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값
Number.MIN_SAFE_INTEGER // 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값

Number.POSITIVE_INFINITY // 양의 무한대를 나타내는 숫자값 Infinity와 같다.
Number.NEGATIVE_INFINITY // 음의 무한대를 나타내는 숫자값 -Infinity와 같다.

Number.NaN //  숫자가 아님을 나타내는 값
```

## Number 메서드

### Number.isFinite

숫자값이 정상적인 유한수, Infinity 또는 -Infinity가 아닌지 검사한다.

```jsx
Number.isFinite(0) // true
Number.isFinite(Infinity) // false

Number.isFinite(null) // false

// 암묵적으로 인수의 타입을 변환한다.
isFinite(null) // true
```

### Number.isInteger

숫자값이 정수인지 검사한다.

```jsx
Number.isInteger(0) // true
Number.isInteger(123) // true
Number.isInteger(-11) // true

// 타입을 변환하지 않는다.
Number.isInteger(false) // false
Number.isInteger('123') // false
```

### Number.isNaN

숫자 값이 NaN인지 검사한다.

```jsx
Number.isNaN(NaN) // true
Number.isNaN(undefined) // false

// 빌트인 전역함수 isNaN은 전달받은 인수를 암묵적으로 타입변환 한다.
isNaN(undefined) // true
```

### toFixed

반올림하여 문자열로 반환한다.  인수를 생략하면 기본값은 0이다.

```jsx
12345.6789.toFixed() // 12346
12345.6789.toFixed(1) // 12345.7
12345.6789.toFixed(2) // 12345.68
12345.6789.toFixed(3) // 12345.679 
```
----
# 29장 Math

## Math 프로퍼티

```jsx
Math.PI // 원주율 값 반환
// 3.141592653589793
```

## Math 메서드

### Math.abs

절대값을 반환한다.

```jsx
Math.abs(-1)
Math.
```

### Math.round

소수점 이하를 반올림한 정수를 반환한다.

```jsx
Math.round(1.4) // 1
Math.round(-1.5) // -2
```

### Math.ceil

소수점 이하를 올림한 정수를 반환한다.

```jsx
Math.ceil(1.4) //2
Math.ceil(1.6) //2
Math.ceil(-1.4) //-1
```

### Math.floor

소수점 이하를 내림한 정수를 반환한다.

```jsx
Math.floor(1.9) // 1
Math.floor(-1.9) // -2
```

### Math.sqrt

숫자의 제곱근을 반환한다.

```jsx
Math.sqrt(9) //3
Math.sqrt() // NaN
```

### Math.random

임의의 난수를 반환한다. 0에서 1미만의 실수다.

### Math.pow

첫 번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과를 반환한다.

```jsx
Math.pow(2, 8) // 256
Math.pow(2) // NaN
```

### Math.max

인수 중에서 가장 큰 수를 반환한다.

```jsx
Math.max(1, 2) //2
Math.max(1, 2, 3) //3
Math.max() //Infinity

Math.max(...[1, 2, 3]) // 3
```

### Math.min

인수 중에서 가장 작은 수를 반환한다.

```jsx
Max.min(1, 2) //1
Max.min(1, 2, 3) //1
Max.min() //Infinity

Math.min(...[1, 2, 3]) // 1
```
---
# 30장 Date

Date는 생성자 함수다.

현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정된다.

## Date 메서드

```jsx
// 연도를 나타내는 정수를 반환한다.
new Date('2021/06/22').getFullYear() // 2021

// 월을 0~11의 정수를 반환한다.
new Date('2021/06/22').getMonth() // 5

// 날짜를 나타내는 정수를 반환한다.
new Date('2021/06/22').getDate() // 22

// 시간을 나타내는 정수를 반환한다.
new Date('2021/06/22 12:00').getDate() // 12

// 분을 나타내는 정수를 반환한다.
new Date('2021/06/22 12:30').getMinutes() // 30

// 초를 나타내는 정수를 반환한다.
new Date('2021/06/22 12:30:10').getSeconds() // 10

// 밀리초(0~999)를 나타내는 정수를 반환한다.
new Date('2021/06/22 12:30:10:150').getMillisecons() // 150

// 문자열로 날짜를 반환한다.
new Date('2020/7/24/12:30').toDateString() // Fri Jul 24 2020

// 시간을 표현한 문자열을 반환한다.
new Date('2020/7/24/12:30').toTimeString() // 12:30:00 GMT+0900(대한민국 표준시)

// ISO8601 형식으로 날짜와 시간을 표현한 문자열을 반환한다.
new Date('2020/7/24/12:30').toISOString() // 2020-07-24T03:30:00.000Z

// 인수로 전달한 locale을 기준으로 날짜와 시간을 표현한 문자열을 반환한다.
new Date('2020/7/24/12:30').toLocaleString() // 2020. 7. 24. 오후 12:30:00
new Date('2020/7/24/12:30').toLocaleString('en-US') // 7/24/2020, 12:30:00 PM
```
---
# 31장 RegExp

일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식언어다.

## 정규 표현식의 생성

```jsx
const target = 'Is this all there is?'

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

regexp.text(target) // true
```

## RegExp 메서드

### exec

정규표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.

g플래그를 지정해도 첫 번째 매칭 결과만 반환한다.

```jsx
const target = 'Is this all there is?'
const regexp = /is/i;

regexp.exec(target)
// ["is", index:5, input:'Is this all thier is?', group:undefined]
```

### test

정규표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```jsx
const target = 'Is this all there is?'
const regexp = /is/i;

regexp.test(target) // true
```

### match

정규표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```jsx
const target = 'Is this all there is?'
const regexp = /is/i;

regexp.test(target)
// ["is", index:5, input:'Is this all thier is?', group:undefined]
```

g플래그를 지정하면 모든 매칭 결과를 배열로 반환한다.

```jsx
const target = 'Is this all there is?'
const regexp = /is/g;

regexp.test(target) // ['is', 'is']
```

## 플래그

- i: 대소문자를 구별하지 않고 패턴을 검색한다.
- g: 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
- m: 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.

```jsx
const target = 'Is this all there is?'

// is 문자열을 대소문자를 구별하여 한 번만 검색한다.
regexp.match(/is/)
// ["is", index:5, input:'Is this all thier is?', group:undefined]

// is 문자열을 대소문자를 구별하지 않고 한 번만 검색한다.
regexp.match(/is/i)
// ["Is", index:0, input:'Is this all thier is?', group:undefined]

// is 문자열을 대소문자를 구별하여 전역 검색한다.
regexp.match(/is/g)
// ["is", "is"]

// is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
regexp.match(/is/ig)
// ["Is", "is", "is"]
```

## 패턴

### 반복 검색

{m, n}은 최소 m번, 최대 n번 반복되는 문자열을 의미한다. 

```jsx
const target = 'A AA B BB Aa Bb AAA';

// A가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g;
target.match(regexp) // ["A","AA","A","AA","A"]
```

{n}은 {n,n} 과 같다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// A가 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2}/g;
target.match(regexp) // ["AA", "AA"]
```

{n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// A가 최소 2번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A{2, }/g;
target.match(regexp) // ["AA", "AAA"]
```

+는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다.

```jsx
const target = 'A AA B BB Aa Bb AAA';

// A가 최소 한번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A+/g;
target.match(regexp) // ["A", "AA", "A", "AAA"]
```

### or 검색

```jsx
const target = 'A AA B BB Aa Bb';

// A 또는 B를 의미한다.
const regExp = /A|B/g;
target.match(regexp) // ["A","A","A","B","B","B","A","B"]
```

### NOT 검색

[...] 내의 ^은 Not의 의미를 갖는다.

```jsx
const target = 'A AA 11 Aa Bb';

// 숫자를 제외한 문자열을 전역 검색한다.
const regExp = /[^0-9]+/g;
target.match(regexp) // ["A","AA","Aa","Bb"]
```

### 시작 위치로 검색

```jsx
const target = 'https://www.google.com/';

// https로 시작하는지 검사한다.
const regExp = /^https/g;
regExp.test(target)  // true
```

### 마지막 위치로 검색

```jsx
const target = 'https://www.google.com/';

// com으로 끝나는지 검사한다.
const regExp = /com$/g;
regExp.test(target)  // true
```