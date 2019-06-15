# npm update

npm update를 입력하면 설치된 의존성 모듈들을 전부 업데이트 한다.

```npm update gatsby```

특정한 모듈만 업데이트를 하려면 위처럼 npm update 모듈명

```npm outdated```

npm 버전들을 관리할 수 있다.

많은 의존성 모듈에서 새로운 버전이 나오는 모듈을 일일이 확인하기 어려운데 npm outdated 명령어를 사용하면 의존성 모듈 중에 업데이트가 필요한 모듈을 확인할 수 있다.

npm outdated를 실행하면 업데이트가 필요한 모듈만 정리되어 나온다.

"Current"는 현재 설치된 버전이고 "Wanted"는 package.json에 지정한 버전 범위로 설치되는 최대 범위를 의미한다.

즉, npm update를 실행하면 설치되는 버전이다. "Latest"는 모듈의 최신 버전이다. 위 화면에서는 "Wanted"와 "Latest"가 같은 모듈이 빨간색으로 표시되었고 "Latest"가 "Wanted"보다 높은 모듈은 구별할 수 있게 노란색으로 표시되었다.

---
#### 참고

https://blog.outsider.ne.kr/1228