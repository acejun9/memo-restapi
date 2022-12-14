# memo-restapi

#### 1. 수정, 삭제 API의 request를 어떤 방식으로 사용하셨나요? (param, query, body)
- 수정은 id, (제목, 이름, 비밀번호, 내용)여러 키 값이 필요한데 비밀번호를 노출하면 안되기 때문에 PostRequestDto를 body에 담아서 보내기 위해 RequsetBody를 사용했다
- 삭제는 제목 이름 내용 필요없이 id, 비밀번호만 있으면 되기 때문에 비밀번호를 RequestParam으로 body에 담아서 보내주었다

#### 2. 어떤 상황에 어떤 방식의 request를 써야하나요?
- 데이터의 양이 많지 않고 서버에서 데이터를 가공하지 않고 클라이언트에게 그대로 보내주면 될 때 GET
- 데이터를 서버로 보내고 새리소스를 만들 때, 정보를 숨겨야 할 때 POST
- 기존 리소스를 업데이트 할 때, 변경 할 속성 뿐 아니라 모든 속성의 데이터 필요 PUT
- 기존 리소스를 수정하는 것은 PUT 방식과 유사하지만 변경하려는 특정 속성만 포함되면 된다 PATCH
- 지정된 리소스를 삭제하려 할 때 DELETE


#### 3. RESTful한 API를 설계했나요? 어떤 부분이 그런가요? 어떤 부분이 그렇지 않나요?
REST란 URI를 통해 자원을 표시하고 HTTP Method를 이용하여 데이터를 전송 및 처리하는 방법
- REST 기반으로 CRUD 의도에 맞게 api 서비스 구현, 제공


#### 4. 적절한 관심사 분리를 적용하였나요? (Controller, Repository, Service)
- Controller는 웹에서 요청이 왔을 때 클라이언트에서 받은 데이터를 적절한 Service로 넘겨주고 처리한 후 지정된 뷰에 다시 객체를 넘겨주는 역할
- Service는 알맞는 비즈니스 로직을 수행하고 가공하여 Controller에게 다시 데이터를 넘긴다. DB정보가 필요할 때 Repository에 요청
- Repository는 Entity객체에 의해 DB에 직접 접근, 관리, CRUD를 수행한다


#### 5. API 명세서 작성 가이드라인을 검색하여 직접 작성한 API 명세서와 비교해보세요!

