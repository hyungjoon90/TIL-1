# ASP.NET MVC EF의 콜드쿼리와 미리컴파일

EF를 이용한 MVC 웹을 처음에 배포하고 나면 처음 실행 시에는 약 5 - 10초가 걸리는데 그 이후에 그 페이지의 작업은 1초가 걸리지 않는다. 처음 작업이 오래 걸리는 이유는 콜드쿼리(cold query)라고 불리는 것을 날리고 그 이후는 웜 쿼리를 날리기 때문이다.

이 콜드쿼리는 MVC웹의 내부 데이터 구조를 메모리에 구축함으로써 시간이 걸리는 것입니다. 도메인 당 한 번 발생하기 때문에 '/Account'라는 페이지가 콜드쿼리가 발생했고 그다음 '/Notice'라는 페이지로 진입한다면 이 페이지도 콜드쿼리가 발생하게 된다. EF의 

요약하면 '처음 페이지에 진입 시 최초의 사용자는 오래 걸림, 그다음 진입 시에 다른 사용자들은 빠르다'입니다.

이 문제를 해결하려면 웹 배포 시에 미리컴파일 옵션을 키고 배포하면 됩니다. 컴파일 하고 배포하기 전에 콜드 쿼리를 날려서 내부 데이터 구조를 메모리에 구축하기 때문에 컴파일의 시간이 더 오래 걸리지만 해당 도메인의 처음 진입시에 약 5 - 10초가 걸리지 않습니다.

```
  <PropertyGroup>
    <PrecompileBeforePublish>True</PrecompileBeforePublish>
    <EnableUpdateable>True</EnableUpdateable>
  </PropertyGroup>
```

게시 프로파일에서의 설정입니다.

```
/p:PrecompileBeforePublish=true /p:EnableUpdateable=true
```

MSBuild에서의 설정입니다.

재컴파일, IIS 재시작에서의 콜드쿼리는 사라지게 했지만 유휴시간이 종료되고 그 이후는 다시 콜드쿼리가 발생할 것이다. 다시 발생하게 되는 이유는 IIS에서 특정 응용프로그램 풀이 HTTP요청이 없는 경우에는 기본적으로 20분이 지나면 자동적으로 프로세스를 종료시키기 때문이다. EF의 내부 데이터 구조를 메모리에 구축되었던 것이 날라갈 것이고 다시 구동한다면 또 콜드쿼리를 날리면서 위의 문제가 반복될 것이다. 해결책은 IIS에서 응용프로그램 풀의 고급설정에서 시작 모드를 AlwaysRunning, 유휴 시간 제한 조치를 Suspend, 미리 로드 활성화됨을 True로 하면 된다. 그리고 윈도우 서버 역활에서 응용 프로그램 초기화를 체크하면 된다.
---
#### 참고

https://codeday.me/ko/qa/20190502/439020.html

https://docs.microsoft.com/ko-kr/ef/ef6/fundamentals/performance/perf-whitepaper

https://galleryserverpro.com/eliminate-startup-delay-in-your-galleries-using-application-initialization/