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
  expect(
    async () =>
      await lessonDomain.reserveLesson({
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
