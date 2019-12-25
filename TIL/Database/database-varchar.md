# DataBase별 데이터 선언 차이

**MySQL, MariaDB**\
varchar(10) : 한글10자, 영문,숫자 10자

**MS-SQL**\
varchar(10) : 한글 5자, 영문,숫자 10자\
nvarchar(10) : 한글 10자, 영문, 숫자 10자

**Oracle**\
varchar2(10) : 한글 3자, 영문,숫자 10자\
nvarchar2(10) : 한글 10자, 영문,숫자 10자

**char와 varchar(varchar2) 비교**

문자의 경우 char와 varchar의 차이는 저장 영역과 문자열의 비교 방법이다.

char에서는 문자열 비교를 할 떄, 공백을 채워서 비교를 하기 떄문에 'AA' = 'AA '은 실제로 'AA      ' = 'AA      '이 되어 같다는 결과가 나온다.

반면, varchar에서는 공백도 하나의 문자로 취급하므로 끝에 공백이 들어가면 다른 문자로 판단한다.

이름, 주소 등의 길이가 변할 수  있는 값은 varchar를 사용하는 것을 추천하고, 사번, 주민등록번호 같이 길이가 일정한 데이터는 char를 사용하는 것이 좋다.

---
#### 참고

https://kimsi1027.tistory.com/2

https://hyeonstorage.tistory.com/290