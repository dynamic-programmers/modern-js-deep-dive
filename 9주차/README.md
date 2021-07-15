# 36 디스트럭처링 할당
```js
const [one, two, three, four=4]=[1,2,3]
const { firstName: name, secondName='test2' } = {fisrtName:'test'}
// 배열, 객체 디스트럭처링 할당 혼용
const todos = [{ id:1 },{ id:2 }]
const [,{id}]=todos

// 중첩 객체
const user = {
    address: {
        zipCode: 'test'
    }
}
const { address: {zipCode}}=user
```
# 37 Set과 Map
## Set & Map
```
Set                                 
0. 생성: new Set([])                            
1. 요소 개수 확인: Set.prototype.size           
2. 요소 추가: Set.prototype.add()//중복 미허용  
3. 요소 존재 여부 확인: Set.prototype.has       
4. 요소 삭제: Set.prototype.delete
5. 요소 일괄 삭제: Set.prototype.clear
6. 요소 순회: Set 객체는 이터러블이다.
7. 교집합: Set.prototype.intersection
8. 합집합: Set.prototype.union
9. 차집합: Set.prototype.difference
10. 상위 집합인지 확인: Set.prototype.isSuperset

Map
0. 생성: new Map([])
1. 요소 개수 확인: Map.prototype.size
2. 요소 추가: Map.prototype.set // 중복된 키를 가질 수 없다. 모든 값을 키로 설정가능 (ex: 객체), NaN === NaN, false인데 Map에서는 true
3. 요소 존재 여부 확인: Map.prototype.has
4. 요소 삭제: Map.prototype.delete
5. 요소 일괄 삭제: Map.prototype.clear
6. 요소 순회: Map.prototype.keys, values, entires도 허용
``` 

# 38 브라우저의 렌더링 과정
1. HTML parsing
2. CSSOM 생성
3. 자바크립트 파싱하여 AST(Abstract Syntax Tree) 생성 및 바이트 코드 변경. 이 때 DOM and CSSOM 컨트롤 가능
5. Render Tree(DOM, CSSOM 병합)
4. Layout(Reflow)
5. Paint(Repaint)
6. Composite Layers(Layer 병합)

## 38.1 요청과 응답
브라우저에 https://poiemaweb.com을 입력하면
1. 루트 요청(/, 스키마와 호스트 만으로 구성된 URI에 의한 요청)이 poiemaweb.com 서버로 전송된다.
루트 요청에는 명확히 리소스를 요청하는 내용이 없지만 일반적으로 서버는 루트 요청에 대해 암묵적으로 index.html을 응답한다.
2. 서버는 루트 요청에 대해 서버의 루트 폴더에 존재하는 정적 파일 index.html을 클라이언트로 응답한다.
만약 index.html이 아닌 다른 정적 파일을 서버에 요청하려면 URI의 호스트 뒤의 패스에 기술한다.

자바스크립트를 통해 동적으로 서버에 정적/동적 데이터를 요청할 수도 있다.(ajax)

요청한 내용을 확인하면 index.html 뿐만아니라 CSS, 자바스크립트, 이미지, 폰트 등도 응답된다.

이는 HTML parsing 도중에 외부 리소스를 요청했기 때문이다.

## 38.2 HTTP 1.1과 HTTP 2.0
HTTP/1.1은 기본적으로 커넥션당 하나의 요청과 응답만 처리한다. 즉, 여러 개의 요청을 한 번에 전송 할 수 없고 응답 또한 마찬가지다.

이처럼 HTTP/1.1은 리소스의 동시 전송이 불가능한 구조이므로 요청할 리소스의 개수에 비례하여 응답 시간도 증가하는 단점이 있다.

HTTP/2.0은 커넥션당 여러 개의 요청과 응답, 즉 다중 요청/응답이 가능하다. HTTP/1.1에 비해 약 50% 속도가 빠르다.
# 39 DOM
## 39.1.2 노드 객체의 타입
![DOM](DOM).jpg)
1. Document Node(문서 노드)
DOM 트리의 최상위에 존재, 집입점 역할을 함
2. Element Node
HTML 요소를 가리키는 객체
3. Attribute Node
HTML 요소의 어트리뷰트를 가리키는 객체
4. Text Node
HTML 요소의 텍스트를 기리키는 객체

