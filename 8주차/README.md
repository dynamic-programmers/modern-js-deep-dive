# 32 String
new 키워드를 사용하여 String 인스턴스를 생성한다. 만약 인수가 전달되지 않으면 [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성한다.

String은 유사 배열이면서 이터러블이다. 따라서 배열과 유사하게 인덱스를 사용하여 각 문자에 접근할 수 있다.

```js
"test"[0] // 't'

// 문자열은 원시값으므로 변경할 수 없지만, 에러가 발생하지 않는다.
let str = "test"
str[0] = "T"
str // 'test'

new String(123) // 문자열로 강제 변환한다.
new String(NaN) // "NaN"
new String(Infinity) // "Infinity"
new String(true) // "true"
```

## 32.3 String 메서드
메서드로 원본 값을 변경할 수 없고 새로운 객체로 복사되어 나온다.

1. String.prototype.indexOf
가장 앞쪽부터 대상 문자열을 검색하여 인덱스를 반환한다. 실패하면 -1이 나온다.
```js
const str='hello world'
str.indexOf('l') // 2
// 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있따,
str.indexOf('l', 3) // 3
```
2. String.prototype.search
3. String.prototype.includes
4. String.prototype.startsWith
5. String.prototype.endsWith
6. String.prototype.chartAt
주어진 인수에 위치한 인덱스의 문자를 반환한다.
7. String.prototype.substring
8. String.prototype.slice
substring과 비슷하지만 음수인 인자를 전달받을 수 있다. 음수를 전달 받으면 문자열 가장 뒤에서부터 시작한다.
9. String.prototype.toUpperCase
10. String.prototype.toLowerCase
11. String.prototype.trim
12. String.prototype.repeat
python의 문자열 곱하기와 같다.
13. String.prototype.replace
14. String.prototype.split
# 33 7번째 데이터 타임 Symbol
## 33.1 심벌이란?
ES6에서 도입된 7번 원시형 데이터 타입으로, 다른 값과 중복되지 않은 유일무이한 값이다. 따라서 주로 유니크한 값이 필요할 때 사용한다.

주로 프로퍼티 키로 사용한다.

심벌의 값은 외부로 노출되지 않아 확인할 수 없다.(log로 확인할 수 없다.)

하위 호환성을 보장할 수 있다.(33.6)
```js
const mySymbol = Symbol()
const mySymbol2 = new Symbol() // error, 원시값이기 때문에 new 연산자와 함께 호출할 수 없다.
const mySymbol3 = Symbol('test')
const mySymbol4 = Symbol('test') // 인수가 같더라도 유일무이한 심벌 값을 생성한다.
mySymbol3 === mySymbol4 // false
```

심벌 값도 '.'을 사용하여 접근하면 암묵적으로 래퍼 객체를 생성한다.

심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다. 단, 불리언 타입은 변환된다.
```js
const mySymbol = Symbol()
!!mySymbol // true
```

1. Symbol.for
Symbol은 전역 심벌 레지스트리에 등록되지 않지만, 유일한 키 값을 갖은 심벌을 생성하거나 전역 심벌 레지스트리에서 인수로 전달받은 문자열 키와 일치하는 심벌 값을 검색한다.
```js
Symbol('mySymbol')
Symbol.for('mySymbol') // Symbol(mySymbol)
```
2. Symbol.keyFor
전역 심벌 레지스트리에서 심벌을 가져온다.
```js
Symbol.keyFor(Symbol.for('test')) // 'test'
```

## 33.3 심벌과 상수
값에 특별한 의미가 없고 상수 이름 자체에 의미가 있을 때 심벌을 사용하기 좋은 케이스이다.
```js
const Direction = Object.freeze({
    UP: Symbol('UP')
})

const mine=Direction.UP
mine === Direction.UP // true
```
```ts
enum Direction {
    UP= 1
}
const min=Direction.UP
mine === Direction.UP
```

## 33.4 심벌과 프로퍼티 키
심벌로 동적 프로퍼티 생성이 가능하다.
```js
const obj = {
    [Symbol.for('mySymbol')]: 1
}
obj[Symbol.for('mySymbol')] // 1
```

## 33.5 심벌과 프로퍼티 은닉
심벌 프로퍼티는 for ... in, Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다. 단, Object.getOwnPropertySymbols로는 찾을 수 있다.

## 33.6 심벌과 표준 빌트인 객체 확장
빌트인 객체 확장은 권장하지 않으나 필요하다면 심볼을 사용해 추후 충돌 대비하라.
```js
Array.prototype[Symbol.for('sum')]=function(){
...
}
[1,2][Symbol.for('sun')]() // 3
```

## 33.7 Well-Known Symbol
자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-known Symbol이라 부른다.

일례로 순회 가능한 빌트인 이터러블은 Symbol.iterator를 키로 갖는 메서드를 가지며 Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다.

만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하고 싶다면 아래처럼
```js
const iterable = {
    [Symbol.iterator]() {
        let cur = 1
        const max = 5
        // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
        return {
            next() {
                return { value: cur++, done: cur > max+1}
            }
        }
    }
}

for (const num of iterable) {
    console.log(num)
}
```
# 34 이터러블

# 35 스프레드 문법