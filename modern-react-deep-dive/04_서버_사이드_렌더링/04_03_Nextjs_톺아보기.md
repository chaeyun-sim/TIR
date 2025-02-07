# 4. 서버 사이드 렌더링

## 🏷 Next.js 톺아보기

> 리액트 서버 사이드 렌더링을 기반으로 작성된 프레임워크

### 4.3.1 Next.js란?

- Vercel에서 만든 풀 스택 애플리케이션 구축 프레임워크
- 실제 디렉터리 구조를 URL로 변환하여 라우팅 사용

<br />

### 4.3.2 Next.js 시작하기

- 프로젝트 만들기

```
npx create-next-app@latest --ts
```

#### package.json

- next: Next.js의 기준이 되는 패키지
- eslint-config-next: Next.js에서 사용하도록 만들어진 eslint 설정

#### next.config.ts

```ts
/** @type (import('next').NextConfig) */
const nextConfig = {
	reactStrictMode: true,
	swcMinify: true,
};

module.exports = nextConfig;
```

- 첫번째 줄은 NextConfig을 기준으로 타입 도움을 받을 수 있는 코드
- reactStrictMode: 잠재적인 문제를 개발자에게 알리기 위한 도구
- swcMinify: 번들링과 컴파일을 빠르게 수행하기 위해 만들어진 swc를 기반으로 코드 최소화 작업 여부 설정

#### pages/\_app.tsx

- 전체 페이지의 시작점
- 웹 애플리케이션에서 공동으로 설정해야하는 것들을 여기에서 실행
- 여러 바운더리를 사용해 전역 에러 처리, reset.css와 같은 전역 CSS 선언, 모든 페이지에 사용되는 공통 데이터 제공

#### pages/\_document.tsx

```ts
import { Html, Head, Main, NextScript } from 'next/document';

export default function Document() {
	return (
		<Html lang='ko'>
			<Head />
			<body className='body'>
				<Main />
				<NextScript />
			</body>
		</Html>
	);
}
```

- 필수 파일은 아님
- `<html>`이나 `<body>`에 DOM 추가 시 사용
- \_document는 무조건 서버에서 실행, 이벤트 핸들러 추가 불가 (onClick 등)
- next/document는 오직 \_document에서 사용, title 등 담을 수 있음
- 공통 제목이 필요한 경우 \_app.tsx에, 페이지별 제목이 필요한 경우 파일 내부에 사용
- 서버에서 사용 가능한 데이터 불러오기 함수 사용 불가

#### pages/\_error.tsx

```ts
import { NextPageContext } from 'next';

function Error() {
	// ...
}

Error.getInitialprops = ({ res, err }: NextPageContext) => {
	const statusCode = res ? res.statusCode : err ? err.statusCode : '';
	return { statusCode };
};
```

- 클라이언트 또는 서버에서 발생하는 500 에러를 처리

#### pages/404.tsx

- 404 페이지 정의
- 없으면 Next.js 기본 404 페이지 사용

#### pages/500.tsx

- 서버 에러 핸들링
- \_error.tsx보다 우선적으로 실행

#### pages/index.tsx

- 폴더 구조 예시

  ```
  /pages
  ㄴ index.tsx -------- /
  ㄴ hi.tsx ----------- /hi
  ㄴ hello
    ㄴ world.tsx ------ /hello/world
    ㄴ [id].tsx ------- /hello/1, /hello/2, ...
    ㄴ [...props].tsx
  ```

- 대괄호 []와 함께 파일을 만들면 어떤 문자든 모두 올 수 있다는 뜻
- [...props]의 파일은 상위 엔드포인트로 시작하는 모든 주소가 올 수 있음
  - 해당 폴더 그룹의 가장 최상위 폴더가 hello인 경우 /hello/1, /hello/hi/ho, /hello/123/1/1 다 가능

```tsx
export default function Component({props}: {props: string[]}) {
  // ...
}

export const getServerSideProps = (context: NextPageContext) => {
  const {query: {props}} = context;
  return { props { props }}
}
```

#### 서버 라우팅과 클라이언트 라우팅의 차이

- Next.js는 SSR과 동시에 SPA, CSR 수행
- 최초 페이지 렌더링은 서버에서 수행 (페이지 루트 컴포넌트의 window는 undefined)
- next/link로 이동하면서 클라이언트에서 필요한 JS를 가져와 라우팅/렌더링 방식으로 동작

