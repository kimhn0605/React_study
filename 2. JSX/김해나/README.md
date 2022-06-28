# 2. JSX

## 2.1 코드 이해하기

`src/App.js`

```jsx
import React from "react";
import logo from "./logo.svg";
import "./App.css";

function App() {
	return (
		<div className='App'>
			<header className='App-header'>
				<img src={logo} className='App-logo' alt='logo' />
				<p>
					Edit <code>src/App.js</code> and save to reload.
				</p>
				<a
					className='App-link'
					href='https://reactjs.org'
					target='_blank'
					rel='noopener noreferrer'
				>
					Learn React
				</a>
			</header>
		</div>
	);
}

export default App;
```

`import React from 'react';`

- 리액트를 불러와서 사용하겠다는 의미
- node_modules 디렉토리에 설치된 react 모듈을 불러오는 코드

- 원래 브라우저에는 모듈을 불러와서 사용하는 기능 X
- 브라우저가 아닌 환경에서 자바스크립트를 실행할 수 있게 해주는 환경인 Node.js 에서 지원하는 기능

  - Node.js 에서는 import 가 아닌 require 라는 구문으로 패키지를 불러옴.
  - 이러한 기능을 브라우저에서도 사용하려면 번들러를 사용해야 함.
    <br>

- **번들러 (Bundler)**
  - import (또는 require) 로 모듈을 불러왔을 때 불러온 모듈을 모두 합쳐서 하나의 파일을 생성해주는 도구
  - 최적화 과정에서 여러 개의 파일로 분리될 수도 있음.
  - 대표적인 번들러 - 웹펙 (리액트에서 주로 사용) - Parcel - browserify
    <br>

<img style="height:300px" src="https://user-images.githubusercontent.com/35218826/59730847-eb233b00-927e-11e9-9788-408e699c9e58.png">
<br><br>

- 이 책의 프로젝트에서는 src/index.js 를 기점으로 필요한 파일들을 모두 불러와서 번들링
  <br>

`import logo from './logo.svg';`
`import './App.css';`

- 웹팩을 사용하면 SVG 파일과 CSS 파일도 불러와서 사용 가능
- 이러한 파일들을 불러오는 것은 _웹팩의 로더_ 라는 기능이 담당하고 있음.
  <br>

- **웹팩의 로더 (loader)**

  - 로더는 여러 종류가 존재
  - css-loader 는 CSS 파일, file-loader 는 웹 폰트나 미디어 파일, babel-loader 는 자바스크립트 파일들을 불러오면서 최신 자바스크립트 문법으로 작성된 코드를 바벨이라는 도구를 사용하여 ES5 문법으로 변환해줌.
    <br>

- **최신 자바스크립트로 작성된 코드를 ES5 로 변환하는 이유**
  - 이전 버전의 자바스크립트인 ES5로 변환하는 이유는 `구버전 웹 브라우저와 호환하기 위해서`
    <br>

```jsx
function App() {
	return (
		<div className='App'>
			<header className='App-header'>
				<img src={logo} className='App-logo' alt='logo' />
				<p>
					Edit <code>src/App.js</code> and save to reload.
				</p>
				<a
					className='App-link'
					href='https://reactjs.org'
					target='_blank'
					rel='noopener noreferrer'
				>
					Learn React
				</a>
			</header>
		</div>
	);
}
```

- App 이라는 컴포넌트를 만들어주는 코드
- function 키워드를 통해 컴포넌트를 만들었기 때문에 _함수형 컴포넌트_
- _프로젝트에서 컴포넌트를 렌더링하면 함수에서 반환하고 있는 내용을 나타냄._
- 함수에서 반환하는 내용은 얼핏 보면 HTML 같지만 JSX 에 해당하는 코드
  <br>

## 2.2 JSX란?

- **JSX**

  - 자바스크리트의 확장 문법이며 XML 과 매우 비슷한 형태
  - 브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환됨.
    <br>

- **변환되는 과정**

`JSX 코드`

