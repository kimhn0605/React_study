# 7. 컴포넌트의 라이프사이클 메서드

## 7.1 라이프사이클 메서드의 이해

- 모든 리액트 컴포넌트에는 라이프사이클 (수명 주기) 존재
  - 컴포넌트의 수명은 페이지에 렌더링되기 전인 준비 과정 ~ 페이지에서 사라질 때까지를 의미
- 라이프사이클 메서드는 클래스형 컴포넌트에서만 사용 가능
  - 함수형 컴포넌트에서는 Hooks 기능을 사용하여 비슷하게 작업 가능
    <br>

#### ※ 컴포넌트의 라이프사이클

<img style="height:400px" src="https://user-images.githubusercontent.com/77706631/178925783-c01b4c12-df60-42c3-8898-7361264fa1d4.png">

- **라이프사이클 메서드는 총 3가지 카테고리**

  - 마운트
  - 업데이트
  - 언마운트
    <br>

- **라이프사이클 메서드의 종류는 총 9가지**
  - `Will` 접두사가 붙은 메서드
    - <em> 어떤 작업을 작동하기 전에 실행되는 메서드 </em>
  - `Did` 접두사가 붙은 메서드 - <em> 어떤 작업을 작동한 후에 실행되는 메서드 </em>
    <br>

### 1) 마운트 (mount)

> 💡 DOM 이 생성되고 웹 브라우저상에 나타나는 것

<Br>

#### \* 마운트할 때 호출하는 메서드

<br>

<img style="height:300px" src="https://user-images.githubusercontent.com/77706631/178926468-758142e2-6665-49a5-8e47-bd866ef1d48d.png"> <br><Br>

- `constructor`
  - 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드
    <Br>
- `getDerivedStateFromProps`
  - props 에 있는 값을 state 에 넣을 때 사용하는 메서드
    <br>
- `render`
  - 우리가 준비한 UI 를 렌더링하는 메서드
    <br>
- `componentDidMount`
  - 컴포넌트가 웹 브라우저상에 나타난 후 호출하는 메서드
    <br>

### 2) 업데이트 (update)

> 💡 아래 4가지 경우에 해당할 때 업데이트 발생

- props 가 바뀔 때
- state 가 바뀔 때
- 부모 컴포넌트가 리렌더링될 때
- this.forceUpdate 로 강제로 렌더링을 트리거할 때

<Br>

#### \* 업데이트할 때 호출하는 메서드

<br>

<img style="height:420px" src="https://user-images.githubusercontent.com/77706631/178927446-7cd06c09-4f3a-444b-85d9-1ee544a866c9.png"> <br><br>

- `getDerivedStateFromProps`
  - props 변화에 따라 state 값에도 변화를 주고 싶을 때 사용하는 메서드
  - 마운트 과정에서도 호출되며, 업데이트가 시작하기 전에도 호출됨
    <br>
- `shouldComponentUpdate`
  - 컴포넌트가 리렌더링을 해야 할지 말아야 할지를 결정하는 메서드
  - true 혹은 false 값을 반환
    - true 반환 시 다음 라이프사이클 메서드를 계속 실행
    - false 반환 시 작업 중지, 즉 리렌더링 x
      <br>
- `render`
  - 컴포넌트를 리렌더링하는 메서드
  - 만약 특정 함수에서 this.forceUpdate() 함수를 호출한다면 위 과정을 생략하고 바로 render 함수 호출
    <br>
- `getSnapshotBeforeUpdate`
  - 컴포넌트 변화를 DOM 에 반영하기 바로 직전에 호출하는 메서드
    <br>
- `componentDidUpdate`
  - 컴포넌트의 업데이트 작업이 끝난 후 호출하는 메서드
    <br>

### 3) 언마운트 (unmount)

> 💡 마운트의 반대 과정, 즉 컴포넌트를 DOM 에서 제거하는 것

<Br>

#### \* 마운트할 때 호출하는 메서드

<br>

<img style="height:130px" src="https://user-images.githubusercontent.com/77706631/178928536-91d48721-5714-4f4a-90ec-0474d355e552.png"> <br><br>

- `componentWillUnmount`
  - 컴포넌트가 웹 브라우저상에서 사라지기 전에 호출하는 메서드
    <br>

## 7.2 라이프사이클 메서드 살펴보기

#### 1) render() 함수

```jsx
render() { ... }
```

