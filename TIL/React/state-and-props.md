# React에서 State와 Props

state 라는 개념은 상태를 객체화하는 개념이 있고
이 state가 변경될 때마다, React 내부에서는 전체 dom 을 새로 그린다고 보면 된다.
render() 라는 메서드를 호출하는것이고 새로운 state 를 가져와서 새롭게 뿌려주는
깊게 들어가서 설명하자면 버츄얼dom 실제로 변경된 부분만 변경한다.

setState를 통해서 state를 업데이트. state가 변경되면 렌더링이 다시된다는 개념을 기억하자.

props는 보통 자식 컴포넌트에 값을 내려줄때.
애가 렌더링을 그릴때 카운터프린터라는 자식컴포넌트가 포함된 것이고 이 컴포넌트는 카운터라는 props를 받게되어있다.
props는 컴포넌트간의 의사소통 또는 통신을 하기 위한 수단이다.

React에서 가장 기본적인 개념은 State와 Props라고 생각합니다.

여기서 더 복잡한 웹은 상태관리 라이브러리인 redux나 mobx를 사용하는 것이 편합니다.

prop-types 를 통해서 개발모드에서 props 의 타입 체크 가능
개발모드에서 에러를 확인하기 위한 용도. 프로덕션 환경에서는 뜨지 않는다.
타입이 다른걸 넘겨주면 ui는 뜨지만 console에 warning이 뜸.

componentDidMount는
서버에서 restful api를 제공해주고
componentdidmount 시점에 ajax 콜을 해서
반환된 결과를 가지고 자기 this.setState를
해서 자기 state를 바꿔주고 
render를 다시 타게 되고 화면상에 뿌려주는
그런식으로 많이 쓴다.

shouldComponentUpdate()는 최적화 할 때 많이 사용한다.


