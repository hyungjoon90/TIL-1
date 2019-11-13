# Visual Studio Code에서 Python Black, Pylint 적용

Python 3.3버전 이후 기본 모듈에 포함된 venv를 이용해서 독립된 가상 환경을 만든다. 가상 환경을 만드는 이유는 파이썬에서는 하나의 라이브러리에 대해서 하나의 버전만 설치가 가능한데, 여러 개의 프로젝트를 진행하다 보면 이는 문제가 되고, 작업을 바꿀 때마다 재설치를 할 수도 있다. 이런 문제를 해결하기 위해서 독립된 가상환경을 제공한다.

필자는 Flask로 프로젝트를 진행하여서 가상 환경의 이름을 Flask로 했다. 아래에서는 sample이기 때문에 test로 설정 후 진행을 해보도록 한다.

https://suwoni-codelab.com/python%20%EA%B8%B0%EB%B3%B8/2018/03/21/Python-Basic-Virtual-Environment/

코드 컨벤션을 위한 Python의 black이라는 도구와 Pylint를 이용한 환경을 구성해보도록 한다. Nuxt.js를 사용한 프로젝트에서도 그렇지만 코드 컨벤션은 여러 명이 협업을 하는데 필수라고 생각하기 때문에 혼자 진행하는 프로젝트더라도 뒷사람을 위해서 정하고 작업을 시작하는 편이다.

Nuxt.js 진행 당시 자동으로 포매터 및 검사를 해주었기 때문에 Python에서도 이와 비슷한 도구가 있을 것이라고 생각해서 black을 찾게 되었다. 오류 검사를 위한 Pylint를 사용하였다. 빌드나 컴파일 시 에러외에 '추가'로 자잘한 오류 검사를 할 수 있는 도구를 Lint라고 한다

https://velog.io/@city7310/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%BD%94%EB%93%9C-%ED%8F%AC%EB%A7%A4%ED%84%B0-%EC%9D%B4%EC%95%BC%EA%B8%B0-5wjxdei9iv

가장 먼저 Visual Studio Code의 Extensions에서 python을 검색해서 Python, Python for VSCode를 설치한다.

```
python -m venv test
```

test라는 이름의 python 가상 환경이 설치된다.
가상 환경 터미널을 VS Code에서 열려면 Ctrl + Shift + ` 를 하면 된다.

Ctrl + P를 이용해서 settings.json으로 들어간다.

```json
{
  "python.pythonPath": "test\\Scripts\\python.exe"
}
```

기존 위의 옵션에서 아래의 옵션들을 추가해주도록 한다.

```json
{
    "python.pythonPath": "test\\Scripts\\python.exe",
    "python.formatting.provider": "black",
    "python.formatting.blackArgs": [
        "--line-length",
        "100"
    ],
    "editor.formatOnSave": true
}
```
formatOnSave는 저장시에 자동으로 format을 적용한다는 옵션이다. 

그러면 python 가상 환경에서 black이 적용되었다.

https://github.com/psf/black

에서 그 외 자세한 옵션들을 확인하실 수 있다.

먼저 python 가상 환경에서 `pip install pylint`로 Pylint 설치한다.

Pylint의 경우 아래의 옵션들이 settings.json에 자동으로 생긴다.

```json
    "python.linting.pylintEnabled": true,
    "python.linting.enabled": true
```

그 위에 아래 옵션을 추가해서 저장 시에 자동으로 검사하도록 설정한다.
Visual Studio Code의 linting setting을 확인하니 lintOnSave의 default 값은 true라고 한다.

```json
"python.linting.lintOnSave": true,
```

저는 가상환경을 만드는데 venv, black과 Pylint를 사용하였지만 각자 원하는 환경을 이용해서 본인에 맞는 또는 팀에 맞는 개발환경을 구축하면 될 것 같다.

https://code.visualstudio.com/docs/python/editing
https://code.visualstudio.com/docs/python/linting