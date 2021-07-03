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

# 34 이터러블
# 35 스프레드 문법