# Flask에서 global 변수 사용 및 Swagger 결합, 후행슬래시

#### global 변수 사용

flask 폴더 구조를 변경 이후 global로 선언한 변수를 import 하지 못하고 있다.

global로 선언할 필요 없이, app/__init__ 에서 아래 처럼 쓰면 다른 폴더 구조의 .py에서 사용할 수 있다.

`app/__init__.py`

```python
graph = tf.compat.v1.get_default_graph()
```

`app/widget/service.py`

```python
from app import graph
```

아래 함수의 입출력에서 `->` 의미는 함수의 반환 타입을 선언한다.

```python
def create(new_attrs: WidgetInterface) -> Widget:
```

매개변수 new_attrs의 타입을 WidgetInterface로 선언한 것이다. 매개변수 Python3.6부터 정적타입을 쓸 수 있도록 도입되었다.

#### Swagger 결합

flask_restplus, flask_accepts를 이용해서 swagger가 결합된 flask api를 쉽게 만들 수 있다. flask로 프로젝트 하는데 참고한 좋은 글이다.

#### Flask에서 후행슬래시 라우팅관련(flask_restplus 환경)

@api.route("/")로 라우팅을 했을 경우에 Flask는 보통 후행 슬래시없이 URL에 엑세스하면 Flask가 후행 슬래시가 있는 URL로 리디렉션한다.

```
GET http://127.0.0.1:5000/api/predict/
GET http://127.0.0.1:5000/api/predict
```

위 두개의 요청다 후행슬래시가 있는 Predict 클래스의 @api.route("/")로 리디렉션 된다.

그런데 디버그가 켜져있는 상태에서 Get요청이 아닌 Post요청을 보내면 아래와 같은 안내메시지가 뜨면서 리디렉션해주지 않는다.

```
flask.debughelpers.FormDataRoutingRedirect: b'A request was sent to this URL (http://127.0.0.1:5000/api/predict) but a redirect was issued automatically by the routing system to "http://127.0.0.1:5000/api/predict/".  The URL was defined with a trailing slash so Flask will automatically redirect to the URL with the trailing slash if it was accessed without one.  Make sure to directly send your POST-request to this URL since we can\'t make browsers or
HTTP clients redirect with form data reliably or without user interaction.\n\nNote: this exception is only raised in debug mode'
```

해당 문제는 디버그를 끄면 Post요청도 정상적으로 리디렉션이 된다.

위 상황을 몰랐을 때 디버그를 킨 상태로 테스트를 해서 후행슬래시가 없이 프론트엔드에서 axios 요청을 보냈을 때, 오류가 나서 후행슬래시를 붙이고 요청을 보내는 멍청한 짓을 했다.

---
#### 참고

http://alanpryorjr.com/2019-05-20-flask-api-example/

https://velog.io/@wody/Flask-Tutorial