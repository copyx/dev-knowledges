# [Gatsby](https://www.gatsbyjs.com/)

## GraphQL로 데이터 쿼리하기

개츠비에는 GraphQL로 구성된 Data Layer(이하 데이터 레이어)가 있어서 여러가지 Data Source(이하 데이터 소스. 예: wordpress, shopify, airtable 등)로부터 데이터를 가져올 때 사용할 수 있음.

데이터 소스로부터 데이터 레이어로 데이터를 가져올 때는 **Source Plugin**(이하 소스 플러그인)을 사용함. 소스 플러그인은 보통 gatsby-source-xxx 와 같은 이름을 가짐.

`gatsby-source-filesystem` 플러그인으로 파일 시스템의 데이터에 접근 가능.

개츠비 개발환경 구성하면 GraphiQL라는 도구를 사용할 수 있음. http://localhost:8000/\_\_\_graphql 로 접근 가능. 여기서 GraphQL 쿼리를 테스트해볼 수 있고, 쿼리 가능한 필드 등을 확인할 수 있음.

### Data Layer

데이터 레이어 안에서 정보는 Nodes(이하 노드)라 불리는 객체에 저장되고, 이 노드가 데이터의 최소 형식 단위임. 소스 플러그인 마다 고유한 속성을 가진 서로 다른 종류의 노드를 생성.

### 쿼리 실행 방법

#### 1. useStaticQuery Hook

컴포넌트 안에서 사용 가능한 방식

```tsx
const SomeComponent = () => {
  const data = useStaticQuery(graphql`
    site { siteMetadata { title } }
  `);
  return <div>{title}</div>;
};
```

#### 2. Page Query

페이지 컴포넌트에서 query 변수를 export하면 컴포넌트 밖에서 useStaticQuery로 query 변수에 담긴 GraphQL 쿼리를 실행하고 그 결과를 props의 data 속성으로 전달해줌.

```tsx
const SomeComponent = ({ data }) => {
  return <div>{data.site.siteMetadata.title}</div>;
};
export const query = graphql`
  site { siteMetadata { title } }
`;
```

## Transformer Plugin

소스 플러그인을 통해 가져온 미가공 데이터의 형식이 웹사이트 빌드에 맞지 않을 때, 이 데이터를 더 쓸모있는 형식으로 가공해줌.

보통 `gatsby-transformer-xxx` 형식의 이름을 가짐.

`gatsby-plugin-mdx` 플러그인은 `.mdx` 확장자를 가진 파일 노드들을 MDX 노드로 변환.

### `parent` 필드

변환 플러그인은 새 노드를 만들 때 parent 필드에 원본 소스 노드를 연결해둠.

이를 이용해 마지막으로 파일이 수정된 시간을 가져와 표시할 수도 있음.

```graphql
query {
  allMdx {
    nodes {
      parent {
        ... on File {
          modifiedTime(formatString: "MMMM D, YYYY")
        }
      }
    }
  }
}
```

### MDX Transformer

#### 설치

```sh
npm install gatsby-plugin-mdx @mdx-js/mdx @mdx-js/react
```

```js
// gatsby-config.js
module.exports = {
  ...
  plugins: [
    ...
    "gatsby-plugin-mdx",
  ],
};
```

#### 쿼리

```graphql
query {
  allMdx(sort: { fields: frontmatter___date, order: DESC }) {
    nodes {
      frontmatter {
        title
        date(formatString: "YYYY-MM-DD")
      }
      id
      body
    }
  }
}
```

`body` 필드에 본문이 담겨지는데 사람이 읽을 수 없는 상태임. `MDXRenderer` 컴포넌트만 이해 가능.

## Remark Plugin

마크다운에 요소나 기능을 추가해주는 플러그인. `gatsby-remark-xxx` 와 같은 형식의 이름을 가짐.

## File System Route API

다이나믹 라우팅을 위한 페이지 컴포넌트 파일명 문법을 정의.

`pages/{nodeType.field}.tsx`

다이나믹 라우팅에 사용할 노드 타입과 필드를 중괄호로 묶어서 파일명에 삽입.

예: `pages/{mdx.slug}.tsx`

이렇게 셋팅하면 File System Route API는 빌드 과정에서 mdx 타입의 노드 slug 필드를 가져오고, 이를 이용해 페이지 데이터를 미리 생성해놓을 수 있음.

그리고 페이지 컴포넌트의 Props에 pageContext를 추가해 노드의 id와 slug 필드를 전달함.

## Query Variables

GraphQL 쿼리 요청에 추가 데이터를 전달할 떄 사용. 전달한 데이터를 이용해 동적인 쿼리를 만들 수 있음.

쿼리 변수를 생성할 때는 `$variable: Type`, 쿼리 안에서 사용할 때는 `$variable`과 같이 `$`를 붙여서 사용.

```graphql
query ($slug: String) {
  mdx(slug: { eq: $slug }) {
    frontmatter {
      title
      date(formatString: "MMMM D, YYYY")
    }
    body
  }
}
```

개츠비에서 쿼리 변수는 오직 페이지 쿼리 안에서만 사용 가능. `useStaticQuery` 훅으로는 사용 불가능.
