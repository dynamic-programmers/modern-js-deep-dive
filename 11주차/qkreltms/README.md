# 44 REST API
REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처이고, REST API는 REST 기반 API 서비스를 말한다.
## 44.1 REST API의 구성
자원, 행위, 표현 3가지 요소로 구성된다.
```
구성 요소       표현 방법
자원            URL(엔드포인트)
행위            HTTP 요청 메서드
표현            페이로드
```
## 44.2 REST API의 설계 원칙
1. URI는 리소스를 표현해야 한다.
```
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```
2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.
```
HTTP 요청 메서드        종류        목적                페이로드
GET               index/retrieve 모든/특정 리소스 취득      X
POST                create         리소스 생성              O  
PUT                 replace        리소스 전체 교체         O
                                예: todo에서 id 제외한 나머지 전체 교체
PATCH               modify         리소스 일부 수정         O
DELETE              delete         모든/특정 리소스 삭제    X
```
# 45 프로미스
자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용한다. 하지만 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러의 처리가 곤란하여 여러 개의 비동기 처리를 한번에 처리하는 데도 한계가 있다.

그리하여 ES6에서는 비동기 처리를 위한 또 다른 패턴으로 프로미스를 도입했다.

## 45.1 비동기 처리를 위한 콜백 패턴의 단점
1. 콜백 헬
2. 에러 처리의 한계
setTimeout 함수의 콜백 함수를 호출한 것이 setTimeout 함수가 아니라 이벤트 루프에 의해 콜 스택으로 푸시되어 실행된다.

에러는 호출자 방향으로 전파된다.

그러므로 try, catch가 작동되지 않는다.
```js
try {
    setTimeout(() => throw new Error(''), 1000)
} catch (e) {
    console.log(e)
}
```

```js
const promise = new Promise((res, rej) => {
    if (비동기 처리 성공) {
        resolve('result')
    } else {
        reject('failed')
    }
})
```
![프로미스](./promises.png)
## 45.3 프로미스의 후속 처리 메서드
1. Promise.prototype.then(ressolved, reject)
2. Promise.prototype.catch
3. Promise.prototype.finally
## 45.6 프로미스의 정적 메서드
1. Promise.resolve / Promise.reject
이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다.
```js
const res = Promise.resolve([1,2,3])
res.then(console.log) // [1,2,3]
// 아래와 같다
const res = new Promise(res => res([1,2,3]))
res.then(console.log) // [1,2,3]
``` 
2. Promise.all
여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다.

인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료한다.

인수로 전달받은 이터러블 요소가 프로미스가 아닌 경우 자동으로 Promise.resolve 메서드롤 통해 프로미스로 래핑한다.
```js
Promise.all([1,2,3]).then(console.log) // [1,2,3]
```
3. Promise.race
가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환한다.

인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료한다.
4. Promise.allSettled
ES11에서 도입되었고 인수로 전달된 프로미스가 모두 settled 상태(비동기 처리가 수행된 상태, 즉 fulfilled 또는 rejected 상태)가 되면 처리 결과를 배열로 반환한다.
```js
Promise.allSettled([
    new Promise(res => setTimeout(() => res(1), 2000)),
    new Promise((_, rej) => setTimeout(() => rej(new Error('error')), 1000)) 
]).then(console.log)
(2) [{…}, {…}]
/*
status: "fulfilled"
value: 1
[[Prototype]]: Object

reason: Error: error at <anonymous>:3:50
status: "rejected"
[[Prototype]]: Object
length: 2
[[Prototype]]: Array(0
*/
```
## 45.7 마이크로태스크 큐
```js
setTimeout(() => console.log(1), 0)
Promise.resolve().then(() => console.log(2)).then(() => console.log(3))
```
위의 함수를 실행하면 2 => 3 => 1 순으로 출력된다.

왜냐하면 Promise를 담아두는 마이크로태스크 큐가 존재하고, 이벤트 루프의 선택 우선순위가 setTimeout이 담겨있는 태스크큐보다 높다.
# 46 제너레이터와 async/await
# 47 에러 처리