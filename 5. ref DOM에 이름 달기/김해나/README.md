# 5. ref: DOM 에 이름 달기

## 5.1 ref 는 어떤 상황에서 사용해야 할까?

- ref 는 특정 DOM 을 직접적으로 건드려야 할 때 사용

- 함수형 컴포넌트에서 ref 를 사용하기 위해서는 Hooks 사용해야 함. (8장 참고)
  <br>

`ValidationSample.css`

```css
.success {
	background-color: lightgreen;
}

.failure {
	background-color: lightcoral;
}
```

<Br>

`ValidationSample.js`

```jsx
import React, { Component } from "react";
import "./ValidationSample.css";

class ValidationSample extends Component {
	state = {
		password: "",
		clicked: false,
		validated: false,
	};

	handleChange = (e) => {
		this.setState({
			password: e.target.value,
		});
	};

	handleButtonClick = () => {
		this.setState({
			clicked: true,
			validated: this.state.password === "0000",
		});
	};

	render() {
		return (
			<div>
				<input
					type='password'
					value={this.state.password}
					onChange={this.handleChange}
					className={this.state.clicked ? (this.state.validated ? "success" : "failure") : ""}
				/>
				<button onClick={this.handleButtonClick}>검증하기</button>
			</div>
		);
	}
}

export default ValidationSample;
```

<br>

`App.js`

```jsx
import React from "react";
import ValidationSample from "./ValidationSample";

const App = () => {
	return <ValidationSample />;
};

export default App;
```

<br>

### 🔍 실행 화면

![image](https://user-images.githubusercontent.com/77706631/178253406-4343ad7f-b934-4b9d-95c7-47e8407f8e50.png)

- 앞 예제에서는 state 를 사용하여 필요한 기능을 구현했지만, 가끔 state 만으로 해결할 수 없는 기능들이 존재
  <br>

- 아래 예시 상황들은 어쩔 수 없이 DOM 에 직접적으로 접근해야 하는 경우 => `ref 사용`

  - 특정 input 에 포커스 주기
  - 스크롤 박스 조작하기
  - Canvas 요소에 그림 그리기 등
    <br>

## 5.2 ref 사용

#### 1) 콜백 함수를 통한 ref 설정

```jsx
<input ref={(ref) => {this.input=ref}} />
```

- ref 를 달고자 하는 요소에 ref 라는 콜백 함수를 props 로 전달
  - 이 콜백 함수는 ref 값을 파라미터로 전달받고,
  - 함수 내부에서 파라미터로 받은 ref 를 컴포넌트의 멤버 변수로 설정
    <Br>
- 위 예시에서 this.input 은 앞으로 input 요소의 DOM 을 가리키게 됨.
  - ref 의 이름은 자유롭게 지정 가능 ex) this.aa = ref
    <Br>

#### 2) createRef 를 통한 ref 설정

- 내장 함수 createRef 를 사용하여 ref 설정
  - 리액트 v16.3 부터 도입되었으며, 이전 버전에서는 작동 x
    <Br>

`RefSample.js`

```jsx
import React, { Component } from "react";

class RefSample extends Component {
	input = React.createRef();

	handleFocus = () => {
		this.input.current.focus();
	};

	render() {
		return (
			<div>
				<input ref={this.input} />
			</div>
		);
	}
}

export default RefSample;
```

- createRef 를 사용하여 ref 를 만들려면 우선 컴포넌트 내부에서 멤버 변수로 React.createRef() 를 담아줘야 함.
- 해당 멤버 변수를 ref 를 달고자 하는 요소에 ref props 로 넣어주면 ref 설정 완료
- 나중에 ref 를 설정해 준 DOM 에 접근하려면 this.input.current 를 조회하면 됨.
  - 콜백 함수를 사용할 때와 다른 점은 뒷부분에 .current 를 넣어줘야 한다는 것
    <br>

`ValidationSample.js`

```jsx
import React, { Component } from "react";
import "./ValidationSample.css";

class ValidationSample extends Component {
	state = {
		password: "",
		clicked: false,
		validated: false,
	};

	handleChange = (e) => {
		this.setState({
			password: e.target.value,
		});
	};

	handleButtonClick = () => {
		this.setState({
			password: "", // 버튼 클릭 시 input 입력값 초기화
			clicked: true,
			validated: this.state.password === "0000",
		});

		// 버튼 클릭 시 input 에 포커스가 자동으로 넘어가도록
		this.input.focus();
	};

	render() {
		return (
			<div>
				<input
					type='password'
					value={this.state.password}
					ref={(ref) => (this.input = ref)}
					onChange={this.handleChange}
					className={this.state.clicked ? (this.state.validated ? "success" : "failure") : ""}
				/>
				<button onClick={this.handleButtonClick}>검증하기</button>
			</div>
		);
	}
}

export default ValidationSample;
```