DOM 트리의 최종단이다.
## 39.1.3 노드 객체의 상속 구조
모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받는다.

추가적으로 문서 노드는 Document, HTMLDocument 인터페이스를 상속 받고

어트리뷰트 노드는 Attr

텍스트 노드는 CharacterData 인터페이스를 상속받는다.

## 39.2 요소 노드 취득
1. id를 이용
document.getElementById
2. tag
document.getElementsByTagName, 인수로 '*'을 전달하면 모든 요소 노드를 가져온다.
3. class
document.getElementsByClassName
4. CSS 선택자를 이용
document.querySelector/querySelectorAll
5. 특정 요소를 취득할 수 있는지 확인
Element.matches, Element.matches('#fruits > li.apple')
6. HTMLCollection과 NodeList
DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬랙션 객체이다. 

유사 배열 객체이면서 이터러블이다.

주의: HTMLCollection과 NodeList는 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체이다. 단, NodeList는 대부분 실시간으로 반영하지 않지만 chilNodes 프로퍼티가 반환하는

NodeList 객체는 실시간으로 반영한다.

```js
<li class='red'>1</li>
<li class='red'>2</li>
<li class='red'>3</li>

element=document.getElementsByClassName('red') // HTMLCollection(3) [li.red, li.red, li.red]

for (let i = 0; i<element.length;i++) {
    element[i].className='blue'
}
// 결과, 첫 번째, 세 번째만 blue로 바뀐다.
// 1. red에서 blue로 변경된다. class가 red인 것만 HTMLCollection에 들어있어야 하므로 리스트에서 제외된다.
// 2. i=1 일 때 HTMLCollection[1]는 전 HTMLCollection의 3번째와 같다. 3번째를 red에서 blue로 변경하고 마찬가지로 제외된다.
// 3. HTMLCollection의 길이가 1로 줄었기 때문에 조건문이 false가 나와 종료된다. 

// 해결 방법1. 역방향으로 순회하며 적용
// 해결 방법2. while문으로 length가 존재하지 않을 때까지 적용
// 해결 방법3. 유사 배열인 HTMLCollection을 forEach를 사용해 배열로 변환해 부작용 제거
// 해결 방법4. NodeList를 반환하는 함수 사용(querySelectorAll)
```

## 39.3 노드 탐색
![DOM2](DOM2).jpg)
요소 노드를 취득한 다음, 취득한 요소 노드를 기점으로 DOM트리의 노드를 옮겨 다닌다.

## 39.4 노드 정보 취득
1. Node.prototype.nodeType
노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환한다.

Node.ELEMENT_NODE: 1

Node.TEXT_NODE: 3

Node.DOCUMENT_NODE: 9

2. Node.prototype.nodeName
노드의 이름(태그)을 문자열로 반환하다.

## 39.5 요소 노드의 텍스트 조작
1. nodeValue
setter와 getter 모두 존재하는 접근자 프로퍼티이다. 텍스트 노드의 텍스트 값을 변경 가능하다.

2. textContent
setter와 getter 모두 존재하는 접근자 프로퍼티이다. 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.
```js
<div>Hello<span>World</span></div>
getElementById(...).textContent // Hello World
```
## 39.6 DOM 조작
노드를 추가하거나 삭제, 교체한다. 

리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다.

1. innerHTML
문자열을 DOM으로 바꿔준다. 단, XSS에 취약하므로 위험하다. 악성코드가 포함되어있다면 그대로 실행될 수 있다.
2. insertAdjacentHTML
기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.
```js
Element.insertAdjacentHTML('beforebegin', '<p>beforebegin</p>')
Element.insertAdjacentHTML('afterbegin', '<p>afterbegin</p>')
Element.insertAdjacentHTML('beforeend', '<p>beforeend</p>')
Element.insertAdjacentHTML('afterend', '<p>afterend</p>')
```

