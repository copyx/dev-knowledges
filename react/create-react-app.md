# create-react-app

페북에서 만든 리액트 앱 초기 셋팅 도구.

```bash
$ npx create-react-app [app name]
```

위 명령어 하나면 프로젝트 폴더가 만들어지고 그 안에 리액트 셋팅이 끝!

- npx는 어떻게 내가 설치하지 않은 create-react-app을 알아서 설치해서 실행시켜줄까?

## 번들 사이즈 분석

create-react-app으로 프로젝트를 만들고 source-map-explorer 모듈을 이용해 번들 사이즈 분석 가능

```bash
yarn add source-map-explorer
```

source-map-explorer 설치 후 package.json의 scripts에 추가

```diff
   "scripts": {
+    "analyze": "source-map-explorer 'build/static/js/*.js'",
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test",
```

프로젝트를 빌드한 후 분석

```bash
npm run build
npm run analyze
```

참고: https://create-react-app.dev/docs/analyzing-the-bundle-size/

## Customize Webpack Config

CRA는 내부적으로 웹팩 설정을 관리함. 그래서 신경쓰지 않아도 되므로 편리하지만, 웹팩 설정을 변경하려면 좀 번거로워짐.

### `yarn eject`

숨겨져서 관리되고 있는 웹팩 설정을 모두 드러내게 만드는 스크립트. 단, 되돌릴 수 없다. 설정파일들을 꺼내서 변경한 후 부터는 직접 모든 것을 관리해야한다.

### `react-app-rewired` && `customize-cra`

`react-app-rewired`로 CRA의 웹팩 설정들을 가져오고, `customize-cra`로 설정을 추가.

참고: https://dev.to/stanleysathler/enabling-styled-components-debugging-options-in-your-cra-app-without-ejecting-16c1

## 서버의 Root Path에 배포하지 않을 때

CRA에서 제공하는 스크립트로 빌드하면 앱이 서버의 루트에 호스팅 된다고 가정하고 빌드된다. 만약 배포하려는 앱이 루트가 아닌 다른 경로(예: github.io/project)에 배포된다면 package.json의 homepage 필드에 해당 경로를 설정해야한다.

```json
{
  "homepage": "http://copyx.github.io/jetflix"
}
```

이래야 빌드된 결과물이 호스팅 될 때 문제 없음.
