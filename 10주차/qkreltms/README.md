# 40 이벤트
## 40.1 이벤트 드리븐 프로그래밍
브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 이벤트를 발생시킨다.
```js
const button = document.querySelector('button')
button.onclick = () => {}
```
## 40.2 이벤트 타입
1. 마우스 이벤트
```
이벤트 타입        이벤트 발생 시점
click               마우스 버튼 클릭
dblclick            마우스 버튼 더블 클릭
mousedown           마우스 버튼 눌릴 때
mouseup             마우스 버튼을 놓았을 때
mousemove           마우스 커서 움직일 때
mouseenter          마우스 커서를 HTML 요소 안으로 이동했을 때(버블링이 되지 않음)
mouseover           마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링 됨)
mouseleave          마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링되지 않음)
mouseout            마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링된다)
```
2. 키보드 이벤트
```
이벤트 타입         이벤트 발생 시점
keydown             모든 키를 눌렀을 때 발생. 문자, 숫자, 특수 문자, enter 키를 눌렀을 때는 연속적으로 발생하지만 그 외의 키는 한번만 발생
keypress            문자 키를 눌렀을 때 연속적으로 발생한다. deprecated 됨
keyup               누르고 있던 키를 놓았을 때 한 번만 발생한다. 
```
3. 포커스 이벤트
```
이벤트 타입         이벤트 발생 시점
focus               HTML 요소 포커싱 될 때(버블링 x)
blur                HTML 요소 포커싱 잃을 때(버블링 x)
focusin             HTML 요소 포커스 될 때(버블링)
focusout            HTML 요소 포커스 잃을 때(버블링)
```
4. 폼 이벤트
```
이벤트 타입         이벤트 발생 시점
submit              submit 눌릴 때
reset               reset 눌릴 때
```
5. 값 변경 이벤트
```
이벤트 타입         이벤트 발생 시점
input               input 요소 값이 입력될 때
change              input 요소 값이 변경될 때
readystatechange    HTML 문서의 로드와 파싱 상태를 나타내는 document.readyState 프로퍼티 값이 변경될 때
```
6. DOM 뮤테이션 이벤트
```
이벤트 타입         이벤트 발생 시점
DOMContentLoaded    DOM 생성 완료
```
7. 뷰 이벤트
```
이벤트 타입         이벤트 발생 시점
resize              브라우저 윈도우 리사이즈 시 연속적으로 발생(오직 window 객체에서만 발생)
scroll              document 또는 HTML 요소 스크롤시 연속적으로 발생
```
8. 리소스 이벤트
```
이벤트 타입         이벤트 발생 시점
load                DOMContentLoaded 이벤트 발생 이후, 모든 리소스(이미지, 폰트 등)의 로딩이 완료될 때
unload              리소스가 언로드 될 때(주로 새로운 웹페이지를 요청한 경우)
abort               리소스 로딩 중단시
error               리소스 로딩 실패
```
## 40.3.3 addEventListener 메서드 방식
```js
Element.addEventListener('eventType', ()=>{}, useCapture) // 3번째 인자 버블링 단계의 이벤트 캐치 여부
```
onclick 프로퍼티로 이벤트 등록 방식은 중복이 되지 않는 것에 반면 addEventListener는 이벤트는 중복이 된다.
```js
button.addEventListener('click', () => {})
button.addEventListener('click', () => {})
```
## 40.4 이벤트 핸들러 제거
addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 정확히 일치해야 이벤트 핸들러가 제거된다.
## 40.5 이벤트 객체
이벤트 객체를 생성할 수 있다.
```js
new FocusEvent('focus')
new MouseEvent('click')
```
## 40.5.2 이벤트 객체의 공통 프로퍼티
```
공통 프로퍼티           설명                                                                타입
type                    이벤트 타입                                                        string
target                  이벤트를 발생시킨 DOM 요소                                          DOM
currentTarget           이벤트 핸들러가 바인딩된 DOM                                        DOM
eventPhase              이벤트 전파 단계(0: 없음, 1: 캡처링, 2: 타깃, 3: 버블링)            number
bubble                  이벤트를 버블링으로 전파하는지 여부
                        다음 이벤트는 버블링이 되지 않는다.
                        focus/blur, load/unload/abort/error, mouseenter/mouseleave          boolean
cancelable              preventDefault 메서드를 호출하여 이벤트 기본 동작을 취소할 수 있는지 여부
                        다음 이벤트는 취소할 수 없다.
                        focus/blur, load/unload/abort/error, dblclick/mouseenter/mouseleave boolean
defaultPrevented        preventDefault 메서드를 호출하여 이벤트를 취소했는지 여부           boolean
isTrusted               사용자의 행위에 의해 발생한 이벤트인지 여부
                        예를 들어 dispatchEvent 메서드를 통해 인위적 발생시킨 이벤트의 경우 boolean
                        false
timeStamp               이벤트가 발생한 시각(1970/01/01/00:00:0)부터 경과한 밀리초          number
```
## 40.6 이벤트 전파
요소의 이벤트가 발생하면
1. 캡처링: window에서부터 이벤트 타깃 방향으로 전파
2. 타켓: 이벤트 타깃에 도달
3. 버블링: 이벤트 타깃에서 시작해 window 방향으로 전파
## 40.7 이벤트 위임
여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하여 

