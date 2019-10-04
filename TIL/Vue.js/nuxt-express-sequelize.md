# Nuxt.js에서 express와 sequelize

create-nuxt-app v2.10.1을 이용해서 만들었는데, server framework를 express로 선택해서 간단한 api의 경우 같이 구현하려고 했다.\
그런데 가이드에서 nuxt.config.js에 있는 serverMiddleware 속성이 보이지를 않았다.

조금 삽질을 했지만 그냥 기존의 server/index.js에서 app.use(nuxt.render) 전에 만들어두었던 express 설정이나 라우팅등을 호출하면 적용된다.
nuxt.render 뒤에 하면 안된다.

nuxt middleware에 표현하기 전에 express server api를 구성해야 한다.

```
npm install -s sequelize
npm install -s mysql
npm install -s mysql2

npm install -g mysql  // auto로 모델을 생성할 때 이 패키지를 전역으로 설치하지 않으면 오류가 난다.
npm install -g sequelize-cli
npm install -g sequelize-auto

sequelize-auto -o "./server/models" -d DatabaseName -h 127.0.0.1 -u root -p 3306 -x password -e mysql
```

생성 후, models/index.js에서 원하는 테이블을 가져와서 orm 해준다.

SequelizeDatabaseError: Unknown column 'createdAt' in 'field list'
오류가 떴다.\
select 문은 컬럼 전부다 제대로 하다가 createdAt, updatedAt이라는 현재 내가 만든 테이블에 없는 필드도 Select해서 생기는 오류였다.

해당 문제는 sequelize에서 select할 때, 타임스탬프 옵션이 활성화되어 있어서 createdAt, updatedAt도 같이 조회하도록 되는 것이었다.\
모델의 sequelize.define에서 timestamp를 false 해주면 해결된다.\
전체 모델에 적용하려면 sequelize의 config을 수정해주면 된다.

```javascript
{
  "development": {
    "username": "root",
    "password": "password",
    "database": "DatabaseName",
    "host": "127.0.0.1",
    "dialect": "mysql",
    "operatorAliases": false,
    "define": {
      "timestamps": false
    }
  }
}
```

테이블이름의 단수형 복수형도 모델의 define에서 직접 테이블이름을 지정하거나, db connection에서 freezeTableName 옵션을 설정해주면 된다.

---
#### 참고

https://stackoverflow.com/questions/11321635/nodejs-express-what-is-app-use

https://stackoverflow.com/questions/20386402/sequelize-unknown-column-createdat-in-field-list