## 39.6.3 노드 생성과 추가
1. document.createElement
2. document.createTextNode
3. Node.prototype.appendChild
여러개의 노드를 생성하고 추가할 때 반목문에서 횟수만큼 진행하는 것은 리플로우, 리페인트가 여러번 진행되므로 좋지않다.

해결 방법 1: 루프에서 생성하고 컨테이너에 담는다. 루프가 끝나고 마지막에 컨테이너를 대상에 추가한다.

단점: 컨테이너를 생성해야한다.

해결 방법 2: 마찬가지로 진행하되, DocumentFragment 노드를 사용하면 노드를 담을수 있지만, 컨테이너가 생성되지 않는다.

document.createDocumentFragment

4. Node.prototype.insertBefore
## 39.6.7 노드 복사
1. Node.prototype.cloneNode, 두 번째인자를 통해 deep(true), shallow copy할 지 결정할 수 있다.
## 39.6.8 노드 교체
1. Node.prototype.replaceChild(new, old)
## 39.6.9 노드 삭제
1. Node.prototype.removeChild

## 39.7 어트리뷰트
요소 노드의 모든 어트리뷰트는 Element.prototype.arrtibutes 프로퍼티로 취득할 수 있다.

attributes 프로퍼티는 getter만 존재하는 읽기 전용 접근자 프로퍼티이며 NamedNodeMap 객체를 반환한다.

## 39.7.2 HTML 어트리뷰트 조작
arrtibutes 프로퍼티는 getter이므로 조작할 수 없다. 그러므로 메소드를 이용한다. 

1. Element.prototype.getAttribute
2. Element.prototype.setAttribute

## 39.7.3 HTML 어트리뷰트 vs DOM 프로퍼티
어트리뷰트는 HTML 요소의 초기 상태를 지정이는 변하지 않는다.(getAttribute)

요소 노드의 최신 상태는 DOM 프로퍼티가 관리한다.(예 input.value)

즉, input.value != input.getAttribute

단, 사용자의 입력에 의한 상태 변화와 관계없는 id 어트리뷰트와 프로퍼티는 둘 다 변경된다.
```js
input.id = 'foo'
input.id // foo
input.getAttribute('id') // foo
```

## 39.7.4 data 어트리뷰트와 dataset 프로퍼티
data 어트리뷰트를 사용해 자바스크립트 간에 데이터를 교환할 수 있다. data-user-id, data-role과 같이 data 접두사가 붙는다.

data 어트리뷰트의 값은 HTMLElement.dataset 프로퍼티로 취득할 수 있다.

```js
<div data-user-id='test'>

divElement.dataset.userId // 'test'
```

## 39.8 스타일
HTMLElement.prototype.style 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티이다.

```js
// 아래의 모든 프로프터가 추가된다.
div.style.color='blue'
div.style.width='100px'
```

style 프로퍼티를 참조하면 CSSStyleDeclaration 타입의 객체를 반환한다. 다양한 CSS 프로퍼티에 대응하는 프로퍼티를 가지고 있으며,

이 프로퍼티에 값을 할당하면  CSSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가되거나 변경된다.

## 39.8.2 클래스 조작
1. className
setter와 getter가 존재한다.
```js
div.className='test'
```
2. classList
class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.
```js
.box {
    width:,...
}
.red { color: red }
.blue { color: blue }
<div class="box red">Hello World</div>
// 실시간으로 변화를 감지한다.
div.classList // [length: 2, value: 'box blue', 0: "box", 1:"blue"]

div.classList.replace('red', 'blue')
```
DOMTokenList 객체는 class 어트리뷰트의 정보를 나타내는 컬렉션 객체로서 유사 배열 객체이면서 이터러블이다.

```
add
remove
item
contains
replace
toggle
```

## 39.8.3 요소에 적용되어 있는 CSS 스타일 참조
style 프로퍼티는 인라인 스타일만 반환한다. 따라서 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 참조할 수 없다.

이 때에 HTML 요소에 적용되어 있는 모든 CSS 스타일을 참조해야 한다면 getComputedStyle 메서드를 사용한다.

getComputedStyled의 두 번째 인수로 :after, :before과 같은 의사 요소를 지정할 수 있고, 관련 프로퍼티를 가져올 수 있다.