```jsx
function App() {
	return (
		<div>
			Hello <b>react</b>
		</div>
	);
}
```

<br>

`일반 자바스크립트 코드`

```js
function App() {
	return React.createElement("div", null, "Hello", React.createElement("b", null, "react"));
}
```

<br>

- 컴포넌트를 렌더링할 때마다 매번 React.createElement 함수를 사용하게 되면 매우 불편하기 때문에 위와 같이 JSX 코드로 작성
  <br>

## 2.3 JSX 의 장점

- **보다 높은 가독성**

  - 자바스크립트로 일일이 요소를 만드는 것보다 HTML 코드와 비슷하게 작성하는 것이 가독성이 더 높고 작성하기에도 쉬움.
    <br>

- **높은 활용도**
  - JSX 에서는 div 나 span 같은 HTML 태그를 사용할 수 있을 뿐 아니라 컴포넌트도 작성 가능
    <br>

`src/index.js`

```JSX
import React from 'react';
import reactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(
	<React.StrictMode>
		<App />
	</React.StrictMode>,
	document.getElementById('root')
);
```

<br>

`ReactDOM.rener(param1, param2)`

- **컴포넌트를 페이지에 렌더링하는 역할을 하며, react-dom 모듈을 불러와 사용 가능**
  - 첫 번째 파라미터에는 페이지에 렌더링할 내용을 JSX 형태로 작성
  - 두 번째 파라미터에는 해당 JSX 를 렌더링할 document 내부 요소를 설정
    - 위 코드에서는 id 가 root 인 요소 안에 렌더링하도록 작성
      <br>

`React.StrictMode`

- **리액트의 레거시 기능들을 사용하지 못하게 하는 기능**
- 문자열 ref, componentWillMount 등 나중에는 완전히 사라지게 될 옛날 기능을 사용했을 때 경고를 출력
  <br>

## 2.4 JSX 문법

### **1. 감싸인 요소**

- 리액트 컴포넌트에서는 요소 여러 개를 반드시 하나의 부모 요소로 감싸야 함.
  <br>

```jsx
import React from "react";

function App() {
	return (
		<div>
			<h1>첫 번째 요소</h1>
			<h2>두 번째 요소</h2>
		</div>
	);
}

export default App;
```

- 위와 같이 여러 개의 요소는 하나의 부모 요소로 감싸줘야 오류 발생 X

  - Virtual DOM 에서 컴포넌트 변화를 감지해 낼 때 효율적으로 비교할 수 있도록 **컴포넌트 내부는 하나의 DOM 트리 구조로 이루어져야 한다는 규칙이 있기 때문**
    <br>

```jsx
import React from "react";

function App() {
	return (
		<Fragment>
			<h1>첫 번째 요소</h1>
			<h2>두 번째 요소</h2>
		</Fragment>
	);
}

export default App;
```

```jsx
import React from "react";

function App() {
	return (
		<>
			<h1>첫 번째 요소</h1>
			<h2>두 번째 요소</h2>
		</>
	);
}

export default App;
```

- div 대신 Fragment 기능을 사용할 수 있고, <> 와 같이 간결하게 축약하여 사용하는 것도 가능
  <br>

### **2. 자바스크립트 표현**

- JSX 안에서 자바스크립트 표현식을 작성하려면 JSX 내부에서 { } 로 코드를 감싸면 됨.
  <br>

```JSX
import React from "react";

function App() {
	const name = '리액트';
	return (
		<>
			<h1> {name} 안녕!</h1>
		</>
	);
}

export default App;
```

<br>

##### \*\* ES6 의 const 와 let

- const : ES6 문법에서 새로 도입되었으며 한번 지정하고 나면 변경이 불가능한 상수를 선언할 때 사용하는 키워드
- let : 동적인 값을 담을 수 있는 변수를 선언할 때 사용하는 키워드
- var : ES6 이전에는 값을 담는 데 var 키워드를 사용했지만 치명적인 단점 O
  <br>

