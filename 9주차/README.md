## 36 디스트럭처링 할당
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
## 37 Set과 Map
## 38 브라우저의 렌더링 과정
## 39 DOM