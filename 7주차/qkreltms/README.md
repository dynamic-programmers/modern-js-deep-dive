# 28 Number
## 28.1 Number 생성자 함수
new 연산자와 함께 Number를 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.
```js
const numObj = new Number()
undefined
console.log(numObj)
Number {0}__proto__: Number[[PrimitiveValue]]: 0
```
## 28.2 Number 프로퍼티
1. Number.EPSILON
부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.
```js
function isEqual(a,b) {
    return Math.abs(a-b) < Number.EPSILON
}
isEqual(0.1+0.2, 0.3)
```
2. Number.MAX_VALUE
js에서 표현할 수 있는 가장 큰 양수 값 출력(1.7976931348623157e+308)
3. Number.MIN_VALUE
4. Number.MAX_SAFE_INTEGER
js에서 안전하게 표현할 수 있는 가장 큰 정수값 출력(90071992547540991)
5. Number.MIN_SAFE_INTEGET
6. Number.POSITIVE_INFINITY
Infinity와 같다.
7. Number.NEGATIVE_INFINITY
-Infinitiy와 같다.
8. Number.NaN
## 28.3 Number 메서드
1. Number.isNinite
유한수인지 아닌지 판별

전역 함수 isFinite와 치이점: 인수를 숫자로 암묵적으로 변환하지 않음
2. Number.isInteger
정수인지 판별

3. Number.isNaN
NaN인지 판별

전역 함수 isNaN와 차이점: 인수를 암묵적으로 숫자로 변환하지 않음

4. Number.isSafeInteger
ES6부터 추가

5. Number.prototype.toExponential
숫자를 지수 표기법으로 변환하여 문자열로 반환

```js
(77.1234).toExponential(2) // 7.7123e+1
(77.1234).toExponential(2) // 7.71e+1
(77.1234).toExponential() // 7.71234e+1

// 숫자뒤의 '.'을 소수점으로 인식
77.toExponential() // error
77.1234.toExponential() // 7.71234e+1
// 띄어쓰기를하면 인식
77 .toExponential() // 7.7e+1
```
6. Number.prototype.toFixed
7. Number.prototype.toPrecision
8. Number.prototype.toString
# 29 Math
# 29.1 Math 프로퍼티
1. Math.PI
2. Math.abs
```js
Math.abs('')//0
Math.abs([])//0
Math.abs(null)//0
Math.abs(undefined)// NaN
Math.abs({})// NaN
Math.abs('Hello')// NaN
Math.abs()// NaN
```
3. Math.round
인자의 소수점 이하 반올림 정수 반환
4. Math.ceil
인자의 소수점 올림 정수 반환(값을 올린다.)
```js
Math.ceil(1.4) // 2
Math.ceil(1.6) // 2
Math.ceil(-1.4) // -1, -1.4에서 -1이 되는것이 더 커짐
Math.ceil(-1.6) // -1
```
5. Math.floor
인자의 소수점 내림한 정수 반환(값을 줄인다.)
```js
Math.floor(1.9)//1
Math.floor(-1.9)//-2
```
6. Math.sqrt
인자의 제곱근 반환
7. Math.random
```js
Math.random() // 0.1840615524146605
```
8. Math.pow
'**'와 같음
9. Math.max
```js
Math.max(1,2,3)//3
Math.max() // -Infinity

Math.max.apply(null, [1,2,3])//3
Math.max(...[1,2,3])//3
```
10. Math.min
```js
Math.min() // Infinity
```
# 30 Date
UTC, GMT는 초의 소수점 단위에서만 차이가 나기 떄문에 일상에서는 혼용되어 사용한다.

KST(한국 표준시)는 UTC에 9시간을 더한 시간이다.

1970년 1월 1일 0시를 기준으로한다.

## 30.1 Date 생성자 함수
1. new Date()
```js
new Date() // Sun Jun 27 2021 16:47:40 GMT+0900 (대한민국 표준시)
Date() // "Sun Jun 27 2021 16:47:45 GMT+0900 (대한민국 표준시)", 문자열로 나온다.
Date(23423423423423424) // "Sun Jun 27 2021 16:53:25 GMT+0900 (대한민국 표준시)", 무슨 값을 넣던 상관없이 현재가 나온다.

new Date(0) // Thu Jan 01 1970 09:00:00 GMT+0900 (대한민국 표준시)
new Date(24*60*60*1000) // Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시), 하루, 86,400,000 (24h * 60m * 60s * 1000ms)

new Date('May 26, 2020 10:00:10') // Tue May 26 2020 10:00:10 GMT+0900 (대한민국 표준시)

// year, month, day, hour, minute, second, millisecond
new Date(2020,2,26,10,00,00,0)
```

## 30.2 Date 메서드
1. Date.now
2. Date.parse
3. Date.UTC
1970년 1월 1일 00:00:00(UTC)를 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.
```js
Date.UTC(1970,0,2) // 86400000
```
4. Date.prototype.getFullYear
5. Date.prototype.setFullYear
6. Date.prototype.getMonth
7. Date.prototype.setMonth
8. Date.prototype.getDate
날짜(1~31)를 반환한다.
9. Date.prototype.setDate
10. Date.prototype.getDay
요일을 반환한다. 0(일요일)~6(토요일)
11. Date.prototype.getHours
12. Date.prototype.setHours
13. Date.prototype.getMinutes
14. Date.prototype.setMinutes
15. Date.prototype.getSeconds
16. Date.prototype.setSeconds
17. Date.prototype.getMilliseconds
18. Date.prototype.setMilliseconds
19. Date.prototype.getTime
1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초를 반환한다.
20. Date.prototype.setTime
21. Date.prototype.getTimezoneOffset
로캘과 UTC의 차이를 분단위로 반환한다.
```js
const today = new Date()
today.getTimezoneOffset() // 540
today.getTimezoneOffset() / 60 // -9
```
22. Date.prototype.toDateString
23. Date.prototype.toTimeString
24. Date.prototype.toISOString
25. Date.prototype.toLocaleString
인수에 알맞는 로캘로 시간을 반환한다.
```js
const today = new Date()
today.toLocaleString('en-US')
"6/27/2021, 5:10:37 PM"
```

# 31 RefExp
## 31.3 RegExp 메서드
1. RegExp.prototype.exec
```js
const target = 'Is this all there is?'
const regex=/is/
regex.exec(target) // ["is", index: 5, input: "Is this all there is?", groups: undefined]
```
exec는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환한다.
2. RegExp.prototype.test
3. String.prototype.match
```js
'is'.match(/is/g) // ["is"]
```

## 31.4 플래그
i: 대소문자 구분 x

g: 전역 검색

m: 문자열의 행이 바뀌더라도 계속 검색

## 31.5 패턴
1. 반복 검색
```js
/A{1,2}/ // 최소 1번, 최대 2번 반복되는 문자열 찾음
/A{1,}/ // 최소 1번, 최대 n번
/A{2}/ // 2번 반복
/A+/ // 최소 한 번 이상
/A?/ // 최대 한번 또는 0번
```

2. OR 검색
```js
/A|B/ // A또는 B
/[AB]/ // 위와 같음
``` 
3. NOT 검색
```js
/[^0-9]/
```
4. 시작 위치로 검색
```js
/^[0-9]/
```
5. 마지막 위치로 검색
```js
/com$/
```