`1) var 키워드 사용`

```jsx
function myFunction() {
	var a = "Hello';
	if (true) {
		var a = 'bye';
		console.log(a);		// bye
	}
	console.log(a);		// bye
}
```

- var 키워드는 함수 단위 스코프이기 때문에 if 문 밖에서 a 를 조회해도 변경된 값이 출력된 것.

<br>

`2) let 키워드 사용`

```jsx
function myFunction() {
	let a = "Hello';
	if (true) {
		let a = 'bye';
		console.log(a);		// bye
	}
	console.log(a);		// Hello
}
```

- let 과 const 키워드는 블록 단위 스코프이기 때문에 if 문 내부에서 선언한 a 값은 if 문 밖의 a 값을 변경 x
  <br>

- **기본적으로 const 를 사용하고, 해당 값을 바꾸어야 할 때는 let 사용을 권장**
  - 같은 블록 내에 중복 선언 불가
  - const 는 한 번 선언하면 값 재설정 불가
    <Br>

### **3. if 문 대신 조건부 연산자**

- JSX 내부의 자바스크립트 표현식에서 if 문 사용 불가
- 조건문을 렌더링해야 할 때는 JSX 밖에서 if 문을 사용하여 사전에 값을 설정하거나 **{ } 안에 조건부 연산자 사용**
- 조건부 연산자의 또 다른 이름은 삼항 연산자 (조건 ? A : B)
  <br>

```jsx
import React from "react";

function App() {
	const name = "리액트";
	return (
		<div>
			{
				/* { } 안에 조건부 연산자 사용 */
				name === "리액트" ? <h1>리액트입니다.</h1> : <h2>리액트가 아닙니다.</h2>
			}
		</div>
	);
}

export default App;
```

<br>

### **4. AND 연산자 (&&) 를 사용한 조건부 렌더링**

- 특정 조건을 만족할 때 내용을 보여주고, 만족하지 않을 때 아예 아무것도 렌더링하지 않아야 할 상황에서는 2가지 방법 존재
  <br>

`1) 조건부 연산자를 사용한 예시`

```jsx
import React from "react";

function App() {
	const name = "리액트";
	return (
		<div>
			{
				/* { } 안에 조건부 연산자 사용 */
				name === "리액트" ? <h1>리액트입니다.</h1> : null
			}
		</div>
	);
}

export default App;
```

- 조건이 거짓일 때 null 을 렌더링하면 아무것도 보여주지 않도록 설정 가능
  <br>

`2) && 연산자를 사용한 예시`

```jsx
import React from "react";

function App() {
	const name = "리액트";
	return <div>{name === "리액트" && <h1>리액트입니다.</h1>}</div>;
}

export default App;
```

- && 연산자로 조건부 렌더링을 할 수 있는 이유는 리액트에서 false 를 렌더링할 때는 null 과 마찬가지로 아무것도 나타나지 않기 때문
  <br>

```jsx
const number = 0;
return number && <div>내용</div>;
```

- falsy 한 값인 0은 예외적으로 화면에 나타난다는 점 주의할 것!
  <Br>

### **5. undefined 를 렌더링하지 않기**

```jsx
import React from "react";
import "./App.css";

function App() {
	// undefined 만 반환 -> 오류 발생
	const name = undefined;
	return name;
}

export default App;
```

- **리액트 컴포넌트에서는 함수에서 undefined 만 반환하여 렌더링하는 상황이 없도록 해야 함.**
  <br>

```JSX
import React from 'react';
import './App.css';

function App() {
	// OR 연산자 사용하여 오류 방지
	const name = undefined;
	return name || '값이 undefined 입니다';
}

export default App;
```

- 어떤 값이 undefined 일 수도 있다면 **OR (||) 연산자를 사용하여 오류 방지**
  <br>

```jsx
import React from "react";
import "./App.css";

function App() {
	// JSX 내부에서는 undefined 렌더링 가능
	const name = undefined;
	return <div>{name || "리액트"}</div>;
}

export default App;
```