- 라이프사이클 메서드 중 유일한 필수 메서드로서, 컴포넌트 모양새를 정의
- 메서드 내부에서 this.props 와 this.state 에 접근 가능하며, 리액트 요소를 반환
- 주의할 점
  - 이 메서드 안에서는 이벤트 설정이 아닌 곳에서 setState 사용 X
  - 브라우저의 DOM 에 접근 X
  - DOM 정보를 가져오거나 state 에 변화를 줄 때는 componentDidMount 에서 처리
    <br>

#### 2) constructor 메서드

```jsx
constructor(props) { ... }
```

- 컴포넌트의 생성자 메서드로서, 컴포넌트를 만들 때 처음으로 실행됨.
- 메서드 내부에서는 초기 state 지정 가능
  <br>

#### 3) getDerivedStateFromProps 메서드

```jsx
static getDerivedStateFromProps(nextProps, prevState) {
  if (nextProps.value !== prevState.value) {  // 조건에 따라 특정 값 동기화
    return { value: nextProps.value };
  }
  return null;    // state 를 변경할 필요가 없다면 null 반환
}
```

- 리액트 v16.3 이후에 새로 만든 라이프사이클 메서드
- props 로 받아 온 값을 state 에 동기화시키는 용도로 사용되며, 컴포넌트가 마운트될 때와 업데이트될 때 호출됨.

<br>

#### 4) componentDidMount 메서드

```jsx
componentDidMount() { ... }
```

- 컴포넌트 생성 후 첫 렌더링을 모두 마친 후 실행되는 메서드
- 메서드 내부에서 다른 js 라이브러리 또는 프레임워크의 함수를 호출하거나 이벤트 등록, setTimeout, setInterval, 네트워크 요청과 같은 비동기 작업을 주로 처리
  <br>

#### 5) shouldComponentUpdate 메서드

```jsx
shouldComponentUpdate(nextProps, nextState) { ... }
```

- props 또는 state 를 변경했을 때 리렌더링을 시작할지 여부를 지정하는 메서드로서, 반드시 불리언값을 반환
- 컴포넌트를 만들 때 이 메서드를 따로 생성하지 않으면 기본적으로 언제나 true 값을 반환
  - 이 메서드가 false 값을 반환한다면 업데이트 과정은 즉시 중지됨.
- 프로젝트 성능을 최적화할 때, 상황에 맞는 알고리즘을 작성하여 리렌더링을 방지할 때는 false 값을 반환하도록 함.
  <br>

#### 6) getSnapshotBeforeUpdate 메서드

```jsx
getSnapshotBeforeUpdate(prevProps, prevState) {
  if (prevState.array !== this.state.array) {
    const { scrollTop, scrollHeight } = this.list
    return { scrollTop, scrollHeight };
  }
}
```

- 리액트 v16.3 이후에 만들어진 메서드
- render 에서 만들어진 결과물이 브라우저에 실제로 반영되기 직전에 호출됨.
- 이 메서드에서 반환하는 값은 componentDidUpdate 의 3번째 파라미터인 snapshot 값으로 전달받을 수 있음.
  - 주로 업데이트하기 직전의 값을 참고할 일이 있을 때 활용
    <Br>

#### 7) componentDidUpdate 메서드

```jsx
componentDidUpdate(prevProps, prevState, snapshot) { ... }
```

- 리렌더링을 완료한 후 실행되는 메서드
  - 업데이트가 끝난 직후이므로, DOM 관련 처리 해도 무방
- prevProps 또는 prevState 를 통해 컴포넌트가 이전에 가졌던 데이터에 접근 가능
- getSnapshotBeforeUpdate 에서 반환한 값이 있다면 여기서 snapshot 값을 전달받을 수 있음.
  <br>

#### 8) componentWillUnmount 메서드

```jsx
componentWillUnmount() { ... }
```

- 컴포넌트를 DOM 에서 제거할 때 실행되는 메서드
- componentDidMount 에서 등록한 이벤트, 타이머, 직접 생성한 DOM 이 있다면 여기서 제거 작업 진행
  <BR>

#### 9) componentDidCatch 메서드

```JSX
componentDidCatch(error, info) {
  this.setState({
    error: true
  });
  console.log( { error, info });
}
```

