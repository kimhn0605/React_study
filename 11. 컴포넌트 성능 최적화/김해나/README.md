# 9. 컴포넌트 스타일링

- 리액트에서 컴포넌트를 스타일링할 때 사용하는 방식
  - `일반 CSS`
    - 컴포넌트를 스타일링하는 가장 기본적인 방식
  - `Sass`
    - 자주 사용되는 CSS 전처리기 중 하나로 확장된 CSS 문법을 사용하여 CSS 코드를 더욱 쉽게 작성할 수 있도록 해줌.
  - `CSS Module`
    - 스타일을 작성할 때 CSS 클래스가 다른 CSS 클래스의 이름과 절대 충돌하지 않도록 파일마다 고유한 이름을 자동으로 생성해 주는 옵션
  - `styled-components` - 스타일을 자바스크립트 파일에 내장시키는 방식으로 스타일을 작성함과 동시에 해당 스타일이 적용된 컴포넌트를 만들 수 있게 해줌.
    <br>

## 9.1 가장 흔한 방식, 일반 CSS

- CSS 를 작성할 때 가장 중요한 점은 CSS 클래스를 중복되지 않게 만드는 것
  - 1. 이름을 지을 때 특별한 규칙을 적용
  - 2. CSS Selector 활용

<br>

#### 9.1.1 이름 짓는 규칙

- **'컴포넌트 이름-클래스 이름'**
  - 다른 컴포넌트에서 실수로 중복되는 클래스를 만들어 사용하는 경우를 방지
    - ex) App-header, MyComponent-header

<br>

- **BEM 네이밍**
  - CSS 방법론 중 하나로, 이름을 지을 때 일종의 규칙을 준수하여 해당 클래스가 어디에서 어떤 용도로 사용되는지 명확하게 작성하는 방식
    - ex) .Card_title-primary

<br>

#### 9.1.2 CSS Selector

- CSS 클래스가 특정 클래스 내부에 있는 경우에만 스타일 적용 가능
  - 컴포넌트의 최상위 html 요소에는 컴포넌트의 이름으로 클래스 이름을 짓고,
  - 그 내부에서는 소문자를 입력하거나 header 같은 태그를 사용하여 클래스 이름이 불필요한 경우에는 아예 생략 가능
    <br>

`App.js`

```jsx
import React, { Component } from "react";
import "./App.css";

class App extends Component {
	render() {
		return (
			<div className='App'>
				<header>
					<p>
						Edit <code>src/App.js</code> and save to reload.
					</p>
				</header>
			</div>
		);
	}
}

export default App;
```

<br>

`App.css`

```css
.App {
	text-align: center;
}

.App header {
	background-color: #282c34;
	min-height: 100vh;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	font-size: calc(10px + 2vmin);
	color: white;
}

.App p {
	color: aqua;
}
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/180171722-162a52d4-d180-464e-81a5-8edba67752b8.png)

<br>

## 9.2 Sass 사용하기

#### ※ Sass (Syntactically Awesome Style Sheets)

> 💡 스타일 코드의 재활용성을 높여주고, 코드의 가독성을 높여서 유지 보수를 더욱 쉽게 할 수 있도록 도와주는 CSS 전처리기 => 2가지 확장자 .scss 와 .sass 를 지원

<br>

**1) .sass**

```scss
$front-stack: Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $front-stack
  color: $primary-color
```

- .sass 확장자는 중괄호와 세미콜론 사용 X

<br>

**2) .scss**

```scss
$front-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
	font: 100% $front-stack;
	color: $primary-color;
}
```

- 기존 CSS 작성하는 방식과 문법이 크게 다르지 않아 주로 .scss 문법이 더 자주 사용됨.

<br>

```python
npm install -g node-sass
node-sass -v

# 오류 시 npm rebuild node-sass
```

- Sass 를 사용하려면 node-sass 라이브러리 설치
  - 이 라이브러리는 Sass 를 CSS 로 변환해주는 역할
    <br>

`SassComponent.js`

```jsx
import React from "react";
import "./SassComponent.scss";

