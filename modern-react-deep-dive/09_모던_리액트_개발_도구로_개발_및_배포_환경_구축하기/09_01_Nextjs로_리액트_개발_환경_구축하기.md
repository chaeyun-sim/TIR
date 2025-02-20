# 9. 모던 리액트 개발 도구로 개발 및 배포 환경 구축하기

## 🏷 Next.js로 리액트 개발 환경 구축하기

- `create-react-app`은 추후 보일러플레이트가 아닌 프레임워크를 제안하는 런처 형태로 변경 예정

<br />

### 9.1.1 create-next-app 없이 하나씩 구축하기

- `npm init`으로 package.json 생성
- `npm i react react-dom next`로 react와 next.js 설치
- `npm i @types/react @types/react-dom @types/node eslint eslint-config-next typescript`로 devDependencies에 필요한 패키지 설치

<br />

### 9.1.2 tsconfig.json 작성하기

- 타입스크립트 설정 기록
- compilerOptions 살펴보기

```
- compilerOptions: TS를 JS로 컴파일할 때 사용하는 옵션
- target: 변환을 목표로하는 언어의 버전을 의미
- lib: target과 비슷하나 해당 버전에 대한 API 정보 확인 가능
- allowJs: 자바스크립트 파일도 컴파일할 지 결정
- skipLibCheck: 라이브러리에서 제공하는 d.ts에 대한 검사 여부 결정
- strict: 타입스크립트 컴파일러의 엄격 모드 제어
- forceConsistentCasingInFileNames: 파일 이름의 대소문자를 구분하도록 강제
- noEmit: 컴파일 없이 타입 체크만 진행
- esModuleInterop: CommonJS 방식으로 보낸 모듈을 ES 모듈 방식으로 import 가능
- module: 모듈 시스템 설정
- moduleResolution: 모듈 해석 방식 설정
- resolveJsonModule: JSON 파일 import 가능
- jsx: .tsx 파일 내부의 JSX를 어떻게 컴파일할지 설정
- include: 타입스크립트 컴파일 대상에 포함시킬 파일 목록
- exclude: 타입스크립트 컴파일 대상에 제외할 파일 목록
```

<br />

### 9.1.3 next.config.js 작성하기

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
	reactStrictMode: true,
	poweredByHeader: false,
	eslint: {
		ignoreDuringBuilds: true,
	},
};

module.exports = nextConfig;
```

<br />

### 9.1.4 ESLint와 Prettier 설정하기

- eslint-config-next는 코드 스타일링을 정의해주지는 않음
- 코드 스타일링 정의를 위해 @titicaca/eslint-config-triple 사용
- 별도 설정 필요

```js
const path = require('path');

const createConfig = require('@titicaca/eslint-config-triple/create-config');

const { extends: extendConfigs, overrides } = createConfig({
	type: 'frontend',
	project: path.resolve(__dirname, './tsconfig.json'),
});

module.exports = {
	extends: [...extendConfig, 'next/core-web-vitals'],
	overrides,
};
```

<br />

### 9.1.5 스타일 설정하기

- `npm i styled-components` 설치
- next.config.js에 `styledComponents: true` 추가
- `pages/_document.tsx`의 Head에 ServerStyleSheet 추가

```tsx
export default function Document() {
	// ...
}

Document.getInitialProps = async () => {
	const sheet = new ServerStyleSheet();
	const originalRenderPage = ctx.renderPage;

	try {
		ctx.renderPage = () =>
			originalRenderPage({
				enhanceApp: (App) => (props) => sheet.collectStyles(<App {...props} />),
			});

		const initialProps = await Document.getInitialProps(ctx);
		return {
			...initialProps,
			styles: (
				<>
					{initialProps.styles}
					{sheet.getStyleElement()}
				</>
			),
		};
	} finally {
		sheet.seal();
	}
};
```

<br />

### 9.1.6 애플리케이션 코드 작성

- Next.js에서는 src/pages 하단에 실제 페이지 라우팅 기재 (app router는 app/ 내에 작성)
