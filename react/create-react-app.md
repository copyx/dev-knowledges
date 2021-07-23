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
