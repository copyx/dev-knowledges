# Jest

## Jest에서 시간 고정하는 법

Jest에서 관련 API를 제공해줌.

```javascript
test("환불 가능 크레딧 옵션이 true면 해당 값을 함께 반환", async () => {
  jest.useFakeTimers("modern");

  // 환불 불가능 시간
  jest.setSystemTime(new Date("2021-04-21 14:00:00"));
  expect(await callFindLessonCreditInfo({ withRefundable: true })).toEqual({
    usedAmount: 1,
    refundableAmount: 0,
  });

  // 환불 가능 시간
  jest.setSystemTime(new Date("2021-04-21 13:59:59"));
  expect(await callFindLessonCreditInfo({ withRefundable: true })).toEqual({
    usedAmount: 1,
    refundableAmount: 1,
  });

  jest.useRealTimers();
});
```

### References

- https://jestjs.io/docs/jest-object#jestusefaketimersimplementation-modern--legacy<br/>
- https://jestjs.io/docs/jest-object#jestsetsystemtimenow-number--date

## 비동기 함수에서 에러를 던지는지 확인할 때

검증 대상 함수를 실행시키는 함수를 만들어서 검증해야함. 그리고 `expect()` 함수의 결과에서 `rejects` 속성을 가져오고, 여기에 `toThrow()`를 이용해 검증.

```javascript
test("GIVEN lessonAt is already reserved on the member, THEN an error should be thrown", async () => {
  await lessonDomain.reserveLesson(paramsToReserveLesson);
  await expect(
    lessonDomain.reserveLesson({
      ...paramsToReserveLesson,
      teacherId: dummyTeacherIds[1],
    })
  ).rejects.toThrow(
    new Error(
      errors.reject(
        null,
        "다른 예약과 시간이 겹쳐 예약이 불가능합니다. 새로고침 후 다시 시도해주세요."
      )
    )
  );
});
```

### Module Mocking

모듈의 함수를 실제로 호출하지 않고, 모킹하고 싶을 때는 `jest.mock(...)`로 가능.

```javascript
import axios from "axios";
import Api from "./api";

jest.mock("axios"); // 모듈 이름 혹은 모듈 경로를 인자로 전달

test("request api", async () => {
  const response = await Api.get("test value");
  expect(Api.get).toBeCalledWith("test value");
});
```

모킹할 모듈의 원래 코드를 사용하고싶다면 `jest.requireActual(...)`로 가능.

```javascript
jest.mock("node-fetch");

// import fetch, { Response } from "node-fetch"; // 이렇게하면 Response의 함수들도 모두 모킹됨
import fetch from "node-fetch";
const { Response } = jest.requireActual("node-fetch"); // 실제 로직이 필요한 부분은 이렇게 가져올 수 있음
import { createUser } from "./createUser";

test("createUser calls fetch with the right args and returns the user id", async () => {
  fetch.mockReturnValue(Promise.resolve(new Response("4")));

  const userId = await createUser();

  expect(fetch).toHaveBeenCalledTimes(1);
  expect(fetch).toHaveBeenCalledWith("http://website.com/users", {
    method: "POST",
  });
  expect(userId).toBe("4");
});
```

### Manual Mocks

테스트할 때마다 모듈을 모킹하고 함수의 모의 데이터를 선언할 필요없음. 모의 데이터를 설정한 모의 모듈을 미리 만들어둘 수 있음.
모의 데이터를 미리 설정할 모듈과 같은 경로에 `__mocks__` 디렉토리를 만들고, 모듈 파일과 같은 이름으로 모의 모듈을 만들어서 모의 데이터 설정 가능.

```text
.
├── models
│   ├── __mocks__
│   │   └── user.js
│   └── user.js
```

이렇게 만들고 테스트에서 `jest.mock('./models/user')`를 명시적으로 호출해줘야 모의 모듈의 사용됨.

node_modules에 있는 모듈도 이런 방식으로 모킹이 가능. node_modules와 같은 경로에 `__mocks__` 디렉토리를 만들고, 모듈과 같은 이름의 파일 생성.
단, node_modules의 모듈들은 `jest.mock('module_name')`을 호출해주지 않아도 됨.

```text
.
├── __mocks__
│   └── fs.js
└── node_modules
```

fs, path 같은 노드 코어 모듈들도 위와 같은 방식으로 모킹 가능. 다만 코어 모듈들은 `jest.mock('module_name')`을 호출해줘야함.