- JSX 내부에서 undefined 를 렌더링하는 것은 가능
  <br>

### **6. 인라인 스타일링**

- **리액트에서 DOM 요소에 스타일을 적용할 때는 문자열 형태가 아닌 객체 형태로 넣어줘야 함.**
- 스타일 이름 중 - 문자가 포함된다면 - 문자를 없애고 **카멜 표기법**으로 작성해야 함.
  - ex) background-color -> backgroundColor

<br>

`1) style 객체를 미리 선언하고 div 의 style 값으로 지정한 경우`

```jsx
import React from "react";
import "./App.css";

function App() {
	const name = "리액트";

	// 객체 형태로 스타일 작성
	const style = {
		// 카멜 표기법으로 표기
		backgroundColor: "black",
		color: "aqua",
		fontSize: "48px", // font-size -> fontSize
		fontWeight: "bold", // font-weight -> fontWeight
		padding: 16, // 단위 생략 시 px 로 자동 지정
	};
	return <div style={style}> {name} </div>;
}

export default App;
```

<br>

`2) style 객체를 미리 선언하지 않고 바로 지정한 경우`

```jsx
import React from "react";
import "./App.css";

function App() {
	const name = "리액트";

	return (
		<div
			style={{
				// 카멜 표기법으로 표기
				backgroundColor: "black",
				color: "aqua",
				fontSize: "48px", // font-size -> fontSize
				fontWeight: "bold", // font-weight -> fontWeight
				padding: 16, // 단위 생략 시 px 로 자동 지정
			}}
		>
			{name}
		</div>
	);
}

export default App;
```

<br>

### **7. class 대신 className**

- JSX 에서는 class 가 아닌 className 으로 지정

`src/App.css`

```css
.react {
	background: aqua;
	color: black;
	font-size: 48px;
	font-weight: bold;
	padding: 16px;
}
```

<br>

`src/App.js`

```jsx
import React from "react";
import "./App.css";

function App() {
	const name = "리액트";
	return <div className="react"> {name} <div>;
}

export default App;
```

- App.css 파일에서 지정했던 스타일이 div 에 적용되어 화면에 출력됨.

<br>

### **8. 꼭 닫아야 하는 태그**

- JSX 코드에서는 항상 태그를 닫아줘야 함.
  <br>

`태그를 닫지 않은 HTML 코드`

```HTML
<form>
		성 : <br>
		<input><br>
		이름 : <br>
		<input>
</form>
```

- 위 코드에서 br 과 input 태그는 닫는 태그를 적지 않아도 문제없이 작동
  <br>

`JSX 코드`

```jsx
import React from "react";
import "./App.css";

function App() {
	const name = "리액트";
	return (
		<>
			<div className='react'> {name} </div>
			<input></input>
		</>
	);
}

export default App;
```

- JSX 코드에서는 항상 닫는 태그를 적어줘야 하는데, 태그 사이에 별도의 내용이 들어가지 않는다면 아래와 같이 작성 가능

  <br>

```JSX
import React from "react";
import "./App.css";

function App() {
	const name = "리액트";
	return (
		<>
			<div className="react"> {name} </div>
			<input />
		</>
	);
}

export default App;
```

- 위와 같이 태그 선언과 동시에 닫는 태그를 self-closing 태그라고 부름.
  <br>

### **9. 주석**

- JSX 내부에서 주석을 작성할 때는 {/\* ... \*/} 로 작성
  <BR>

```jsx
import React from "react";
import "./App.css";

function App() {
	const name = "리액트";
	return (
		<>
			{/* 주석 처리 */}
			<div
				className='react' // 시작 태그를 여러 줄로 작성하는 경우에는 // 로 주석 작성 가능
			>
				{name}
			</div>
			// 이러한 주석은 /* 페이지에 그대로 출력됨. */
			<input />
		</>
	);
}

export default App;
```

- 시작 태그를 여러 줄로 작성하는 경우에는 // 로도 주석 작성 가능
