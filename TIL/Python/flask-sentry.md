# Flask에 Sentry 적용하기

Sentry Python SDK를 설치한다.

```
pip install sentry-sdk[flask]
```

```python
import sentry_sdk
from sentry_sdk.integrations.flask import FlaskIntegration

sentry_sdk.init(
    dsn="https://726d3393be5143bb9b09aa61c4fab969@sentry.io/1275641",
    integrations=[FlaskIntegration()]
)

app = Flask(__name__)
```

Sentry DSN은 Project Settings에서 Client Keys[DSN] 메뉴에 있다.
그리고 General Settings에서 Platform을 설정해서 Flask로 설정할 수 있다.
SDK를 구성하려면 앱이 초기화되기 전 또는 후에 통합으로 SDK를 초기화해야 한다.
`environment`를 이용해서 센트리 프로젝트의 environment도 설정할 수 있다.

```python
@app.route('/debug-sentry')
def trigger_error():
    division_by_zero = 1 / 0
```

오류를 유발하는 경로를 만들어 Sentry 설치를 쉽게 확인 가능하다.
이 경로를 방문하면 Sentry에서 캡처 한 오류가 발생한다.

---
#### 참고

https://docs.sentry.io/platforms/python/flask/
