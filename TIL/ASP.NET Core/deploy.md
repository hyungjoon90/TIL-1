# .NET Core InProcess와 OutOfProcess

컴퓨터에 이전 버전의 SDK가 다운되어 있다가 최근의 SDK를 다운받은 이후에 .NET Core를 배포하려고 하니 오류가 생겼었다. 이유를 아직까지는 잘 모르겠지만 .NET Core 버전의 종속성 때문에 생긴것이라고 추측하고 있다. 빌드하려는 프레임워크의 버전과 로컬에 다운되어 있는 프레임워크 버전의 충돌이라고 생각한다.

IIS에 InProcess로 배포하는 올바른 방법은 아래와 같다.

```dotnet nuget locals all --clear```
nuget 리소스를 지웁니다.

```dotnet publish --output:bin/framework_dependent```
해당 dll을 IIS예 배포합니다.

그리고 .NET Core의 배포에는 2.2이후로 InProcess와 OutOfProcess로 나누어 진다.(ANCM2을 사용) 이 두가지 방식의 차이점은 InProcess는 IIS가 서버가 되는 것이고 OutOfProcess는 Kestrel이 Edge Server로 사용되거나 역방향 프록시로 IIS, Nginx등이 역활을 하고 Kestrel을 Edge Server로 사용한다는 점이다.

2.2이 이후로 생긴 InProcess가 역방향 프록시를 이용한 OutOfProcess보다 처리 성능이 더 좋다. 다만, Kestrel을 직접 호스팅한 서버의 경우에는 성능이 비슷한 것 같았다.

.NET Core로 만들어진 웹의 경우 F12의 개발자 도구에서 NetWork=의 Response Headers를 확인해서 Server에서 IIS 인지 Kestrel인지 확인을 통해 InProcess와 OutOfProcess를 구별할 수 있다.

```
<aspNetCore processPath = "dotnet"arguments = ". \ InProcApp.dll"stdoutLogEnabled = "false"stdoutLogFile = ". \ logs \ stdout"hostingModel = "InProcess"/>
```

```
<aspNetCore processPath = "dotnet"arguments = ". \ InProcApp.dll"stdoutLogEnabled = "false"stdoutLogFile = ". \ logs \ stdout"hostingModel = "OutOfProcess"/>
```

hostingModel을 변경함으로서 InProcess와 OutOfProcess방식으로 배포할 수 있으며, 또는 csproj에서 설정이 가능하다. 그리고 디버그에서도 디버그 속성으로 설정이 가능하며 Kestrel을 Edge Server로도 디버그가 가능하다.

Windows환경에서는 IIS를 서버로 사용하는 것이 처리량에서와 보안에서 더 좋은 것 같다.

---
#### 참고

https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/

https://appdevmusings.com/host-your-asp-net-core-2-2-web-app-with-iis-in-process-and-out-of-process-hosting-model-and-deploy-to-docker-windows-containers

https://codewala.net/2019/02/05/hosting-asp-net-core-applications-on-iis-in-process-hosting/


