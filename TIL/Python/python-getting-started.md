# python with Visual Studio Code

https://code.visualstudio.com/docs/python/tutorial-flask

venv 하고 그 다음 쓰는게 환경 이름인으로 생성된다. 그 환경을 삭제하려면 디렉토리에서 path만 지우면 된다.

명령 팔레트에서 새 통합 터미널 작성 ( Ctrl + Shift +` )) : 터미널을 작성하고 활성화 스크립트를 실행하여 가상 환경을 자동으로 활성화한다.

맨 아래 프로젝트 구조 나눈 부분에서\
CLI로 실행하기 위해서는 hello_app 폴더로 들어가서, \
`$env:FLASK_APP = "webapp"`\
환경변수를 지정해준 후에, \
`python -m flask run`\
로 정상적으로 실행 가능하다.

# python with Jupyter Notebook

윈도우에서 아나콘다 설치 안하고 기존의 파이썬에서 쥬피터 사용하는 방법.

```
pip install jupyter
```

jupyter를 설치하고 난 이후에는 원하는 디렉토리에서

jupyter notebook을 실행하면 해당 디렉토리가 연동되서 `http://localhost:8888/tree`가 열린다.

그 이후 쥬피터 노트북을 사용할 수 있다.