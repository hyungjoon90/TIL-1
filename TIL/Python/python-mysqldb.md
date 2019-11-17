# Python MySQLdb 라이브러리 설치 관련

최근 작업한 크롤러에서는 pymysql을 사용해서 작업을 했다. pymysql이 다른 라이브러리들 중 가장 설치가 간단했기 때문이다.

Python에서 mysql을 연결하기 위한 라이브러리는 다양하다. 그 중 하나이다.

그런데 인수인계를 받은 프로젝트 중에서 MySQLdb를 사용하고 있어서 라이브러리를 설치했어야 하는데 Python3에서  설치가 되지 않았다.

```python
import MySQLdb
```

Python에서 MySQLdb  설치가 안될경우, [여기](https://www.lfd.uci.edu/~gohlke/pythonlibs/#mysqlclient) 들어가서 설치된 파이썬 버전에 맞는 것을 mysqlclient를 다운받아서 해당 경로로 `pip install 경로`로 설치하면 된다.

그냥 pymysql 이나 다른 라이브러리를 쓰는게 속편할 것 같다.

---
#### 참고

https://victorydntmd.tistory.com/275