const SassComponent = () => {
	return (
		<div className='SassComponent'>
			<div className='box red' />
			<div className='box orange' />
			<div className='box yellow' />
			<div className='box green' />
			<div className='box blue' />
			<div className='box indigo' />
			<div className='box violet' />
		</div>
	);
};

export default SassComponent;
```

<br>

`SassComponent.scss`

```scss
// 변수 사용하기
$red: #fa5252;
$orange: #fd7e14;
$yellow: #fcc419;
$green: #40c057;
$blue: #339af0;
$indigo: #5c7cfa;
$violet: #7950f2;

// 믹스인 => 재사용되는 스타일 블록을 함수처럼 사용 가능
@mixin square($size) {
	$calculated: 32px * $size;
	width: $calculated;
	height: $calculated;
}

.SassComponent {
	display: flex;
	.box {
		// 일반 CSS 에선 .SassComponent .box 와 마찬가지
		background: red;
		cursor: pointer;
		transition: all 0.3s ease-in;

		&.red {
			// .red 클래스가 .box 와 함께 사용 됐을 때
			background: $red;
			@include square(1);
		}
		&.orange {
			background: $orange;
			@include square(2);
		}
		&.yellow {
			background: $yellow;
			@include square(3);
		}
		&.green {
			background: $green;
			@include square(4);
		}
		&.blue {
			background: $blue;
			@include square(5);
		}
		&.indigo {
			background: $indigo;
			@include square(6);
		}
		&.violet {
			background: $violet;
			@include square(7);
		}
		&:hover {
			// .box 에 마우스 올렸을 때
			background: black;
		}
	}
}
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/180201201-5b676039-469c-4271-a82e-ca2ec1ad8347.png)

<br>

#### 9.2.1 utils 함수 분리하기

- 여러 파일에서 사용될 수 있는 Sass 변수 및 믹스인은 다른 파일로 따로 분리하여 작성한 뒤 필요한 곳에서 쉽게 불러와 사용 가능

  - src 디렉토리에 styles 라는 디렉터리 생성 후, utils.scss 파일 생성
    <br>

`utils.scss`

```scss
// 변수 사용하기
$red: #fa5252;
$orange: #fd7e14;
$yellow: #fcc419;
$green: #40c057;
$blue: #339af0;
$indigo: #5c7cfa;
$violet: #7950f2;

// 믹스인 => 재사용되는 스타일 블록을 함수처럼 사용 가능
@mixin square($size) {
	$calculated: 32px * $size;
	width: $calculated;
	height: $calculated;
}
```

<br>

`SassComponent.scss`

```scss
@import "./styles/utils.scss"; // utils.scss import

.SassComponent {
	display: flex;
	.box {
		background: red; // 일반 CSS 에선 .SassComponent .box 와 마찬가지
		cursor: pointer;
		transition: all 0.3s ease-in;
		&.red {
			// .red 클래스가 .box 와 함께 사용 됐을 때
			background: $red;
			@include square(1);
		}
		&.orange {
			background: $orange;
			@include square(2);
		}
		&.yellow {
			background: $yellow;
			@include square(3);
		}
		&.green {
			background: $green;
			@include square(4);
		}
		&.blue {
			background: $blue;
			@include square(5);
		}
		&.indigo {
			background: $indigo;
			@include square(6);
		}
		&.violet {
			background: $violet;
			@include square(7);
		}
		&:hover {
			// .box 에 마우스 올렸을 때
			background: black;
		}
	}
}
```

- 다른 scss 파일 불러올 때는 @import 구문 사용
  <br>

#### 9.2.2 sass-loader 설정 커스터마이징하기

<br>

```python
@import '../../../styles/utils';
```

- 위 예시처럼 상위 폴더로 한참 거슬러 올라가야 하는 경우
  - 웹팩에서 Sass 를 처리하는 sass-loader 설정을 커스터마이징하여 해결 가능
    <Br>

