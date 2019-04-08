# Visual Studio Code 풀스택 개발하기

### 멀티플 프로젝트

1단계 : 모든 프로젝트를 저장할 새 디렉토리(솔루션 디렉토리)를 만듭니다.

```mkdir vs-code-multiple-projects```

2단계 : 디렉토리를 변경하고 .net core cli 를 이용해서 새로운 첫 프로젝트를 추가합니다.

```cd vs-code-multiple-projects```

```dotnet new mvc -o Jhyeok.Web```

3단계 : 이제 두 번째와 세 번째 프로젝트를 만듭니다.

```dotnet new classlib -o Jhyeok.Data```

```dotnet new classlib -o Jhyeok.Domain```

4단계 : 프로젝트를 열고 참조를 추가합니다.

```cd Jhyeok.Web```

```dotnet add reference ../Jhyeok.Data/Jhyeok.Data.csproj```

```dotnet add reference ../Jhyeok.Domain/Jhyeok.Domain.csproj```

프로젝트에 '..\Jhyeok.Domain\Jhyeok.Domain.csproj' 참조가 추가되었습니다.
이런식으로 참조추가가 완료됩니다. 웹 csproj 파일을 보면 참조가 추가된 것을 확인할 수 있습니다.

실행은 각 프로젝트에 들어가서 dotnet run 을 해서 실행, dotent build 를 해서 빌드를 하거나 vs-code-multiple-projects 라는 경로에서 dotnet build Jhyeok.Data 를 해서 해당 Jhyeok.Data 를 빌드할 수 있습니다.

### .net core 개발 인증서 오류와 watch 모드로 개발하기

Visual Studio Code 로 개발시에 cli에서 dotnet run을 했는데 인증서 오류가 뜬다면

```dotnet dev-certs https --trust```

한 후에 Visual Studio Code를 재시작 후에 다시 dotnet run 을 하면 문제가 해결된다.

.net core 2에서는 dotnet watch 가 cli에 포함되어있다.

```dotnew watch run``` 으로 실행시키고
서버단 소스를 변경하면

```
watch : Exited
watch : File changed: D:\Dev\vscode-watch\Watch.Web\Controllers\HomeController.cs
watch : Started
```

이런식으로 변경이 완료되고 변경된 코드가 적용된다.

단점은 보고 있는 브라우저에서 F5 키를 눌러서 새로고침을 한번 해주어야 한다.