개별로 등록하지 않아도 됨

## 40.8 DOM 요소의 기본 동작 조작
DOM 요소는 저마다 기본 동작이 있다.

예를 들어 a 요소를 클릭하면 href 어트리뷰트에 지정된 링크로 이동.

이벤트 객체의 preventDefault 메서드를 호출함으로써 이 기본 동작을 중단시킬 수 있다.
## 40.8.2 이벤트 전파 방지
stopPropagation 메서드를 사용해 호출 시점에서부터 캡처링 & 버블링 전파를 중단시킨다.
## 40.9 이벤트 핸들러 내부의 this
이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
```html
<button onclick="handleClick(this)"></button>
<script>
    function handleClick(button) {
        console.log(button) // 이벤트를 바인딩한 button 요소
        console.log(this) // window
    }
</script>
```
## 40.9.2 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식
```html
<button class='btn1'></button>
<button class='btn2'></button>
<script>
    const $button1 = document.querySelector('.btn1')
    const $button2 = document.querySelector('.btn2')
    $button1.onclick = function (e) {
        // 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this) // $button1
    }
    $button2.addEventListener('click', function (e) {
        console.log(this) // $button2
    })
</script>
```

아래의 경우도 위와 마찬가지이다.
```html
<button class='btn'></button>
<script>
    class App {
        constructor() {
            this.$button = document.querySelector('.btn')
            this.$button.onclick = this.increase
        }
        increase() {
            this.$button.textContent = ++this.count // 여기서의 this는 button element이므로 잘못된 값이 나온다.
        }
    }
</script>
```
## 40.11 커스텀 이벤트
```js
// 앞서 언급했듯 아래의 함수를 호출해 커스텀 이벤트를 만들 수 있다.
new KeyboradEvent('keyup')
new CustomEvent('foo')
```
커스텀 이벤트는 isTrusted 프로퍼티가 언제가 false(인위적)이다.
## 40.11.2 커스텀 이벤트 디스패치
커스텀 이벤트는 onclick과 같은 어트리뷰트로 이벤트 핸들러 등록을 할 수 없으므로 addEventListener 방식으로 등록해야 한다.

# 41 타이머
## 41.2 타이머 함수
## 41.2.1 setTimeout / clearTimeout
```
매개변수            설명
func                타이머가 만료된 뒤 호출된 콜백
delay               타이머 만료시간
param1,2,...        콜백 함수에 전달해야 할 인수가 존재할 경우
```

## 41.2.2 setInterval / clearInterval
## 41.3 디바운스와 스로틀
디바운스: 일정 시간 동안 이벤트 핸들러 호출하지 않다가(무시) 일정 시간이 경과한 이후에 이벤트 한번만 호출

스로틀: 이벤트 발생시 일정 시간이 지나기 전에 다시 호출되지 않도록 함
# 42 비동기 프로그래밍
## 42.1 동기 처리와 비동기 처리
자바스크립트 엔진은 단 하나의 실행 컨택스트 스택을 갖고 싱글 스레드 방식으로 동작한다.

그렇기 때문에 시간이 걸리는 태스크를 실행할 경우 블로킹이 발생한다.

그러나 아래의 경우 블로킹이 발생하지 않는다.
```js
function foo(){}
function bar(){}

setTimeout(foo, 1000)
bar()
// bar이 먼저 실행되고 foo가 다음에 실행된다.
```
## 42.2 이벤트 루프와 태스크 큐
자바스크립트는 싱글 스레드이지만 브라우저가 동시에 처리되는 것처럼 느껴진다.

자바스크립트의 동시성을 지원하는 것이 바로 이벤트 루프이다.

![이벤트루프](이벤트루프.jpg)
구글의 V8 자바스크립트 엔진을 비롯한 대부분의 자바스크립트 엔진은 크게 2개의 영역으로 구분할 수 있다.
1. 콜 스택: 실행 컨텍스트가 바로 콜 스택이다.
함수를 호출하면 함수 실행 컨텍스트가 순차적으로 콜 스택에 푸시되어 순차적으로 실행된다. 자바스크립트 엔진은 단 하나의 콜 스택을 사용하기 때문에 최상위 실행 컨텍스트가 종료되어 콜 스택에서 제거되기 전까지는 다른 어떤 태스크도 실행되지 않는다.