```jsx
{
  test: sassRegex,
  exclude: sassModuleRegex,
  use: getStyleLoaders({
    importLoaders: 2,
    sourceMap: isEnvProduction && shouldUseSourceMap
  }).concat({
    loader: require.resolve('sass-loader'),
    options: {
      includePaths: [paths.appSrc + '/styles'],
      sourceMap: isEnvProduction && shouldUseSourceMap,
      prependData: `@import 'utils';`
    }
  }),
  sideEffects: true
},
```

- `yarn eject` 명령어 실행 후 webpack.config.js 파일에 커스터마이징될 sass-loader 설정 추가
  - 앞부분에 상대 경로 없이 styles 디렉터리 기준 절대 경로를 사용하여 불러오기 가능
  - data 옵션 설정을 통해 Sass 파일을 불러올 때마다 코드의 맨 윗부분에 특정 코드 포함시키기 가능

<br>

#### 9.2.3 node_modules 에서 라이브러리 불러오기

- Sass 장점 중 하나는 라이브러리를 쉽게 불러와서 사용할 수 있다는 점

<br>

##### 1) yarn 을 통해 설치한 라이브러리를 사용하는 가장 기본적인 방법

```python
@import '../../node-modules/library/styles';
```

- 상대 경로를 사용하여 node-modules 까지 들어가서 불러와야 함.
  <br>

##### 2) 물결 문자 (~) 를 사용하는 방법

```python
@import '~library/styles';
```

- 물결 문자를 사용하면 자동으로 node-modules 에서 라이브러리 디렉토리를 탐지하여 스타일 불러오기 가능
  <br>

`utils.scss`

```scss
@import "~include-media/dist/include-media";
@import "~open-color/open-color";
```

<Br>

`SassComponent.scss`

```scss
.SassComponent {
  display: flex;
  background: $oc-gray-2;
  @include media('<768px') {
    background: $oc-gray-9;
  }
  (...)
}
```

- SassComponent 배경색을 open-colors 팔레트 라이브러리에서 불러온 후 설정하고,
  - 화면 가로 크기가 768px 미만이 되면 배경색을 어둡게 바꿔줌.

<br>

## 9.3 CSS Module

- CSS 를 불러와서 사용할 때 클래스 이름을 고유한 값, 즉 **[파일명]\_[클래스명]\_[해시값]** 형태로 자동으로 만들어서 컴포넌트 스타일 클래스 이름이 중첩되는 현상을 방지해 주는 기술
  - v2 버전 이후부터는 `.module.css` 확장자로 파일 저장하기만 하면 자동으로 CSS Module 적용
    <BR>

`CSSModule.module.css`

```css
/* 자동으로 고유해질 것이므로 흔히 사용되는 단어를 클래스 이름으로 마음대로 사용가능*/

.wrapper {
	background: black;
	padding: 1rem;
	color: white;
	font-size: 2rem;
	&.inverted {
		// inverted 가 .wrapper 와 함께 사용 됐을 때만 적용
		color: black;
		background: white;
		border: 1px solid black;
	}
}

/* 글로벌 CSS 를 작성하고 싶다면 */
:global {
	// :global {} 로 감싸기
	.something {
		font-weight: 800;
		color: aqua;
	}
}
```

- 특정 클래스가 웹 페이지에서 전역적으로 사용되는 경우라면 :global 을 앞에 입력하여 글로벌 CSS 임을 명시

<Br>

`CSSModule.js`

```jsx
import React from "react";
import styles from "./CSSModule.module.scss";

const CSSModule = () => {
	return (
		<div className={styles.wrapper}>
			안녕하세요, 저는 <span className='something'>CSS Module!</span>
		</div>
	);
};

export default CSSModule;
```

- 고유 클래스명을 사용하려면 JSX 엘리먼트에 `className={styles.[클래스명]}` 형태로 전달
- CSS Modlue 이 적용된 스타일 파일을 불러오면 객체를 하나 전달받게 되는데,
  - CSS Module 에서 사용한 클래스명과 해당 이름을 고유화한 값이 키-값 형태로 들어 있게 됨.
    - ex) { wrapper: "CSSModule_wrapper\_\_1Sbd0" }

