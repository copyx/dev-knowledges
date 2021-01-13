# Koa

Express 만든 팀이 설계한 새로운 웹 프레임워크.<br/>
웹 애플리케이션 및 API를 위한 더 작고, 더 표현적이고(**무슨 뜻이지?**), 더 강건한 기초를 목표로 함.<br/>
`async` 함수를 활용해 콜백을 버리고 에러 처리를 향상시킴. Koa는 코어에 어떤 미들웨어도 묶어 넣지 않고, 빠르고 즐겁게 서버를 작성할 수 있게 해주는 우아한 메소드 모음을 제공함.

## Application

코아 애플리케이션(이하 앱)은 미들웨어 함수 배열을 가진 객체임. 이 미들웨어 함수들은 요청에 스택처럼 구성되고 실행됨.

코아 앱은 컨텐츠 협상, 캐시 신선도, 프록시 지원, 리디렉션 같은 평범한 작업들을 위한 메소드가 있음.

-- 좀 더 내가 받아들인 말로 써보자.

## Middleware

[Koa 깃허브 위키](https://github.com/koajs/koa/wiki#middleware)에 미들웨어 리스트가 있음.

## Koa 설치

```bash
npm install koa
```

## 간단한 Koa 앱 만들고 실행

```javascript
const koa = require("koa");
const app = new Koa();

app.listen(3000, () => {
  console.log("Server start listening to port 3000");
});
```

간단! 익스프레스랑 거의 비슷!

## Koa 라우터

제일 많이 사용되는 라우터는 [koa-router](https://github.com/koajs/router)

### koa-router 설치

```bash
$ npm install @koa/router
```

### koa-router 적용

```javascript
const Koa = require("koa");
const Router = require("@koa/router");

const app = new Koa();
const router = new Router();

router.get("/", (ctx) => {
  ctx.body = "This is root";
});

app.use(router.routes());

app.listen(3000, () => {
  console.log("Server start listening on port 3000");
});
```

익스프레스와 비슷한 스타일의 라우팅 방식.

### URL Parameter 적용

```javascript
const Koa = require("koa");
const Router = require("@koa/router");

const app = new Koa();
const router = new Router();

router.get("/", (ctx) => {
  ctx.body = "This is root";
});

router.get(
  "/users/:id",
  (ctx, next) => {
    ctx.body = `User ${ctx.params.id}'s page`;
    next();
  },
  (ctx) => {
    console.log(`${ctx._matchedRoute} is visited`);
  }
);

app.use(router.routes());

app.listen(3000, () => {
  console.log("Server start listening on port 3000");
});
```

[path-to-regexp](https://github.com/pillarjs/path-to-regexp) 모듈을 사용해 패스를 정규표현식으로 변환.

### 한 패스에 미들웨어 여러개 등록

### 중첩 라우터 적용

```javascript
// index.js
const Koa = require("koa");
const Router = require("@koa/router");

const usersRouter = require("./routers/usersRouter");

const app = new Koa();
const router = new Router();

router.get("/", (ctx) => {
  ctx.body = "This is root";
});

router.use("/users", usersRouter.routes());

app.use(router.routes());

app.listen(3000, () => {
  console.log("Server start listening on port 3000");
});
```

```javascript
// usersRouter.js
const Router = require("@koa/router");
const router = new Router();

router.get("/:id", (ctx) => {
  ctx.body = `User ${ctx.params.id}'s page`;
});

module.exports = router;
```

라우터도 미들웨어니까 `use()` 메서드로 사용. 라우터 객체의 `routes()` 메서드 호출해 미들웨어로 등록해야함.