| Method | URL | Request | Response                                                                                                                                                                                                                                                                                              |
| --- | --- | --- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GET | /api/posts | - | {<br/>{"id": 1,<br/>"title": "title1",<br/>"name": "name1",<br/>"content": "content1",<br/>"createdAt": "2022-12-08T12:00:01.123456”<br/> }, <br/>{<br/>"id": 2,<br/>"title": "title2",<br/>"name": "name2",<br/>"content": "content2",<br/>"createdAt": "2022-12-08T12:01:01.123456” }<br/> … <br/>} | 
| GET | /api/post/{id} | - | {<br/>"id": 1,<br/>"title": "title1",<br/>"name": "name1",<br/>"content": "content1",<br/>"createdAt": "2022-12-08T12:00:01.123456”<br/>}                                                                                                                                                             |
| POST | /api/post | {<br/>"title" : "title1",<br/>"name" : "name1",<br/>"password" : "password1",<br/>"content" : "content1"<br/>} | {<br/>"id": 1,<br/>"title": "title1",<br/>"name": "name1",<br/>"content": "content1",<br/>"createdAt": "2022-12-08T12:00:01.123456”<br/>}                                                                                                                                                             |
| PUT | /api/post/{id} | {<br/>"title" : "title1",<br/>"name" : "name1",<br/>"password" : "password1",<br/>"content" : "content12345"<br/>} | {<br/>"title" : "title1",<br/>"name" : "name1",<br/>"password" : "password1",<br/>"content" : "content12345",<br/>"createdAt": "2022-12-08T12:00:01.123456”<br/>}                                                                                                                                                    |
| DELETE | /api/post/{id} | {<br/>"password" :"password1"<br/>} | {<br/>"success": true<br/>}                                                                                                                                                                                                                                                                           |




## 숙련 주차 과제 Lv1

#### 1. 처음 설계한 API명세서에 변경사항이 있었나요? 어떤 점 때문 일까요? 첫 설계의 중요성에 대해
- 로그인 기능을 새로 추가하게 되어서 전에 만들었던 게시글 API의 입력값이나 로직도 변경되었다
- 변경사항이 추가되면 연관된 많은 부분을 같이 변경해야 하므로 비용이 크게 증가한다 
- 설계에 시간을 투자하는 것이 장기적으로는 시간을 단축시킬 수 있다

#### 2. ERD를 먼저 설계한 후 Entity를 개발했을 때 어떤 점이 도움이 되셨나요?
- 연관관계나 구조를 한번에 파악할 수 있었고 이해가 쉬웠다

#### 3. JWT를 사용하여 인증/인가를 구현 했을 때의 장점은?
- 서버가 여럿일 경우 client의 정보를 가지지 않은 서버로 연결됐을 때의 문제를 해결할 수 있다
- 동시 접속자가 많을 때 서버의 부하가 적다

#### 4. 반대로 JWT를 사용한 인증/인가의 한계점은?
- JWT 구현의 복잡도가 높다
- JWT에 담는 내용이 커질수록 네트워크 비용이 증가한다
- SecretKey 유출 시 조작이 가능

#### 5. 만약 댓글 기능이 있는 블로그에서 댓글이 달려있는 게시글을 삭제하려고 한다면 무슨 문제가 발생할까요? Database테이블 관점에서 해결방법은?
- 댓글이 게시글의 기본키를 외래키로 가지고 있어서 댓글이 있는 게시글을 삭제하려고 하면 삭제가 되지 않는다
- 게시글을 삭제하려고 할 때 하위에 있는 댓글들도 모두 삭제 한다.
- 게시글을 delete하는 대신 삭제된 게시글이라고 update한다

#### 6. IoC / DI에 대해 간략하게 설명
- IoC : 제어의 역전이라는 의미로 사용자가 객체를 생성하는 것이 아니고 프레임워크가 알아서 생성된 객체를 주입시켜 사용하는 것
- DI : IoC를 구현하는 디자인 패턴, 클래스 타입에 고정되지 않고 인터페이스 타입으로 주입 받아 구현할 수 있다, 의존성이 떨어지고 재사용성이 높아진다.


![ERD](/src/main/resources/static/images/ERD-Lv2.png)


| 기능        | API URL | Method | Request Header                                                                                                                                                 | Request | Response |
|-----------| --- | --- |----------------------------------------------------------------------------------------------------------------------------------------------------------------| --- | --- |
| 회원 가입     | /api/auth/signup | POST |                                                                                                                                                                | {<br/>"userName": "jun99",<br/>"password": "tjdwns123"<br/>} | signup success<br/>"statusCode": 200 |
| 로그인       | /api/auth/login | POST |                                                                                                                                                                | {<br/>"userName": "jun99",<br/>"password": "tjdwns123"<br/>} | login success<br/>"statusCode": 200 |
| 게시글 작성    | /api/post | POST | Authorization: Bearer eyJhbGciOiJIUzI1NiJ9<br/>.eyJzdWIiOiJqdW45OSI<br/>sImV4cCI6MTY3MTExMTk<br/>wNiwiaWF0IjoxNjcxMTA<br/>4MzA2fQ.my2KjfOuKPC2<br/>M11eOjqMSDytGV1fHlZc<br/>Mlq2EXWv0Ck | {<br/>"title": "title1",<br/>"content": "content1"<br/>} | {<br/>"id": 1,<br/>"title": “title1",<br/>"userName": "userName1",<br/>"content": "content1",<br/>"createdAt": "2022-12-01T12:56:36.821474"<br/>} |
| 게시글 목록 조회 | /api/posts | GET |                                                                                                                                                                |  | [<br/>{<br/>"id": 1,<br/>"title": “title1",<br/>"userName": "userName1",<br/>"content": "content1",<br/>"createdAt": "2022-12-01T12:56:36.821474"<br/>},<br/>{<br/>"id": 2,<br/>"title": “title2",<br/>"userName": "userName2",<br/>"content": "content2",<br/>"createdAt": "2022-12-01T12:56:36.821474"<br/>},<br/>{<br/>"id": 3,<br/>"title": “title3",<br/>"userName": "userName3",<br/>"content": "content3",<br/>"createdAt": "2022-12-01T12:56:36.821474"<br/>}<br/>…<br/>] |
| 선택 게시글 조회 | /api/posts/{id} | GET |                                                                                                                                                                |  | {<br/>"id": 1,<br/>"title": “title1",<br/>"userName": "userName1",<br/>"content": "content1",<br/>"createdAt": "2022-12-01T12:56:36.821474"<br/>} |
| 선택 게시글 수정 | /api/posts/{id} | PUT | Authorization: Bearer eyJhbGciOiJIUzI1NiJ9<br/>.eyJzdWIiOiJqdW45OSI<br/>sImV4cCI6MTY3MTExMTk<br/>wNiwiaWF0IjoxNjcxMTA<br/>4MzA2fQ.my2KjfOuKPC2<br/>M11eOjqMSDytGV1fHlZc<br/>Mlq2EXWv0Ck      | {<br/>"title": "title11",<br/>"content": "content111"<br/>} | {<br/>"id": 1,<br/>"title": “title11",<br/>"userName": "userName1",<br/>"content": "content111",<br/>"createdAt": "2022-12-01T12:56:36.821474"<br/>} |
| 선택 게시글 삭제 | /api/posts/{id} | DELETE | Authorization: Bearer eyJhbGciOiJIUzI1NiJ9<br/>.eyJzdWIiOiJqdW45OSI<br/>sImV4cCI6MTY3MTExMTk<br/>wNiwiaWF0IjoxNjcxMTA<br/>4MzA2fQ.my2KjfOuKPC2<br/>M11eOjqMSDytGV1fHlZc<br/>Mlq2EXWv0Ck      |  | delete success<br/>"statusCode": 200|

