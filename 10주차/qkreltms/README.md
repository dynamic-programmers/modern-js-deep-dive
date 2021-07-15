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
# 41 타이머
# 42 비동기 프로그래밍
# 43 Ajax