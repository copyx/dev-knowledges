# TypeScript

https://www.typescriptlang.org/

자바스크립트에 타입을 추가한 언어. 컴파일 과정을 통해 실행가능한 자바스크립트 코드로 변환. 컴파일(Compile)한다고 하지만 전통적인 컴파일과 좀 다름. 그래서 트랜스파일(Transpile)이라는 용어를 많이 사용.

원래 자바스크립트는 Interpreted 언어. 쉽게 말하면 한 줄 한 줄 실행. 그래서 버그를 런타임에 찾아야함. 하지만 타입스크립트는 컴파일 과정을 통해 컴파일 타임에 버그를 찾아낼 수 있음.

## Install

대표적으로 두 가지 방법을 사용.

### By Package Manager(NPM, Yarn)

#### Global

```bash
# 설치
npm install typescript -g
yarn global add typescript

# 삭제
npm uninstall typescript -g
yarn global remove typescript
```

#### Project

```bash
# 프로젝트 초기화
npm init # 귀찮으면 -y 옵션
yarn init

# 설치
npm install typescript -D
yarn add typescript -D
```

타입스크립트는 보통 실행 환경이 아닌 개발환경에서 필요함. 그래서 devDependency에 추가.

## Compile

특정 파일을 컴파일 할 때는 파일을 명시

```bash
tsc source.ts
# source.js 생성됨
```

프로젝트 범위의 파일들을 한 번에 컴파일 하고 싶다면 프로젝트 디렉토리 내에 설정 파일(tsconfig.json)이 필요함. 이는 명령을 통해 생성 가능. 설정 파일이 있는 상태에서 `tsc` 명령만 쳐주면, 설정에 명시된 대로 컴파일 진행.

```bash
tsc --init # tsconfig.json 생성
tsc        # 디렉토리 내 .ts 파일 컴파일 진행
```

`-w`(`--watch`) 옵션을 통해 소스코드에 변화가 있을 시 자동 컴파일 가능.

```bash
tsc -w
```

## VS Code

타입스크립트 컴파일러가 내장되어있음. VS Code 버전이 올라가면 컴파일러 버전도 올라감. 내장 컴파일러를 사용할 것인지, 외부 컴파일러를 사용할 것인지 선택 가능.

> 쉘에서 `code` 명령어를 사용하려면 PATH를 추가해야함.<br/>
> VS Code의 커맨드 팔레트에서 `Shell Command: Install 'code' command in PATH`를 검색해 실행시키면 완료.<br/>
> 출처: https://code.visualstudio.com/docs/setup/mac

## [Types by Inference(추론)](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#types-by-inference)

타입스크립트는 타입을 지정하지 않으면, 처음 저장되는 데이터의 타입으로 변수 타입을 지정한다.

```typescript
let a = "Jake"; // 암시적으로
a = 1;
```

## Type Annotation

변수 뒤에 콜론(`:`)으로 타입을 표현할 수 있음.