#### 페이지에서 getServerSideProps를 제거하면 어떻게 될까?

- 어떠한 방식으로 접근해도 서버에 로그가 남지 않음
- 빌드 결과물에서 이유를 찾을 수 있음

#### /pages/api/hello.ts

- api 폴더 내에 있는 파일은 /api/\*\*로 호출할 수 있음
- HTML 요청을 하는 것이 아닌 서버 요청을 주고 받음
- 예시

  ```ts
  type Data = {
  	name: string;
  };

  export default function handler(
  	req: NextApiRequest,
  	res: NextApiResponse<Data>
  ) {
  	res.status(200).json({ name: 'Harry Potter' });
  }
  ```

<br />

### 4.3.3 Data Fetching

- pages/ 폴더에 있는 라우팅 파일에서만 사용 가능
- 반드시 export를 사용해 함수를 외부로 내보내야함

#### getStaticPaths와 getStaticProps

- getStaticProps와 getStaticPaths는 반드시 함께 있어야 사용 가능
- getStaticPaths는 접근 가능한 주소를 정의하는 함수
  - params를 키로 하는 함수에 값을 배열로 넘겨주면 페이지에서 접근 가능
- getStaticProps는 해당 페이지로 요청이 왔을 때 제공할 props를 반환하는 함수

#### getServerSideProps

- 서버에서 실행하는 함수, 무조건 페이지 진입 전에 실행
- props로 내려줄 수 있는 값은 JSON만 가능
  - HTML에 정적으로 작성해서 내려주기 때문ㅈ
- 특징
  - 브라우저에서만 접근할 수 있는 객체는 접근 불가(예: window.document)
  - protocol와 domain 없이 fetch 요청 불가
  - 에러 발생 시 미리 정의한 에러 페이지로 리다이렉트(404.tsx, 500.tsx, ...)

#### getInitialProps

- getStaticProps, getServerSideProps 이전의 유일한 페이지 데이터 fetching 함수
- 루트 함수에 정적 메서드 추가, props 객체가 아닌 바로 객체 반환

<br />

### 4.3.4 스타일 적용하기

#### 전역 스타일

- 애플리케이션 전체에 공통으로 적용 -> \_app.tsx

#### 컴포넌트 레벨 CSS

- 충돌 방지를 위해 고유한 클래스명 제공
- 어느 파일에서건 추가할 수 있음

#### SCSS와 SASS

- css와 같은 방식으로 사용
- export 문법으로 variable 사용 가능

#### CSS-in-JS

- 자바스크립트 내부에 스타일시트 삽입하는 방식
- styled-jsx, styled-components, Emotion, Linaria 등

<br />

### 4.3.5 \_app.tsx 응용하기

```tsx
import App, { AppContext } from 'next/app';
import type { AppProps } from 'next/app';

function App({ Component, pageProps }: AppProps) {
	return (
		<>
			<Component {...pageProps} />
		</>
	);
}

App.getInitialProps = async (context: AppContext) => {
	const appProps = await App.getInitialProps(context);
	return appProps;
};

export default App;
```

- getInitialProps의 특성
  - 최초 시점: SSR으로 페이지 전체 요청
  - 그 이후: 클라이언트 라우팅을 수행하기 위해 해당 페이지의 getServerSideProps의 json 결과만 요청

<br />

### 4.3.6 next.config.js 살펴보기

```tsx
/**
 * @type {import('next').NextConfig}
 */

const nextConfig = {};

module.exports = nextConfig;
```

- Next.js 실행에 필요한 설정을 추가할 수 있는 파일
- @type 구문을 활용해 미리 선언되어 있는 설정 타입의 도움을 받을 수 있음
- 실무에서 자주 사용되는 설정
  |설정 | 설명 |
  | - |- |
  | `basePath` | 서비스의 기본 경로 |
  | `swcMinify` | swc를 이용해 코드 압축 여부 선택 |
  | `redirects` | 특정 주소를 다른 주소로 보내고 싶을 때 사용 |
  | `reactStrictMode` | 리액트의 엄격 모드 설정 여뷰 선택 |
  | `assetPrefix` | next 빌드 결과물을 다른 CDN에 업로드 시 주소 설정 |
