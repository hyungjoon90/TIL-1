# CodeFirst 비활성화

CodeFirst 로 시작을 한 프로젝트가 있었는데 중간에 그만둔 프로젝트가 있었다.
Migrations 폴더를 삭제하고 DB도 삭제하였지만, DB와 커넥션을 하는 부분을 디버깅해보니 CodeFirst가 비활성화된 줄 알았지만,
계속해서 연결하려는 DB의 __MigrationHistory 테이블을 찾고 있었고 테이블이 존재하지 않았기 때문에 조회에 실패했다고 오류가 뜨면서 서비스가 시작되고 있었다.

이 부분을 해결하려면 CodeFirst를 완전히 비활성화시켜야 한다.

1. 프로젝트 내의 Migrations 폴더를 완전히 삭제한다.
2. Database.SetInitializer<DatabaseContext>(null); DatabaseContext 이니셜 라이저 안에 추가한다.
3. DB내의 __MigrationHistory 테이블을 삭제한다.
4. 빌드하고 실행, 그러면 해결!

위 순서에서 2번을 해주지 않았기 때문에 완전히 비활성화되지 않고 알아차리기 힘들지만, 서비스를 시작할 때, __MigrationHistory 테이블을 조회하면서 오류가 계속해서 발생한 것이다. 심지어 이 오류는 exception을 날리지 않기 때문에 DB 커넥션 부분이나 DB 조회를 디버깅하지 않는다면 알아차리기 힘들다.

---
### 참고
https://stackoverflow.com/questions/14654055/how-can-i-disable-code-first-migrations