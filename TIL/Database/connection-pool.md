# Connection Pool

- 동시 접속자가 가질 수 있는 Connection을 하나로 모아놓고 관리한다는 개념
- DB와 연결된 Connection을 미리 만들어서 Pool 속에 저장해 두고 있다가 필요할 때 Connection을 Pool에서 쓰고 다시 Pool 반환하는 기법

> Pool 속에 미리 생성되어 있는 Connection을 가져다가 사용, 사용이 끝나면 Connection을 Pool에 반환

#### 실제 사용 환경에서 보는 Connection Pool의 필요성

어떤 유저가 게시판 웹에서 로그인을 해서 게시글을 작성하는 과정을 살펴본다.

01. 클라이언트가 서버로 로그인 데이터를 전달
02. DB와 커넥션을 맺은 후, DB에 회원 데이터가 있는 지 조회
03. DB와 커넥션을 끊고, 비즈니스 로직 처리

01. 클라이언트가 서버로 게시글 데이터를 전달
02. 비즈니스 로직 처리
03. DB와 커넥션을 맺은 후, DB에 게시글 데이터를 저장
04. DB와 커넥션을 끊고, 비즈니스 로직 처리
05. 클라이언트에 응답

한명의 접속자로 인해서 짧은 시간에 여러번 DB커넥션에 맺고 끊음이 이루어 진다. 만약 접속자가 많다면 수많은 DB커녁센 맺고 끊는 과정이 반복될 것이다.
이런 상황으로 생기는 오버헤드를 방지하기 위해서 미리 Connection 객체를 생성하고 해당 Connection 객체를 관리하는 것을 의미한다.
Connection Pool에 DB와 연결해 놓은 객체를 두고 필요할 때마다 Connection Pool에서 빌려오는 것이다.

예를 들어, 위에 로그인을 할 때 사용한 Connection이 Pool에 저장되어 있기 때문에 두 번째 게시글을 작성하는 과정에서 불필요한 작업(커넥샌 생성, 끊기)가 사라지므로
성능 향상을 기대할 수 있다.

그리고 연결이 끝나면 다시 Pool에 반환한다.

Coonection Pool을 크게 해놓으면 그 만큼 메모리 소모도 클것이고, 적게 해놓으면 Connection이 많이 발생할 경우 대기시간이 발생할 것이다.
웹 사이트 동시 접속자 수 등 서버 부하에 따라 크기를 조정해야 한다.

#### 특징
- Pool속에 미리 Connection이 생성되어 있기 때문에 Connection을 생성하는 데 드는 연결 시간이 소비되지 않는다.
- Connection을 계속해서 재사용하기 때문에 생성되는 Connection 수가 많지 않다.
- Connection Pool을 사용하면 Connection을 생성하고 닫는 시간이 소모되지 않기 때문에 그만큼 어플리케이션의 실행 속도가 빨라지며
한 번에 생성될 수 있는 Connection 수를 제어하기 때문에 동시 접속자 수가 몰려도 웹 어플리케이션이 쉽게 다운되지 않는다.
- Pool에 남아있는 Connection이 없는 경우라면 해당 클라이언트는 대기 상태로 전환시키고, Connection이 다시 Pool에 들어오면
대기 상태에 있는 클라이언트에게 순서대로 제공한다.

---
#### 참고

https://brownbears.tistory.com/289

https://victorydntmd.tistory.com/42