2. 힙: 객체가 저장되는 메모리 공간
객체는 원시 값과는 달리 크기가 정해져 있지 않으므로 할당해야 할 메모리 공간의 크기를 런타임에 결정해야 한다. 

자바스크립트 엔진은 단순히 태스크가 요청되면 콜 스택을 통해 요청된 작업을 순차적으로 실행할 뿐이다.

비동기 처리에서 소스코드의 평가와 실행을 제외한 모든 처리는 자바스크립트 엔진을 구동하는 환경인 브라우저 또는 Node.js가 담당한다.

이를 위해 브라우저 환경은 태스크 큐와 이벤트 루프를 제공한다.

- 태스크 큐: setTimeout이나 setInterval 같은 비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역이다.
- 이벤트 루프: 콜 스택에 현재 실행 중인 실행 컨텍스트가 있는지, 그리고 태스크 큐에 대기 중인 함수가 있는지 반복해서 확인한다. 만약 콜 스택이 비어 있고 태스크 큐에 대기 중인 함수가 있다면 이벤트 루프틑 순차적으로 태스크 큐에 대기 중인 함수를 콜 스택으로 이동시킨다.

```js
function foo(){}
function bar(){}

setTimeout(foo, 1000)
bar()
// bar이 먼저 실행되고 foo가 다음에 실행된다.
```
1. 전역 코드가 평가되어 전역 실행 컨텍스트가 생성되고 콜 스택에 푸시된다.
2. 전역 코드가 실행되기 시작하여 setTimeout 함수가 호출된다. 
3. setTimeout 함수가 실행 컨텍스트에 올라가서 호출되고 브라우저에서 타이머가 발동된다.
4. 호출이 끝나면 실행 컨텍스트에서 팝된다.
5. foo 함수가 태스크 큐에 푸시되어 대기하게 된다. 
6. bar 함수를 실행 컨텍스트에 올리고 실행, 팝 된다.
7. 전역 코드 실행이 종료되어 팝된다.
8. 이벤트 루프에의해 실행 컨텍스트가 비었고 타이머가 완료됐다는 것을 탐지된다.
9. foo 함수가 콜 스택에 올라오고 실행, 팝 된다.  
# 43 Ajax
Ajax란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.

## 43.2 JSON
클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.

언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용할 수 있다.

## 43.3 XMLHttpRequest
자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLttpRequest 객체를 사용한다.

## 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드
```
프로토타입 프로퍼티                 설명
readyState                         HTTP요청의 현재 상태 UNSENT:0, OPENED: 1, HEADERS_RECEIVED: 2, LOADING: 3, DONE: 4
status                              HTTP 요청에 대한 응답 상태
statusText                          HTTP 요청에 대한 응답 메시지
responseType                        HTTP 응답 타입
response                            HTTP 요청에 대한 응답 몸체
responseText                        서버가 전송한 HTTP 요청에 대한 응답 문자열

이벤트 핸들러 프로퍼티              설명
onreadystatechange                  readyState 프로퍼티가 변경된 경우
onloadstart                         HTTP 요청에 대한 응답을 받기 시작한 경우
onprogress                          HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생
onabort                             abort 메서드에 의해 HTTP 요청이 중단된 경우
onerror                             HTTP 에러 발생
onload                              HTTP 요청 완료
ontimeout                           HTTP 요청 시간 초과
onloadend                           HTTP 요청 완료(성공 또는 실패)

메서드                              설명
open                                HTTP 요청 초기화
send                                HTTP 요청 전송
abort                               이미 전송된 HTTP 요청 중단
setRequestHeader                    특정 HTTP 요청 헤더의 값을 설정
getResponseHeader                   특정 HTTP 요청 헤더의 값을 문자열로 반환

정적 프로퍼티                       값      설명
UNSENT                              0       open 메서드 호출 이전
OPENED                              1       open 메서드 호출 이후
HEADERS_RECEIVED                    2       send 메서드 호출 이후
LOADING                             3       서버 응답 중
DONE                                4       서버 응답 완료
```

## 43.3.3 HTTP 요청 전송
```js
const xhr = new XMLHttpRequest()
xhr.open('GET', '/url')
xhr.setRequestHeader('content-type', 'application/json')
xhr.send()
xhr.onreadystatechage = () => {
    // 응답이 완료되지 않았으면 그냥 종료
    if (xhr.readyState !== XMLHttpRequest.DONE) return
    if (xhr.status === 200) {
        console.log(xhr.response)
    }
}

xhr.onload = () => {
    // onload를 사용하면 if (xhr.readyState !== XMLHttpRequest.DONE) return 을 확인하지 않아도 된다.
    if (xhr.status === 200) {
        console.log(xhr.response)
    }
}
```