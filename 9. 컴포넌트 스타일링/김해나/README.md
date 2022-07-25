# 9. 컴포넌트 스타일링

- 리액트에서 컴포넌트를 스타일링할 때 사용하는 방식
  - `일반 CSS`
    - 컴포넌트를 스타일링하는 가장 기본적인 방식
      <br>
  - `Sass`
    - 자주 사용되는 CSS 전처리기 중 하나로, 확장된 CSS 문법을 사용하여 CSS 코드를 더욱 쉽게 작성할 수 있도록 해줌.
      <br>
  - `CSS Module`
    - 스타일을 작성할 때 CSS 클래스가 다른 CSS 클래스의 이름과 절대 충돌하지 않도록 파일마다 고유한 이름을 자동으로 생성해 주는 옵션
      <br>
  - `styled-components`
    - 스타일을 자바스크립트 파일에 내장시키는 방식으로, 스타일을 작성함과 동시에 해당 스타일이 적용된 컴포넌트를 만들 수 있게 해줌.
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

`App.js`

```jsx
import React, { Component } from "react";
import "./App.css";

class App extends Component {
	render() {
		return (
			<div className='App'>
				<header className='App-header'>
					<p className='App-p'>
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

.App-header {
	background-color: #282c34;
	min-height: 100vh;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	font-size: calc(10px + 2vmin);
	color: white;
}

.App-p {
	color: aqua;
}
```

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

- Sass 사용하려면 node-sass 라이브러리 설치
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
  - 다른 scss 파일을 불러올 때는 `@import` 구문 사용
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

- `yarn eject` 명령어 실행 후 webpack.config.js 파일에 커스터마이징할 sass-loader 설정 추가
  - 앞부분에 상대 경로 없이 styles 디렉터리 기준 절대 경로를 사용하여 불러오기 가능
  - data 옵션 설정을 통해 Sass 파일을 불러올 때마다 코드의 맨 윗부분에 특정 코드 포함시키기 가능

<br>

#### 9.2.3 node_modules 에서 라이브러리 불러오기

- Sass 장점 중 하나는 라이브러리를 쉽게 불러와서 사용할 수 있다는 것

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
@import "./styles/utils.scss";

