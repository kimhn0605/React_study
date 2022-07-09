# 5. ref: DOM에 이름 달기

일반 HTML에서 DOM에 이름을 달 때는 id를 사용

```js
<div id='my-element'></div>
```

- 특정 DOM요소에 어떤 작업을 해야할 때 이렇게 요소에 id를 사용
  - CSS에서 특정 id에 특정 스타일을 적용 가능
  - 자바스크립트에서 해당 id를 가진 요소를 찾아서 작업 가능

> 💡 ref: HTML에서 id를 사용하여 DOM에 이름을 다는 것처럼 리액트 프로젝트 내부에서 DOM에 이름을 다는 방법

#### 리액트 컴포넌트에서 id를 사용하면 안되는가

- 사용할 수 있으나 권장되지 않음
- JSX 안에서 DOM에 id를 달면 해당 DOM을 렌더링할 때 그대로 전달
  - 같은 컴포넌트를 여러 번 사용한다고 가정한다면
  - id는 유일해야 하는데, 중복 id가 생길 수 있음
- ref는 전역적으로 작동하지 않고 컴포넌트 내부에서만 작동하기 때문에 이런 문제가 생기지 않음

## 5.1 ref는 어떤 상황에서 사용해야 할까?

> 💡 DOM을 꼭 직접적으로 건드려야 할 때 사용

### DOM을 꼭 사용해야 하는 상황

- 다음과 같은 상황에서는 state만으로는 해결이 안되고 ref를 사용해야 함

  - 특정 input에 포커스 주기
  - 스크롤 박스 조작하기
  - Canvas 요소에 그림 그리기 등

## 5.2 ref 사용

> 💡 ref를 사용하는 방법은 두 가지

### 5.2.1 콜백 함수를 통한 ref 설정

👉 가장 기본적인 방법

- ref를 달고자 하는 요소에 ref라는 콜백 함수를 props로 전달
- 해당 콜백함수는 ref 값을 파라미터로 전달받음

```jsx
<input
	ref={(ref) => {
		this.input = ref;
	}}
/>
```

👉 이렇게 하면 앞으로 this.input은 input 요소의 DOM을 가리킴

### 5.2.2 createRef를 통한 ref 설정

> 💡 리액트의 내장 함수인 createRef 함수를 사용

- 리액트 v16.3에서 도입

```jsx
import React, { Component } from 'react';

class RefSample extends Component {
	input = React.createRef();

	handleFocus = () => {
		this.input.current.focus();
	};

	render() {
		return (
			<div>
				<input ref={this.input}></input>
			</div>
		);
	}
}
```

- createRef를 사용하여 ref를 만들려면 우선 컴포넌트 내부에서 멤버 변수로 `React.createRef()`를 담아 주어야 함
- 해당 멤버 변수를 ref를 달고자 하는 요소에 ref props로 넣어주면 ref 설정 완료
- `this.input.current`를 통해 조회 가능

## 5.3 컴포넌트에 ref 달기

- 리액트에서는 컴포넌트에도 ref를 달 수 있음
- 해당 방법은 주로 컴포넌트 내부에 있는 DOM을 컴포넌트 외부에서 사용할 때 사용

### 5.3.1 사용법

```jsx
<MyComponent
	ref={(ref) => {
		this.myComponent = ref;
	}}
/>
```

👉 이렇게 하면 MyComponent 내부의 메서드 및 멤버 변수에도 접근 가능

- 즉, 내부의 ref에도 접근 가능

### 5.3.2 컴포넌트 초기 설정

#### 5.3.2.1 컴포넌트 파일 생성

```jsx
// ScrollBox.js
import React, { Component } from 'react';

class ScrollBox extends Component {
	render() {
		return <div ref={(ref) => (this.box = ref)}></div>;
	}
}
```

#### 5.3.2.2 App 컴포넌트에서 스크롤 박스 컴포넌트 렌더링

```jsx
// App.js
import React, { Component } from 'react';
import ScrollBox from './ScrollBox';

class App extends Component {
	render() {
		return (
			<div>
				<ScrollBox />
			</div>
		);
	}
}

export default App;
```

### 5.3.3 컴포넌트에 메서드 생성

```jsx
// ScrollBox.js
import React, { Component } from 'react';

class ScrollBox extends Component {
	scrollToBottom = () => {
		const { scrollHeight, clientHeight } = this.box;
		this.box.scrollTop = scrollHeight - clientHeight;
	};
	render() {
		return <div ref={(ref) => (this.box = ref)}></div>;
	}
}
```

👉 이렇게 만든 메서드는 부모 컴포넌트인 App 컴포넌트에서 ScrollBox에 ref를 달면 사용 가능

### 5.3.4 컴포넌트에 ref 달고 내부 메서드 사용

```jsx
// App.js
import React, { Component } from 'react';
import ScrollBox from './ScrollBox';

class App extends Component {
	render() {
		return (
			<div>
				<ScrollBox />
				<button
					onClick={() => {
						this.scrollBox.scrollBottom();
					}}
				>
					맨 밑으로
				</button>
			</div>
		);
	}
}

export default App;
```