<br>

---

<br>

`CSSModule.js`

```jsx
import React from "react";
import styles from "./CSSModule.module.scss";

const CSSModule = () => {
	return (
		<div className={`${styles.wrapper} ${styles.inverted}`}>
			안녕하세요, 저는 <span className='something'>CSS Module!</span>
		</div>
	);
};

export default CSSModule;
```

- CSS Module 을 사용한 클래스명을 2개 이상 적용할 때는 ES6 문법 템플릿 리터럴 사용
- 템플릿 리터럴 문법을 사용하고 싶지 않다면 아래와 같이 작성하는 것도 가능
  - `className={[styles.wrapper, styles.inverted].join(' ')}`

<br>

## 9.4 styled-components

- **CSS-in-JS**
  - 자바스크립트 파일 안에 스타일을 선언하는 방식
  - styled-components 는 CSS-in-JS 라이브러리 중 하나

<BR>

- **styled-components**
  - 자바스크립트 파일 하나에 스타일까지 작성 가능
  - .css 또는 .scss 확장자를 가진 스타일 파일을 따로 만들지 않아도 된다는 큰 이점

<br>

`StyledComponent.js`

```jsx
import React from "react";
import styled, { css } from "styled-components";

const Box = styled.div`
	/* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
	background: ${(props) => props.color || "blue"};
	padding: 1rem;
	display: flex;
`;

const Button = styled.button`
	background: white;
	color: black;
	border-radius: 4px;
	padding: 0.5rem;
	display: flex;
	align-items: center;
	justify-content: center;
	box-sizing: border-box;
	font-size: 1rem;
	font-weight: 600;

	/* & 문자를 사용하여 Sass 처럼 자기 자신 선택 가능 */
	&:hover {
		background: rgba(255, 255, 255, 0.9);
	}

	/* inverted 값이 true 일 때만 특정 스타일 부여 */
	${(props) =>
		props.inverted &&
		css`
			background: none;
			border: 2px solid white;
			color: white;
			&:hover {
				background: white;
				color: black;
			}
		`};
	& + button {
		margin-left: 1rem;
	}
`;

const StyledComponent = () => (
	<Box color='black'>
		<Button>안녕하세요</Button>
		<Button inverted={true}>테두리만</Button>
	</Box>
);

export default StyledComponent;
```

- 일반 classNames 를 사용하는 CSS/Sass 를 비교했을 때, 가장 큰 장점은 props 값으로 전달해 주는 값을 쉽게 스타일에 적용할 수 있다는 것

<br>

#### ※ Tagged 템플릿 리터럴

<br>

```css
const Box = styled.div`
	/* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
	background: ${(props) => props.color || "blue"};
	padding: 1rem;
	display: flex;
`;
```

- 스타일 작성 시 ` 을 사용하여 만든 문자열에 스타일 정보를 넣어 주었는데, 이러한 문법을 **Tagged 템플릿 리터럴** 이라고 부름.
  <br>

```jsx
`hello ${{ foo: "bar" }} ${() => "world!"}!`;
// 결과 : "hello [object Object] () => 'world'!"
```

![image](https://user-images.githubusercontent.com/77706631/180218069-956f41c4-b83d-4468-87fa-8f65df338c24.png)

<br>

```jsx
function tagged(...args) {
	console.log(args);
}
tagged`hello ${{ foo: "bar" }} ${() => "world"}!`;
```

![image](https://user-images.githubusercontent.com/77706631/180217905-db2a5db8-3c11-425a-825d-d8e9eda8f3f0.png)

- CSS Module 에서 사용했던 일반 템플릿 리터럴과 다르게 템플릿 사이사이에 들어가는 자바스크립트 객체나 함수의 원본 값을 그대로 추출 가능

<br>

#### ※ 스타일에서 props 조회하기

<br>

```jsx
const Box = styled.div`
	/* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
	background: ${(props) => props.color || "blue"};
	padding: 1rem;
	display: flex;
`;
```
