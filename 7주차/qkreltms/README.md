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
# 31 RefExp