### CodeFirst
---

1. 프레임워크 업그레이드
- ASP.NET MVC Core 2.1
2. EntityFramework core 설치
3. DbContext 생성 -> Table 생성할 수 있는 코드
를 작성.
4. DbContext -> 실제 테이블을 생성

Code First
Model Class -> Dbcontext -> 실제 테이블을 생성

User 모델
Note 모델 -> 게시판

User 클래스의 프로퍼티
사용자 번호(PK)
사용자 이름
사용자 ID
사용자 Password

Note (게시판)
게시물 번호(PK)
게시물 제목
게시물 내용
작성자(숫자     사용자 번호)

connectionstring google 검색하면 connectionstrings.com 사이트에서 커넥션스트링 형식 다 얻을 수 있음.

PM> add-migration FirstMigration
PM> update-database

사용자 로그인, 회원가입
회원가입 -> 테이블 -> 사용자 정보 입력 -> DB 저장
로그인 -> 사용자 정보를 입력 -> 사용자 정보를 보유

GET : DB -> 받는다
POST : DB <- 전달

Model > 기본 엔티티 클래스
User
-> UserNo
-> UserName
iD,Password

ViewMoedl 은 View 를 위한 모델.
근본적으로 저 뜻은 아닌데, 디자인 패턴.
MVC ( Model, View, Controller )
WPF ( M V VM )

Cache 
- 목적 : 자주 불러오는 데이터를 메모리에 담아서 출력하는 것
- 장점 : 컴퓨팅 비용 감소, 데이터를 출력하는 속도가 빨라짐.
- 단점 : 메모리를 많이 필요로 함.

---
#### 참고

개발토끼님 강좌