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