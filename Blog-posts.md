# Blog Posts

## [Techie to tech lead: My five biggest mistakes](https://muchtrans.com/translations/techie-tech-lead-my-5-biggest-mistakes.ko.html)

1. 기술적인 능력 ≠ 리더십
2. 기술에 집착하지말고 능력을 넓혀야한다.
3. 개발자가 아닌 리더로서 일해야한다.
4. 다른 사람에게 위임한 것을 모두 알고 제어할 수 없다.
5. 지금까지와 다른 신호를 포착해야한다.

## [Four REST API Versioning Strategies](https://www.xmatters.com/blog/blog-four-rest-api-versioning-strategies/)

### 1. Versioning through URI Path

> http://www.example.com/api/**1**/products
>
> - Major: URI에 표시됨. 새 API가 추가되면 새 메이저 버전.
> - Minor & Patch: URI에 표시안됨. 내부적으로 충돌이 없는 업데이트.

- **Pros:** 캐싱이 쉬움.
- **Cons:** API 전체를 코드레벨에서 분기해야해서 상당히 큰 흔적이 남음.

### 2. Versioning through query parameters

> http://www.example.com/api/products?version=1

- **Pros:** 복잡하지 않음. 최신 버전을 기본으로 하기 쉬움.
- **Cons:** 쿼리 파라미터로 라우팅하기 어려움.

### 3. Versioning through custom headers

> curl -H "Accepts-version: 1.0"
> http://www.example.com/api/products

- **Pros:** URI를 버전 정보로 채우지 않음.
- **Cons:** 커스텀 헤더가 필요함.

### 4. Versioning through content negotiation

> curl -H "Accept: application/vnd.xm.device+json; **version=1**"
> http://www.example.com/api/products

- **Pros:** 전체가 아닌 부분적인 API 버저닝이 가능. 코드에 흔적이 적게 남음. URI 라우팅 룰 구현 안해도됨.
- **Cons:** 미디어 타입과 함께 HTTP 헤더가 필요함. API 테스트가 더 어려워지고, 브라우저로 API 탐색이 어려워짐.
