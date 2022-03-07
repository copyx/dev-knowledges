# React Native

React를 기반으로 하는 iOS, Andrdoid 크로스플랫폼 애플리케이션 프레임워크.

## 동작 원리

단순 웹뷰나 브라우저 위에서 리액트 코드가 실행되는 것이 아님. 과거에는 리액트 네이티브 코드와 네이티브 API 사이에 Bridge가 있어서 렌더링 요청을 중계해줬음. 지금은 Fabric이라는 새로운 렌더러로 바뀜.

리액트 로직을 호스트 플랫폼에 렌더링 하는 작업을 `Render pipeline`이라고 함. 이 렌더 파이프라인은 세 단계로 쪼갤 수 있음.

1. Render: 리액트는 자바스크립트로 된 [React Element Trees](https://reactnative.dev/docs/architecture-glossary#react-element-tree-and-react-element)를 생성하는 제품 로직을 실행함. 렌더러는 이 트리로부터 C++로 된 [React Shadow Tree](https://reactnative.dev/docs/architecture-glossary#react-shadow-tree-and-react-shadow-node)를 생성.
1. Commit: React Shadow Tree가 완전히 생성된 후, 렌더러가 커밋을 발생시킴. 이는 React Element Tree와 새로 생성된 React Shadow Tree를 마운트될 "다음 트리"로 격상시킴.
1. Mount: 레이아웃 계산 결과와 함께 React Shadow Tree는 [Host View Tree](https://reactnative.dev/docs/architecture-glossary#host-view-tree-and-host-view) 속으로 변환됨.

렌더 파이프라인의 각 단계는 상황마다 다른 스레드에서 발생할 수 있음. 자세한 내용은 [Threading model](https://reactnative.dev/docs/threading-model) 참조

![React native - Data flow](images/react-native-data-flow.jpeg)

- 참조: https://reactnative.dev/docs/render-pipeline

## 개발환경 설정

빌드에 필요한 도구들(Xcode, Android studio, CocoapPds, Watchman, Node 등)을 설치해야함.

- Xcode는 App Store가 아닌 애플 개발자 사이트에서 다운로드 받아서 설치할 것!!!
  - App Store에서 설치하면 진짜 너무 오래걸림.
- CocoaPods는 gem으로 설치가 안되면 brew로 설치하자.

## Rules

### Use View component instead of div element

RN에서는 div 사용 불가. 대신 View 컴포넌트를 사용하면 됨.

### All texts have to be with Text component

RN에서 사용할 모든 텍스트는 Text component로 감싸서 사용해야 함.

### StyleSheet is not CSS

RN에서 사용할 수 있는 스타일 속성은 CSS와 다름. 편하게 자동완성을 사용하려면 `StyleSheet.create({})` 추천!

## Components

Android/iOS 공통으로 사용할 수 있는 Core Component와 API, 그리고 Android, iOS 각각의 전용 컴포넌트들이 있음.

과거에는 더 많은 컴포넌트들과 API를 RN에서 직접 제공했었으나 지금은 많이 줄임. 지원 컴포넌트와 API를 늘리고 이를 유지보수하는 것보다 성능에 더 집중하며 이렇게 변함.

### 터치를 처리하는 컴포넌트들

터치했을 때 발생하는 이벤트를 캐치해 그래픽적 피드백을 주거나, 다양한 터치 방식을 처리할 수 있게 만들어주는 컴포넌트.

|           Name           |                            Description                            |
| :----------------------: | :---------------------------------------------------------------: |
|     TouchableOpacity     |           터치 시 투명도를 변경하는 것이 대표적인 동작            |
|    TouchableHighlight    |           터치 시 배경색을 변경하는 것이 대표적인 동작            |
| TouchableWithoutFeedback |            터치 시 아무 그래픽적인 피드백을 주지 않음             |
|        Pressable         | 여러가지 터치 이벤트 및 방식에 대해 세세한 설정이 가능한 컴포넌트 |

## Layout System

RN에서는 Flexbox로 레이아웃을 구성함. (`block`, `inline-block`, `grid` 이런거 없음.)

- 모든 View는 Flex 컨테이너임. CSS 처럼 `display: flex` 해줄 필요가 없음.
- `flexDirection의` 기본 값은 `column`. CSS 처럼 `row`가 아님.

다양한 화면 크기에 대응하기 위해 비율을 사용할거라면 `flex` 속성을 사용하면 됨.

## 3rd Party Components

|                           Name                           |                                                                             Description                                                                             |
| :------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| [React Native Directory](https://reactnative.directory/) |                 RN 컴포넌트들을 찾아볼 수 있는 저장소.<br/>RN팀에서 모든 컴포넌트를 만들고 유지보수하기에는 리소스가 부족해 커뮤니티에 의존하게됨.                  |
|    [Expo SDK](https://docs.expo.dev/versions/latest/)    | Expo에서 제공하는 컴포넌트와 API의 모음.<br/>Expo 앱 위에서만 동작하는 것이 아닌 순수 RN 앱에서도 사용 가능.<br/>굉장히 많은 종류의 컴포넌트와 API가 있고 안정적임. |
|     [React Navigation](https://reactnavigation.org/)     |             라우팅과 네비게이션을 위한 라이브러리.<br/>브라우저에는 있지만 RN에는 없는 화면 이동 히스토리 스택과 이를 이용한 이동 등 관리 기능을 제공함             |

## Tools

### [Expo](https://expo.dev/)

모든 리액트 앱(리액트 네이티브 포함)을 위한 프레임워크이자 플랫폼. 동일한 자바스크립트 혹은 타입스크립트 코드로 iOS, Android, Web 앱의 개발, 빌드, 배포 사이클을 빠르게 반복할 수 있도록 도와주는 리액트 네이티브와 네이티브 플랫폼들에 기반한 도구와 서비스들의 집합.

#### [Snack](https://snack.expo.dev/)

React Native를 웹 브라우저에서 개발할 수 있는 코드 에디터.

### RN 프로젝트 생성 도구

RN 개발 시 많이 사용되는 설정과 라이브러리를 프로젝트 생성과 동시에 셋팅

- [Ignite](https://github.com/infinitered/ignite)
  - 설정과 라이브러리는 물론 디렉토리 구조와 테마 등 많은 것들이 셋팅됨.
  - 단, 세부적인 네이티브 앱 설정이 힘듦. 모두 Ignite 설정에 의존해야함.
- [create-react-native-app](https://github.com/expo/create-react-native-app)
  - Facebook, Expo의 합작
  - Native 파일들(앱 관련 파일)에 접근 가능(Expo는 불가능)
  - Expo에서 제공하는 프리뷰 사용 가능
  - 과거에는 package.json의 scripts에 react-native 명령을 사용했으나, 현재는 expo를 사용함.
