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
# 46 제너레이터와 async/await
# 47 에러 처리