.SassComponent {
	display: flex;
	background: $oc-gray-2;

	@include media("<768px") {
		background: $oc-gray-9;
	}

	.box {
		cursor: pointer;
		transition: all 0.3s ease-in;
		&.red {
			background: $red;
			@include square(1);
		}
	  	...
```

- SassComponent 배경색을 open-colors 팔레트 라이브러리에서 불러온 후 설정
  - 화면 가로 크기가 768px 미만이 되면 배경색을 어둡게 변경

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/180749821-fa0ec38d-1d0c-4a7a-b2b2-5bc95650b8b2.png)

<br>

## 9.3 CSS Module

- CSS 를 불러와서 사용할 때 클래스 이름을 고유한 값, 즉 **[파일명]\_[클래스명]\_[해시값]** 형태로 자동으로 만들어서 컴포넌트 스타일 클래스 이름이 중첩되는 현상을 방지해 주는 기술
  - v2 버전 이후부터는 `.module.css` 확장자로 파일 저장하기만 하면 자동으로 CSS Module 적용
    <BR>

`CSSModule.module.css`

```css
/* 자동으로 고유해지기 때문에 흔히 사용되는 단어로 클래스 이름 설정해도 무관 */

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

/* 글로벌 CSS 를 작성하고 싶을 때는 :global {...} 로 감싸면 됨. */
:global {
	.something {
		font-weight: 800;
		color: aqua;
	}
}
```

- 특정 클래스가 웹 페이지에서 전역적으로 사용되는 경우라면 `:global` 을 앞에 입력하여 글로벌 CSS 임을 명시

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

  - 전역적으로 선언한 클래스의 경우 평상시처럼 문자열로 넣어주면 됨.
    <br>

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

- CSS Module 을 사용한 클래스명을 2개 이상 적용할 때는 **ES6 문법 템플릿 리터럴** 사용
- 템플릿 리터럴 문법을 사용하고 싶지 않다면 아래와 같이 작성하는 것도 가능
  - `className={[styles.wrapper, styles.inverted].join(' ')}`

<br>

### 9.3.1 classnames

> 💡 CSS 클래스를 조건부로 설정하거나 CSS Module 사용 시 여러 개의 클래스를 설정할 때 유용한 라이브러리

<br>

```python
yarn add classnames
```

- 해당 라이브러리 설치
  <br>

#### 1) CSS 클래스를 조건부로 설정할 때

<br>

```jsx
import classNames from "classnames";

classNames("one", "two"); //  = 'one two'
classNames("one", { two: true }); //  = 'one two'
classNames("one", { two: false }); //  = 'one'
classNames("one", ["two", "three"]); //  = 'one two three'

const myClass = "hello";
classNames("one", myClass, { myCondition: true }); // = 'one hello myCondition'
```

<br>

#### \* classnames 라이브러리 사용 전

```jsx
const MyComponent = ({ highlighted, theme }) => (
	<div className={`MyComponent ${theme} ${highlighted ? "highlighted" : ""}`}>Hello</div>
);
```

<br>

#### \* classnames 라이브러리 사용 후

```jsx
const MyComponent = ({ highlighted, theme }) => (
	<div className={classNames("MyComponent", { highlighted }, theme)}>Hello</div>
);
```

- highlighted 값이 true 이면 highlighted 클래스 적용
- theme 으로 전달받은 문자열은 내용 그대로 클래스에 적용

<br>

#### 2) CSS Module 사용 시 여러 개의 클래스를 설정할 때

<br>

```jsx
import React from "react";
import classNames from "classnames/bind";
import styles from "./CSSModuls.module.css";

// 미리 styles 에서 클래스를 받아오도록 설정
const cx = classNames.bind(styleS);

const CSSModule = () => {
	return <div className={cx("wrapper", "inverted")}>Hello</div>;
};

export default CSSModule;
```

- classnames 에 내장되어 있는 bind 함수를 사용하면 클래스를 넣어줄 때마다 `styles.[클래스 이름]` 형태를 사용할 필요 X
  - 사전에 미리 styles 에서 클래스를 받아온 후 사용할 수 있게끔 설정
  - `cx('클래스 이름1', '클래스 이름2')` 형태로 사용 가능
    <br>

### 9.3.2 Sass 와 함께 사용하기

<br>

```jsx
import styles from "./CSSModule.module.scss";
```

- Sass 를 사용할 때도 파일 이름 뒤에 `.module.scss` 확장자를 사용해주면 CSSModule 로 사용 가능

<br>

### 9.3.3 CSS Module 이 아닌 파일에서 CSS Module 사용하기

<br>

```css
:local .wrapper {
	/* 스타일 */
}
```

- CSS Module 에서 글로벌 클래스 정의 시 `:global` 사용했던 것처럼
  - CSS Module 이 아닌 일반 .css/.scss 파일에서도 `:local` 을 사용하여 CSS Module 사용 가능
    <br>

## 9.4 styled-components

- **CSS-in-JS**
  - 자바스크립트 파일 안에 스타일을 선언하는 방식
  - styled-components 는 CSS-in-JS 라이브러리 중 하나

<BR>

```python
yarn add styled-components
```

- **styled-components**
  - 자바스크립트 파일 하나에 스타일까지 작성 가능
  - .css 또는 .scss 확장자를 가진 스타일 파일을 따로 만들지 않아도 된다는 큰 이점 O

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

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/180764417-a281b7e7-5fd0-422a-a96c-8971c1a5349d.png)

<br>

### 9.4.1 Tagged 템플릿 리터럴

<br>

```css
const Box = styled.div`
	background: ${(props) => props.color || "blue"};
	padding: 1rem;
	display: flex;
`;
```

- 스타일 작성 시 백틱 (`) 을 사용하여 만든 문자열에 스타일 정보를 넣어 주었는데, 이러한 문법을 **Tagged 템플릿 리터럴** 이라고 부름.
  <br>

```jsx
`hello ${{ foo: "bar" }} ${() => "world!"}!`;
```

- 템플릿에 객체를 넣거나 함수를 넣으면 형태를 잃어 버리게 됨.

  - 객체는 "[object Object]" 로 변환되고, 함수는 함수 내용이 그대로 문자열화되어 나타남.

  ##### 🔍 실행 화면

  ![image](https://user-images.githubusercontent.com/77706631/180218069-956f41c4-b83d-4468-87fa-8f65df338c24.png)

<br><br>

```jsx
function tagged(...args) {
	console.log(args);
}
tagged`hello ${{ foo: "bar" }} ${() => "world"}!`;
```

- CSS Module 에서 사용했던 일반 템플릿 리터럴과 다르게 템플릿 사이사이에 들어가는 자바스크립트 객체나 함수의 원본 값을 그대로 추출 가능

  ##### 🔍 실행 화면

  ![image](https://user-images.githubusercontent.com/77706631/180217905-db2a5db8-3c11-425a-825d-d8e9eda8f3f0.png)

<br>

#### 9.4.2 스타일링된 엘리먼트 만들기

<br>

- styled-components 를 사용하여 스타일링된 엘리먼트를 만들 때는
  - 컴포넌트 파일의 상단에서 styled 를 불러오고,
  - `styled.태그명` 을 사용하여 구현

<br>

```jsx
import styled from "styled-components";

const MyComponent = styled.div`
	font-size: 2rem;
`;
```

- styled.div 뒤에 Tagged 템플릿 리터럴 문법을 통해 스타일을 넣어 주면,
  - 해당 스타일이 적용된 div 로 이루어진 리액트 컴포넌트가 생성됨.

<br>

```jsx
// 태그 타입을 styled 함수의 인자로 전달
const MyInput = styled("input")`
	background: gray;
`;
```

```jsx
// 아예 컴포넌트 형식의 값을 넣어 줌
const Sample = ({ className }) => {
	return <div className={className}>Sample</div>;
};

const StyledSample = styled(Sample)`
	color: blue;
`;
```

- 사용해야 할 태그명이 유동적이거나 특정 컴포넌트 자체에 스타일링해 주고 싶다면 위와 같은 형태로 구현 가능

<br>

#### 9.4.3 스타일에서 props 조회하기

<br>

- styled-components 를 사용하면 스타일 쪽에서 컴포넌트에게 전달된 props 값을 참조 가능

<br>

```jsx
const Box = styled.div`
	/* props 로 넣어준 값을 직접 전달 */
	background: ${(props) => props.color || "blue"};
	padding: 1rem;
	display: flex;
`;

...

<Box color="black">(...)</Box>
...
```

- props 로 아무런 값도 주어지지 않으면 blue 를 기본 색상으로 지정
  <br>

#### 9.4.4 props 에 따른 조건부 스타일링

<br>

- 일반 CSS 클래스에서 조건부 스타일링 해야 할 때는 className 을 사용하여 조건부 스타일링
- stlyed-components 에서는 간단하게 props 로도 조건부 스타일링 구현 가능

<br>

`StyledComponent.js`

```jsx
...

/* 아래 코드는 inverted 값이 true 일 때 특정 스타일 부여 */
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
	`}
& + button {
	margin-left: 1rem;
}
...
```

- 스타일 코드 여러 줄을 props 에 따라 넣어 주어야 할 때는 CSS 를 styled-components 에서 불러 와야 함.

  - 조건부 스타일링을 할 때 props 를 참조한다면 반드시 CSS 로 감싸주어서 Tagged 템플릿 리터럴을 사용해야 함.
    <Br>

```jsx
${(props) =>
	props.inverted &&
	`
		background: none;
		border: 2px solid white;
		color: white;

		&:hover {
			background: white;
			color: black;
		}
	`};
```

- CSS 를 사용하지 않고 위와 같이 바로 문자열을 넣어줘도 작동은 하지만
  - VS Code 확장 프로그램에서 신택스 하이라이팅이 제대로 이루어지지 않는다는 단점 O
  - Tagged 템플릿 리터럴이 아니기 때문에 함수를 받아 사용하지 못해 해당 부분에서는 props 값을 사용 X
    <br>

#### 9.4.5 반응형 디자인

<br>

- 일반 CSS 사용할 때와 동일하게 media 쿼리 사용

<br>

`StyledComponent.js`

```jsx
...

const Box = styled.div`
	/* props 로 넣어준 값도 직접 전달 가능 */
	background: ${(props) => props.color || "blue"};
	padding: 1rem;
	display: flex;

	/* 기본적으로는 가로 크기 1024px 에 가운데 정렬을 하고
		가로 크기가 작아짐에 따라 크기를 줄이고
		768px 미만이 되면 꽉 채움. */
	width: 1024px;
	margin: 0 auto;
	@media (max-width: 1024px) {
		width: 768px;
	}
	@media (max-width: 768px) {
		width: 100%;
	}
`;

...
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/180769979-72800b33-cd9c-4613-a40b-4272c6a90d52.png)
