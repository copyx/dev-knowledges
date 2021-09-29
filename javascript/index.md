# Javascript

자바스크립트 언어에 대한 지식과 경험

| Link                                                | Tags         |
| --------------------------------------------------- | ------------ |
| [Javascript 소수점 처리](Javascript-소수점-처리.md) | `javascript` |
| [ES6 변경사항](ES6-변경사항.md)                     | `javascript` |
| [Javascript "use strict"](Javascript-use-strict.md) | `javascript` |

## `import`와 `dotenv`

dotenv를 이용한 환경변수 셋팅은 import의 호이스팅으로 인해 제대로 동작하지 않음.

```javascript
// index.js
import dotenv from "dotenv";
dotenv.config();

import db from "db";
```

원인은 import는 선언문이고, config 메소드 호출은 표현식이라 import만 호이스팅되기 때문. dotenv를 위한 다른 모듈을 만들어서 import해야 원하는 순서대로 실행 가능.

```javascript
// index.js
import "dotenv.js";
import db from "db";

// dotenv.js
import dotenv from "dotenv";
dotenv.config();
```

index.js에서 import 구문들이 호이스팅되도 dotenv.js는 최상단에서 import되고, 모듈을 로드하는 과정에서 config가 실행됨.