- 콜백 함수를 사용하여 ValidationSample 컴포넌트에도 ref 설정
- 버튼 클릭 시
  - input 요소에 포커스가 자동으로 넘어가도록 하여 텍스트 커서가 깜빡이도록
  - input 입력값 초기화되도록
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/178257386-28d857b1-1a4e-4b06-91d0-6153f94f0835.png)

## 5.3 컴포넌트에 ref 달기

- 리액트에서는 컴포넌트에도 ref 를 달 수 있는데, 이 방법은 주로 컴포넌트 내부에 있는 DOM 을 컴포넌트 외부에서 사용할 때 활용
  <br>

```JSX
<MyComponent
  ref={ (ref) => {this.myComponent=ref}}
/>
```

- 위와 같이 작성하면 MyComponent 내부의 메서드 및 멤버 변수에도 접근 가능
  - 즉, 내부의 ref 에도 접근 가능 ex) myComponent.handliClick, myComponent.input 등
    <br>

`ScrollBox.js`

```jsx
import React, { Component } from "react";

class ScrollBox extends Component {
	render() {
		const style = {
			border: "1px solid black",
			height: "300px",
			width: "300px",
			overflow: "auto",
			position: "relative",
		};

		const innerStyle = {
			width: "100%",
			height: "650px",
			background: "linear-gradient(white, black)",
		};

		return (
			<div
				style={style}
				ref={(ref) => {
					this.box = ref;
				}}
			>
				<div style={innerStyle} />
			</div>
		);
	}
}

export default ScrollBox;
```

- JSX 의 인라인 스타일링 문법으로 스크롤 박스를 생성하고, 최상위 DOM 에 ref 설정
  <br>

### 🔍 실행 화면

<br>

<img style="height:250px" src="https://user-images.githubusercontent.com/77706631/178258847-71651010-c341-42ed-bb09-deee9ff3096f.png">

<br><br>

`ScrollBox.js`

```jsx
import React, { Component } from "react";

class ScrollBox extends Component {
	scrollToBottom = () => {
		const { scrollHeight, clientHeight } = this.box;
		/* 앞 코드에서는 비구조화 할당 문법을 사용했지만, 다음 코드와 같은 의미
      const scrollHeight = this.box.scrollHeight;
      const clientHeight = this.box.clientHeight;
    */

		// 맨 아래로 스크롤을 내리려면 scrollHeight 에서 clientHeight 높이를 빼주면 됨.
		this.box.scrollTop = scrollHeight - clientHeight;
	};

	render() {
		const style = {
			border: "1px solid black",
			height: "300px",
			width: "300px",
			overflow: "auto",
			position: "relative",
		};

		const innerStyle = {
			width: "100%",
			height: "650px",
			background: "linear-gradient(white, black)",
		};

		return (
			<div
				style={style}
				ref={(ref) => {
					this.box = ref;
				}}
			>
				<div style={innerStyle} />
			</div>
		);
	}
}

export default ScrollBox;
```

`App.js`

```jsx
import React, { Component } from "react";
import ScrollBox from "./ScrollBox";

class App extends Component {
	render() {
		return (
			<div>
				<ScrollBox ref={(ref) => (this.scrollBox = ref)} />
				<button onClick={() => this.scrollBox.scrollToBottom()}>맨 밑으로</button>
			</div>
		);
	}
}
export default App;
```

- 컴포넌트에 스크롤바를 맨 아래쪽으로 내리는 메서드 생성 후 렌더링

  - scrollTop : 세로 스크롤바 위치 (0~350)
  - scrollHeight : 스크롤이 있는 박스 안의 div 높이 (650)
  - cliendHeight : 스크롤이 있는 박스의 높이 (300)
    <br>

- 문법상 onClick = {this.scrollBox.scrollBottom} 과 같이 작성해도 되긴 하지만 오류 발생

  - 컴포넌트가 처음 렌더링될 때는 this.scrollBox 값이 undefined 이므로 this.scrollBox.scrollToBottom 값을 읽어 오는 과정에서 오류 발생
    <br>

- 화살표 함수 문법을 사용하여 아예 새로운 함수를 만들고 그 내부에서 this.scrollBox.scrollToBottom 메서드를 실행해야 오류 X
  - 버튼을 누를 때 (이미 한 번 렌더링되어 this.scrollBox 를 설정한 시점) this.scrollBox.scrollToBottom 값을 읽어 와서 실행하므로 오류 발생 x

<br>

### 🔍 실행 화면

<br>

<img style="height:350px" src="https://user-images.githubusercontent.com/77706631/178263036-f1db0d48-94ee-4309-9763-21fc6618fae3.png">