- 리액트 v16 에서 새롭게 도입된 메서드
- 컴포넌트 렌더링 도중에 에러 발생 시 오류 UI 를 보여 줄 수 있게 해줌.
  - error 는 파라미터에 어떤 에러가 발생했는지,
  - info 파라미터는 어디에 있는 코드에서 오류가 발생했는지에 대한 정보를 제공
- 주의할 점
  - 컴포넌트 자신에게 발생하는 에러를 잡아낼 수 없고,
  - 자신의 this.props.children 으로 전달되는 컴포넌트에서 발생하는 에러만 잡아낼 수 있음.

<br>

## 7.3 라이프사이클 메서드 사용하기

`LifeCycleSample.js`

```jsx
import React, { Component } from "react";

class LifeCycleSample extends Component {
	state = {
		number: 0,
		color: null,
	};

	myRef = null; // ref 를 설정할 부분

	constructor(props) {
		super(props);
		console.log("constructor");
	}

	static getDerivedStateFromProps(nextProps, prevState) {
		console.log("getDerivedStateFromProps");
		if (nextProps.color !== prevState.color) {
			return { color: nextProps.color };
		}
		return null;
	}

	componentDidMount() {
		console.log("componentDidMount");
	}

	shouldComponentUpdate(nextProps, nextState) {
		console.log("shouldComponentUpdate", nextProps, nextState);
		// 숫자의 마지막 자리가 4 이면 리렌더링 X
		return nextState.number % 10 !== 4; // 불리언값 반환
	}

	componentWillUnmount() {
		console.log("componentWillMount");
	}

	handleClick = () => {
		this.setState({
			number: this.state.number + 1,
		});
	};

	getSnapshotBeforeUpdate(prevProps, prevState) {
		console.log("getSnapshotBeforeUpdate");
		if (prevProps.color !== this.props.color) {
			return this.myRef.style.color;
		}
		return null;
	}

	componentDidUpdate(prevProps, prevState, snapshot) {
		console.log("componentDidUpdate", prevProps, prevState);
		if (snapshot) {
			// getSnapshotBeforeUpdate 에서 반환한 값이 있다면
			console.log("업데이트되기 직전 색상 : ", snapshot);
		}
	}

	render() {
		console.log("render");

		const style = {
			color: this.props.color,
		};

		return (
			<div>
				<h1 style={style} ref={(ref) => (this.myRef = ref)}>
					{this.state.number}
				</h1>
				<p>color: {this.state.color}</p>
				<button onClick={this.handleClick}>더하기</button>
			</div>
		);
	}
}

export default LifeCycleSample;
```

- 위 컴포넌트는 각 라이프사이클 메서드를 실행할 때마다 콘솔에 기록하고, 부모 컴포넌트에서 props 로 색상을 받아 버튼을 누르면 state.number 값을 1씩 더함.
  <Br>

`App.js`

```jsx
import React, { Component } from "react";
import LifeCycleSample from "./LifeCycleSample";

// 랜덤 색상 생성
function getRandomColor() {
	return "#" + Math.floor(Math.random() * 16777215).toString(16);
}

class App extends Component {
	state = {
		color: "#000000",
	};

	handleClick = () => {
		this.setState({
			color: getRandomColor(),
		});
	};

	render() {
		return (
			<div>
				<button onClick={this.handleClick}>랜덤 색상</button>
				<LifeCycleSample color={this.state.color} />
			</div>
		);
	}
}
export default App;
```

- 랜덤 버튼 클릭 시 state 의 color 값을 랜덤 색상으로 설정
- 불러온 LifeCycleSample 컴포넌트에 color 값을 props 로 설정
  <br>

### 🔍 실행 화면

<br>
<img style="height:250px" src="https://user-images.githubusercontent.com/77706631/178933388-39bae154-7044-47cd-9d16-fddd5b28f5b8.png">

<br><br>

### 🔍 콘솔창

