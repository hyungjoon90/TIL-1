# React Native와 Flutter를 고민하는 개발자분들에게 (이상훈, Loplat)

같은 서비스를 Android, IOS 멀티 플랫폼을 지원하기 위함.

하이브리드 플랫폼의 경우 하나의 코드로 여러 플랫폼에 대응할 수 있다는 장점이 있음.

그 중, React Native, Flutter\
Web View가 아닌 Native Platform이기 때문에 N급 성능이 나옴.

React Native
- 8.1k 10.12 기준 React Native이 더 인기가 많음
- 리엑트 컴포넌트가 브릿지를 네이티브 컴포넌트에 연결
- UI면에서 플랫폼, SDK에 대한 종속성이 있음
- 일부 컴포넌트의 경우에는 안드로이드, IOS 각각에만 존재하기 때문에 플랫폼별 각각 다르게 구현해야 할경우가 있음.

Flutter
- 7.5k
- Skia 기반의 자체 UI엔진이 모든 위젯을 직접 그림
- UI면에서 플랫폼, SDK에 대한 종속성이 없음
- 각각의 스타일을 유지하기 위해서는어쩔 수 없이 분기처리 해야함 (안드로이드 스타일, IOS 스타일)

#### 성능 (갤럭시 S10 기준 카운트 늘어나는 + 버튼 눌러서, 2분간 실행 1초에 2번씩 누름 기준)
Native 안정\
Flutter, React Native은 가끔씩 CPU가 높이 올라감. 그런 경우들이 있음.\
React Native은 평균 20%, flutter는 평균 10%
평가 : React Native이 CPU를 더 씀\
Pixel 폰에서 테스트 결과도 React Native이 더 CPU를 쓴다.

C++ 엔진으로 그리는 것 vs 브릿지를 한번 더 통과해야하는 리엑트컴포넌트\
Flutter는 네이티브보다 빠르다.\
React Native은 네이티브급 빠름이다.

#### App Size (Hello world 기준)
안드로이드 타겟\
플루터는 4.5mb, React Native은 7.1mb

IOS 타겟은 반대상황\
플루터 52mb, React Native은 24mb

빌드 도구 버전에 따라 크기 차이가 날 수 있음

#### 개발자 측면
Flutter, React Native 둘 다 지원\
지원하는 IDE는 많음\
Hot Reload는 변경된 순간 APP재시작 없이 반영

JS는 익숙하지만, Dart는 생소, Dart는 구글에서 만든 언어. 인터프리터언어가 아닌 컴파일 언어\
UI를 클래스 기반으로 들여쓰기 형태로 짬\
React Native은 JSX 기반

개발자 측면에서 UI를 짤 때 Dart는 불편하다.\
필자 개인의견 : HTMl 처럼 태그기반이엇으면 괜찬앗을 것 같다.

#### 아키텍처

리엑트 네이티브 Redux\
데이터가 아래처럼 한 방향으로만 흐르기 때문에 관리하기가 좋고,
함수형 프로그래밍에 최적화, Store에서 상태를 저장하여 UI와 연결하여 사용

플루터 BLoC\
페이지당 BLoC 하나씩 하는게 정석, Stream형태로 구성되어 있다.

아키텍처는 설계 나름이지만 보통 위처럼 사용을 많이 한다고 한다.

#### 3rd-party Libraries
React Native > Flutter\
Flutter는 구글 공식 라이브러리들이 정식 버전이 아닌 pre 버전들이 많음.\
Flutter는 플러그인이 많이 존재하지 않음.\
React Native은 나온지 오래됬기 떄문에 라이브러리들이 많고 관리도 나름 잘되는 편이다.

#### 생태계 EcoSystem
React Native은 문서가 잘되어 있고 래퍼런스도 많음. 예제가 많음\
Flutter는 문서가 은근 되어있지만, 아직 나온지 얼마 되지 않아 래퍼런스가 부족함. 예제가 아직 부족함(추가되는 중으로 보임)\
Flutter는 First-Party 라이브러리의 버전들이 아직 0점대임.\
실제 서비스되어 있는 앱들이 React Native이 더 많다.

지금은 React Native을 추천하는 바이지만, 나중에 Flutter의 생태계가 발전한다면 경쟁하는 프레임워크가 될 것 같다.\
발표자분은 React Native을 선택할 것 같다.
