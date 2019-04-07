# Visual Studio Code 풀스택 개발하기

#### 멀티플 프로젝트

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
