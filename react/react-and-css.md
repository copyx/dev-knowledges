# React and CSS

`Component.module.css`와 같은 형식으로 CSS 파일을 만들어서 컴포넌트 범위로 사용 가능.

```css
/* Header.module.css */
.navList {
  display: flex;
}
```

```jsx
// Header.js
import React from "react";
import styles from "Header.module.css";
export default function Header() {
  return <header classname={styles.navList}>Header</header>;
}
```

리액트에서 클래스 이름이 겹치지 않도록 접두사로 컴포넌트 이름, 접미사로 난수를 생성해 붙여줌.

하지만 결국 이런 방법도 클래스 이름의 추가/수정/삭제 시 불편함. 그래서 **styled-components** 같은 라이브러리 사용.

## styled-components

HTML 요소나 컴포넌트에 클래스 이름을 설정해 CSS를 적용하는 방식은 동일. 하지만 컴포넌트 이름 접두사, 난수 접미사를 포함한 클래스이름 자체를 스스로 생성해서 관리. 사용자는 클래스이름을 신경쓰지 않아도 됨.

```jsx
import React from "react";
import styled from "styled-components";

const StyledHeader = styled.header`
  display: flex;
`;

export default function Header() {
  return <StyledHeader>Header</StyledHeader>;
}
```

이 외에도 다양한 기능 제공.

## styled-reset

https://github.com/zacanger/styled-reset

styled-components를 위한 Eric Meyer's Reset CSS.
