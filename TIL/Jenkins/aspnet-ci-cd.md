# Jenkins와 MSBuild를 이용한 ASP.NET MVC CI/CD

#### Git 연결

젠킨스에서 빌드유발 탭에서 Build when a change is pushed to GitLab를 체크해서
push event를 캐치할 수 있다.

체크해서 받은 CI Service URL을 gitlab의 Webhook에 등록해주면 젠킨스의 소스 코드 관리 탭에서 설정해놓은 git repository에 push가 발생하면 젠킨스가 자동으로 빌드가 실행된다.

Webhook을 등록 후에 Test 버튼을 눌러서 Test가 가능한데 오류가 날 경우 대부분 네트워크/방화벽 문제이다. 나도 오류가 나서 확인해보니 AWS의 공인 IP가 막혀있었고, 내부 IP로 하니 잘 작동하였다.

#### MSBuild 사용

MSBuild 15를 사용했는데, 비쥬얼 스튜디오 2017로 만들어진 프로젝트를 빌드하기 위함이다. MS에서 빌드 툴을 다운로드받아서 젠킨스 서버에서 실행해서 다운로드를 받아주면 되며 다운로드 받을 때 웹 개발빌드 도구를 체크하고(배포하려는 프로젝트의 닷넷버전도 체크), nuget 패키지 관리자도 체크해서 설치를 해야 한다.

#### 젠킨스에서 Visual Studio의 게시 기능을 사용

게시 프로파일의 경우 git에 추가하거나, 따로 만들어서 젠킨스의 특정 폴더에 넣어놓아야 한다.
만약 특정 폴더에 넣는 방식이면 빌드 전에 그 폴더에 들어있는 게시 프로파일을 옮겨야 하고 git에 추가된 경우는 별문제 없다.
git pull 하면서 전부 가져오기 때문이다.

빌드전에 nuget 으로 C:\nuget restore WebDeployTest.sln 가 되어야 한다.

```
/t:ReBuild
/p:DeployOnBuild=true
/p:PublishProfile=Default-profile
/p:AllowUntrustedCertificate=true
/p:Password=${PASSWORD_1}
/p:PrecompileBeforePublish=true
/p:EnableUpdateable=true 
/p:Configuration=Release
```

위의 패스워드 같은 경우
Mask passwords and regexes (and enable global passwords)
플러그인을 이용해서 PASSWORD_1 라는 변수에 비밀번호를 집어넣을 수 있다.

#### 게시 프로파일 설정 - 특정 프로파일 제외

```
<ExcludeFilesFromDeployment>
Web.config;NLog.config
</ExcludeFilesFromDeployment>
```

로 특정 파일을 제외할 수 있다.

```
<ExcludeFoldersFromDeployment>
  lib
</ExcludeFoldersFromDeployment>
```

로 특정 폴더도 제외할 수 있다.

#### 게시 프로파일 설정 - 기존에 게시된 파일 지우기

기존에 게시된 파일들을 전부 지우고 새롭게 게시할 수 있다. 그리고 기존에 게시된 파일들을 지울 때, Web.Config과 NLog.Config을 제외시킬 수 있다.
기존에 게시된 파일을 지우는 이유는 기존에는 Sample.dll이 배포되어 있었는데 이후에 배포할 때는 Sample.dll을 사용하지 않을 수도 있기 때문이다. 아래는 관련 설정이다.

```
<SkipExtraFilesOnServer>False</SkipExtraFilesOnServer>
  <ItemGroup>
    <MsDeploySkipRules Include="CustomSkipFolder">
      <ObjectName>filePath</ObjectName>
      <AbsolutePath>\\Web.config</AbsolutePath>
    </MsDeploySkipRules>
  </ItemGroup>
  <ItemGroup>
    <MsDeploySkipRules Include="CustomSkipFolder">
      <ObjectName>filePath</ObjectName>
      <AbsolutePath>\\NLog.config</AbsolutePath>
    </MsDeploySkipRules>
  </ItemGroup>
```

#### Jenkins에서 게시하면서 겪은 오류

게시하면서 생긴 오류는 Visual Studio 의 게시 기능을 사용할 경우 8172 포트를 사용하는데
젠킨스에서 Visual Studio 게시 기능을 추가해서 CI/CD 를 구현할 수 있다.
내 로컬에서 웹 게시할 때는 문제가 없었지만 젠킨스에서 게시할 경우, 게시에서 사용하는 8172 포트가 열려있지 않아서 오류가 날 수 있다.
aws 를 이용하기 때문에 aws 보안그룹에서 해당 포트에 허용을 해주어야 하며, 게시에는 공인IP를 이용해야 한다.

---
#### 참고
옛날 글이라서 다른게 좀 많지만 아래 링크를 참고했다.

http://matthewyukiuchino.com/how-to-use-jenkins-to-deploy-c-web-applications-to-iis/