![image](https://user-images.githubusercontent.com/77706631/178980036-e418a5e9-f820-436b-8e00-ec3ada22e97d.png)

- React.StrictMode 가 적용되어 있기 때문에 일부 라이프사이클이 2번씩 호출되는 것
  <br>

`LifeCycleSample.js`

```jsx
import React, { Component } from "react";

class LifeCycleSample extends Component {
	state = {
		number: 0,
		color: null,
	};

	myRef = null; // ref 를 설정할 부분

	constructor(props) {
		super(props);
		console.log("constructor");
	}

	static getDerivedStateFromProps(nextProps, prevState) {
		console.log("getDerivedStateFromProps");
		if (nextProps.color !== prevState.color) {
			return { color: nextProps.color };
		}
		return null;
	}

	componentDidMount() {
		console.log("componentDidMount");
	}

	shouldComponentUpdate(nextProps, nextState) {
		console.log("shouldComponentUpdate", nextProps, nextState);
		// 숫자의 마지막 자리가 4 이면 리렌더링 X
		return nextState.number % 10 !== 4; // 불리언값 반환
	}

	componentWillUnmount() {
		console.log("componentWillMount");
	}

	handleClick = () => {
		this.setState({
			number: this.state.number + 1,
		});
	};

	getSnapshotBeforeUpdate(prevProps, prevState) {
		console.log("getSnapshotBeforeUpdate");
		if (prevProps.color !== this.props.color) {
			return this.myRef.style.color;
		}
		return null;
	}

	componentDidUpdate(prevProps, prevState, snapshot) {
		console.log("componentDidUpdate", prevProps, prevState);
		if (snapshot) {
			// getSnapshotBeforeUpdate 에서 반환한 값이 있다면
			console.log("업데이트되기 직전 색상 : ", snapshot);
		}
	}

	render() {
		console.log("render");

		const style = {
			color: this.props.color,
		};

		return (
			<div>
				{this.props.missing.value} {/* 존재하지 않는 값 */}
				<h1 style={style} ref={(ref) => (this.myRef = ref)}>
					{this.state.number}
				</h1>
				<p>color: {this.state.color}</p>
				<button onClick={this.handleClick}>더하기</button>
			</div>
		);
	}
}

export default LifeCycleSample;
```

- 존재하지 않는 props 인 missing 객체인 value 를 조회해서 렌더링했기 때문에 브라우저상에서는 에러 발생
  <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/178980960-e6743a57-d713-463d-8b4f-63dcd0ea8c19.png)

- 사용자가 웹 서비스를 실제로 사용할 때 위와 같이 흰 화면만 나타나게 되면 오류가 발생한 지 잘 알아채지 못함.
  <br>

`ErrorBoundary.js`

```jsx
import React, { Component } from "react";

class ErrorBoundary extends Component {
	state = {
		error: false,
	};

	componentDidCatch(error, info) {
		this.setState({
			error: true,
		});
		console.log({ error, info });
	}

	render() {
		if (this.state.error) return <div>에러가 발생했습니다!</div>; // 에러 발생 시 문구 출력
		return this.props.children; // 에러가 발생하지 않으면 this.props.children, 즉 태그 사이의 문자열이 실행됨.
	}
}

export default ErrorBoundary;
```

- ErrorBouder.js 파일을 생성하여 에러가 발생하면 componentDidCatch 메서드 호출

<br>

`App.js`

```jsx
import React, { Component } from "react";
import LifeCycleSample from "./LifeCycleSample";
import ErrorBoundary from "./ErrorBoundary";

// 랜덤 색상 생성
function getRandomColor() {
	return "#" + Math.floor(Math.random() * 16777215).toString(16);
}

class App extends Component {
	state = {
		color: "#000000",
	};

	handleClick = () => {
		this.setState({
			color: getRandomColor(),
		});
	};

	render() {
		return (
			<div>
				<button onClick={this.handleClick}>랜덤 색상</button>

				{/* 에러가 발생하지 않으면 this.props.children, 즉 태그 사이의 문자열이 실행됨. */}
				<ErrorBoundary>
					<LifeCycleSample color={this.state.color} />
				</ErrorBoundary>
			</div>
		);
	}
}
export default App;
```

- 에러 발생 시 문구를 출력하고, 발생하지 않으면 태그 사이의 props.children 이 실행될 수 있도록 함.

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/178982710-95049485-d846-47b0-bb1e-3da5fb5273da.png)

## 7.4 정리

![image](https://user-images.githubusercontent.com/77706631/178983463-34ab985c-015a-4c93-ac15-f77168fca649.png)

<br>

- 라이프사이클 메서드는 컴포넌트 상태에 변화가 있을 때마다 실행하는 메서드
  - 서드파티 라이브러리를 사용하거나 DOM 을 직접 건드려야 하는 상황에서 유용
    <br><br>

##### \*\* 이미지 출처

https://velog.io/@hixkix59/7.-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%9D%98-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%EB%A9%94%EC%84%9C%EB%93%9C
