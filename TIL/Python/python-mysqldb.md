# Python MySQLdb

#### MySQLdb

최근 작업한 크롤러에서는 pymysql을 사용해서 작업을 했다. pymysql이 다른 라이브러리들 중 가장 설치가 간단했기 때문이다.

Python에서 mysql을 연결하기 위한 라이브러리는 다양하다. 그 중 하나이다.

그런데 인수인계를 받은 프로젝트 중에서 MySQLdb를 사용하고 있어서 라이브러리를 설치했어야 하는데 Python3에서 설치가 되지 않았다.

```python
import MySQLdb
```

Python에서 MySQLdb 설치가 안될경우, [여기](https://www.lfd.uci.edu/~gohlke/pythonlibs/#mysqlclient) 들어가서 설치된 파이썬 버전에 맞는 것을 mysqlclient를 다운받아서 해당 경로로 `pip install 경로`로 설치하면 된다.

#### Python3에서 MySQLdb

python3.x에 대한 MySQLdb가 없다.

PyMySQL은 Python에서 MySQL 데이터베이스 서버에 연결하기위한 인터페이스.
Python Database API v2.0을 구현하고 순수 Python MySQL 클라이언트 라이브러리를 포함한다.
PyMySQL의 목표는 MySQLdb를 대체 하려는 것이다.

설치 pip install pymysql

```
#!/usr/bin/python3

import pymysql
```

Python3에서 MySQLdb사용하려면 두 가지 방법이 있다. 첫째는 MySQLdb1의 포크인 mysqlclient를 사용하는 것이다.

mysqlclient의 소개 문구이다.

```
이것은 MySQLdb1 의 포크입니다 .
이 프로젝트에는 Python 3 지원 및 버그 수정이 추가되었습니다. 배포가 setuptools로 다시 병합 된 것처럼이 포크가 MySQLdb1로 다시 병합되기를 바랍니다.
```

둘째는 PyMySQL을 설치 한 다음 PyMySQL을 사용하여 MySQLdb를 사용하는 방법도 있다.

pymyslq을 설치한 다음, MySQLdb를 사용하는 방법
```python
import pymysql
pymysql.install_as_MySQLdb()
import MySQLdb

db= MySQLdb.connect("hostname", "username", "password", "database_name")
```

결론

빠른 속도를 위해서는 mysqlclient를 사용하는 것이 좋을 듯 하다. Django 공식 문서에서는 mysqlclient를 사용하는 것을 권장한다.

```
MySQL Connector/Python: 4.554934978485107 [sec]
mysqlclient: 0.8555710315704346 [sec]
PyMySQL: 5.129631996154785 [sec]
```

mysqlclient 가 확실히 빠르다.

#### Python 3.4에서 MySQL 벤치마킹 자료

https://gist.github.com/methane/90ec97dda7fa9c7c4ef1

---
#### 참고

https://victorydntmd.tistory.com/275

https://stackoverflow.com/questions/23376103/python-3-4-0-with-mysql-database

https://pypi.org/project/mysqlclient/

https://raspberrypi.stackexchange.com/questions/78215/how-to-connect-mysqldb-in-python-3

http://www.learningaboutelectronics.com/Articles/How-to-connect-to-a-MySQL-database-in-Python.php