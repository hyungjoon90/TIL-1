# MobX

```
yarn add mobx mobx-react
yarn add @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators

  "babel": {
    "presets": [
      "react-app"
    ],
    "plugins": [
        ["@babel/plugin-proposal-decorators", { "legacy": true}],
        ["@babel/plugin-proposal-class-properties", { "loose": true}]
    ]
  }
```

중요! : 위의 바벨 설정을 하려면 yarn eject를 해야 한다. 그렇지 않으면 오류가 난다.

데코레이터 적용시 오류가 난다면,

수정한 내용을 commit 한 이후에, yarn eject !

yarn eject를 하고 나면 package.json 파일이 풀어져서

그 후에 바벨 옵션을 바꿔주고 추가로 플러그인을 설치하면 decorator 적용 가능

빌드는 되는데 vs code에서 빨간 줄 뜨는 부분은
vs code에서 File-Preferences들어가서 experimental Decorators 체크해주면 해결됨.

---

```yarn add mobx-react-devtools```

필수적인 것은 아니지만 있으면 우리가 어떤 값을 바꿧을 때, 어떠한 컴포넌들이 영향을 받고
업데이트는 얼마나 걸리고, 어떠한 변화가 일어났ㄴㄴ지에 대한 세부적인 정보를 볼 수 있게 해줌.

설치한 다음 App.js 에 적용
```
import DevTools from 'mobx-react-devtools';
{process.env.NODE_ENV === 'development' && <DevTools />}
```
---

스토어들끼리 관계를 형성하려면 RootStore를 만들어주면됨.
```
import RootStore from './stores'
const root = new RootStore();
<Provider {...root}>
```

---

MobX 의 리액트 컴포넌트 최적화 3가지 규칙
1. 리스트를 렌더링 할 땐, 컴포넌트에 리스트 관련 데이터만 props 로 넣자
리스트가 렌더링 될 때는 성능에 대해서 신경을 써주셔야 하는데요,
리스트 컴포넌트에 리스트 관련 props 만 넣는것을 권장합니다

2. 세부참조 (dereference)는 최대한 늦게하자
여기서 세부 참조 (혹은 역참조) 란, 우리가 특정 객체의 내부의 값을 조회하는것을 말합니다.

3. 함수는 미리 바인딩하고, 파라미터는 내부에서 넣어주기
컴포넌트에 함수를 전달해 줄 때에는 미리 바인딩 하는것이 좋고,
파라미터가 유동적일땐 파라미터를 넣는 작업을 컴포넌트 밖이 아니라 안에서 하는것이 좋습니다.


