# Jenkins 와 Slack 알람 통합

Jenkins 에서 빌드를 시작하거나, 빌드에 오류 또는 빌드가 완료되었을 때의 알람처리를 Slack에다가 할 수 있다. Jenkins 의 빌드 후 조치에서 Git Commit 을 주거나, Jira 에 코멘트를 다는 등 여러 조치가 가능하지만, Slack을 이용해서 적용했다.


1. 가장 먼저 Slack 계정과 채널을 만든다.

2. Jenkins-ci를 Slack에 설치해야 한다.

https://myspace.slack.com/services/new/jenkins-ci

위 주소에서 myspace를 적용하려는 Slack의 주소로 변경해서 들어간 후, Jenkins-ci 를 설치한 다음 구성을 추가하고. 알람을 보낼 채널을 선택한다. Post to Channel 에서 선택을 하면 된다. 채널을 선택하는 부분 아래에 Token 이 보일텐데 이 Token을 잘 기억해야 한다.

3. Jenkins 에 들어가서 Slack Notifications plugin 을 설치한다.

해당 플러그인을 설치한 이후에 Jenkins 구성에서 Slack에 나온 Setup Instructions 을 보고 따라하면 되는데, 굉장히 쉬웠다.

BaseURL만 넣어주고 Token만 넣어주면 끝이다.
그 이후에는 각 Job 구성에서 빌드 후 조치에서 Slack Notifications 을 선택해서 원하는 알람유형에 체크를 하면 된다.

나는 Build Start 와 Build Success 보다는 오류가 생겼을 때, 알람이 효율적인 것 같아서 해당 유형들만 체크하였다.

---
#### 참고
https://medium.com/appgambit/integrating-jenkins-with-slack-notifications-4f14d1ce9c7a