# Slack 사용 간단 정리

#### Slack 유저 토큰 생성

`https://api.slack.com/custom-integrations/legacy-tokens`에 접속한다.

Create token을 눌러 API에서 사용할 Token을 생성한다.

유저 토큰을 직접 이용하기 보다는 Incoming Webhook을 사용해서 WebHook URL을 이용한 Slack이용이 보안에 더 좋다.

#### 현재 본인의 팀 Slack 채널에서 사용중인 설정 확인하기

`https://testteam.slack.com/home`

팀에서 Slack을 사용한다면 팀 Slack 주소에 /home을 붙여서 팀 슬랙채널의 home으로 이동할 수 있는데 이 페이지에서 각종 설정이 가능하다.

좌측 메뉴에 `Configure apps`이라는 메뉴가 있는데 이 메뉴를 클릭하면 Slack에 통합된 설정들을 볼 수 있다.

Jenkins CI는 Jenkins Webhook에 사용된다.

Incoming Webhook는 Slack 팀 채널에 메시지를 보낼 수 있다.

생성한 토큰을 이용하면 Incoming Webhook에서 사용가능한 WebHook URL을 얻을 수 있다.

Slack 이용에 참고하면 좋은 글 : http://labs.brandi.co.kr/2019/01/30/kwakjs.html