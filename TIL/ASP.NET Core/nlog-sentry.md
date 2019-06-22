# ASP.NET Core NLog, Sentry 적용

ASP.NET Core에서 오류처리와 오류 커스텀 페이지를 보여주는 방법입니다.
ASP.NET MVC 5에서와는 다르게, ASP.NET Core 2.2에서는 더 간단하게 할 수 있습니다.

먼저, 오류내용을 basedir에 파일로 쌓기 위한 NLog 입니다.

Nuget에서 NLog.Web.AspNetCore를 다운받습니다.
https://github.com/NLog/NLog.Web
https://www.nuget.org/packages/NLog.Web.AspNetCore/4.8.3

프로그램 진입점인 Program 클래스의 Main에다가 아래 코드를 추가해줍니다.

```
public static void Main(string[] args)
{
    var logger = NLogBuilder.ConfigureNLog("nlog.config").GetCurrentClassLogger();
    try
    {
        logger.Debug("Init Main");
        CreateWebHostBuilder(args).Build().Run();
    }
    catch (Exception ex)
    {
        logger.Error(ex, "Stopped program because of exception");
        throw;
    }
    finally
    {
        // Ensure to flush and stop internal timers/threads before application-exit (Avoid segmentation fault on Linux)
        NLog.LogManager.Shutdown();
    }
}
```

그리고 nlog.config을 만들어줍니다.

```
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >
  <extensions>
    <add assembly="NLog.Web.AspNetCore"/>
  </extensions>

  <targets>
    <target name="file" type="File"  fileName="${basedir}/Logs/${level}/${shortdate}.txt" layout="${longdate}|${event-properties:item=EventId_Id}|${uppercase:${level}}|${logger}|${message} ${exception:format=tostring}" />
  </targets>
  <rules>
    <logger name="*" minLevel="Error" writeTo="file" />
  </rules>
</nlog>
```

로깅할 최소 레벨을 설정이 가능한데, Trace로 하면 모든 오류를 다 쌓고, Error로 설정하면 Error 위의 레벨만 쌓습니다.
자세한 로그 레벨 정보는 https://github.com/NLog/NLog/wiki/Configuration-file#log-levels 여기서 확인이 가능합니다.

```
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .UseNLog();
```

CreateWebHostBuilder에다가 UseNLog를 해서 주입을 시켜줍니다.
이러면 NLog 설정이 끝납니다. 오류가 발생한다면 NLog.config에 정의해놓은 target과 rule대로 파일이 만들어질 것 입니다.

dotent core 초창기 버전에서는 Sentry를 쌓으려면 Sentry미들웨어를 만들고, RavenSharp.Core라는 패키지를 이용해야 합니다.
Sentry에게 오류를 보내는 클래스를 만들고 해당 클래스를 가지고 처리하는 SentryMiddleware를 만들어야 하며
오류를 보내는 클래스를 DI주입하고, SentryMiddleware는 ASP.NET Core 요청 파이프라인에 추가해야 합니다.
참고 : https://medium.com/@aevitas/logging-to-sentry-in-asp-net-core-8c84c5c592db

하지만 위의 방법은 Sentry가 .NET Core에 대한 공식지원이 없을 때의 방법이고 공식지원을 하기 시작하면서 위 방법보다는
아래의 방법을 권장합니다.
https://github.com/getsentry/sentry-dotnet

```
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .UseSentry()
    .UseNLog();
```
CreateWebHostBuilder에 UseSentry()를 넣어줍니다. UseSentry에서 람다식으로 Sentry DSN과 옵션을 추가할 수 있고, 아니면 아래 방법처럼
appsettings.json에서 DSN과 옵션을 추가할 수 있습니다.

```
"Sentry": {
  "Dsn": "https://@sentry.io/projectNumber",
  "IncludeRequestPayload": true,
  "IncludeActivityData": true
},
```

놀랍게도 이 설정만 되면 Sentry에 오류보고가 완료됩니다.

