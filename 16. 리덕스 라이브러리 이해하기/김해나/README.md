# 16. 리덕스 라이브러리 이해하기

### Redux

- 가장 많이 사용하는 리액트 상태 관리 라이브러리로서, 전역 상태를 관리할 때 효과적
- 컴포넌트의 상태 업데이트 관련 로직을 다른 파일로 분리시켜서 효율적으로 관리 가능함.
- 코드의 유지 보수성을 높이고, 작업 효율을 극대화시킬 수 있음.

<br>

## 16.1 개념 미리 정리하기

### 16.1.1 액션 (action)

> 💡 하나의 객체로 표현되며, 상태에 어떠한 변화가 필요하면 액션이 발생됨.

<br>

```jsx
{
  type: 'TOGGLE_VALUE'
  data: {
    id: 1,
    text: '리덕스 배우기'
  }
}
```

- 액션 객체는 **type 필드**를 반드시 가지고 있어야 하며, 이 값은 **액션의 이름을 의미**
  - 그 외의 값들은 나중에 상태 업데이트를 할 때 참고해야 할 값이며, 마음대로 작성 가능

<Br>

### 16.1.2 액션 생성 함수 (action creator)

> 💡 액션 객체를 만들어 주는 함수

<br>

```jsx
function addTodo(data) {
	return {
		type: "ADD_TODO",
		data,
	};
}

// 화살표 함수로도 만들기 가능
const changeInput = (text) => ({
	type: "CHANGE_INPUT", text;
});
```

- 어떤 변화를 일으켜야 할 때마다 액션 객체를 만들어야 하는데,
  - 매번 액션 객체를 직접 작성하기 번거롭기도 하고, 만드는 과정에서 실수로 정보를 놓칠 수 있음.
  - 이러한 일을 방지하기 위해 함수로 만들어서 관리하면 편리함.

<br>

### 16.1.3 리듀서 (Reducer)

> 💡 변화를 일으키는 함수

<br>

```jsx
const initialState = {
	counter: 1,
};

function reducer(state = initialState, action) {
	switch (action.type) {
		case INCREMENT:
			return {
				counter: state.counter + 1,
			};
		default:
			return state;
	}
}
```

- 액션을 만들어서 발생시키면 리듀서가 현재 상태와 전달받은 액션 객체를 파라미터로 받아 옴.
  - 두 값을 참고하여 **새로운 상태를 만들어서 반환**

<br>

### 16.1.4 스토어 (Store)

- **프로젝트에 리덕스를 적용하기 위해 스토어를 생성**
- 한 개의 프로젝트에는 단 하나의 스토어만 가질 수 있음.
  - 스토어 안에는 현재 애플리케이션 상태와 리듀서가 들어가 있으며, 이외에도 몇 가지 내장 함수를 지님.
    <br>

### 16.1.5 디스패치 (Dispatch)

- 스토어의 내장 함수 중 하나로, 액션을 발생시키는 것
- dispatch(action) 과 같은 형태로서, 액션 객체를 파라미터로 넣어서 호출
  - **이 함수가 호출되면 스토어는 리듀서 함수를 실행시켜서 새로운 상태를 만들어줌.**
    <br>

### 16.1.6 구독 (Subscribe)

- 스토어의 내장 함수 중 하나로, 상태가 업데이트될 때마다 리스너 함수를 호출시키는 것

<br>

```jsx
const listener = () => {
	console.log("상태가 업데이트됨.");
};

const unsubscribe = store.subscribe(listener);
unsubscribe(); // 추후 구독을 비활성화할 때 함수를 호출
```

- subscribe 함수 안에 리스터 함수를 파라미터로 넣어서 호출해 주면,
  - **이 리스너 함수가 액션이 디스패치되어 상태가 업데이트될 때마다 호출됨.**
    <br>

## 16.2 리액트 없이 쓰는 리덕스

- 리덕스는 리액트에 종속되는 라이브러리 X
  - 리액트 이외의 다른 UI 라이브러리/프레임워크와도 사용 가능
    <br>

### 16.2.1 Parcel 로 프로젝트 만들기

<br>

```bash
npm install -g parcel-bundler

mkdir vanilla-redux
cd vanilla-redux

# package.json 파일 생성
npm init -y

# redux 모듈 설치
npm i redux
```

- Parcel 도구를 사용하면 빠르게 웹 애플리케이션 프로젝트 구성 가능

<br>

`index.html`

```html
<html>
	<body>
		<div>바닐라 자바스크립트</div>
		<script src="./index.js"></script>
	</body>
</html>
```

<br>

`index.js`

```js
console.log("hello parcel");
```

- 코드 작성 후 `parcel index.html` 명령어 실행하면 개발용 서버 실행

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/191110427-78f05a73-ecf4-4b36-88f9-9491a5f24da3.png)

### 16.2.2 간단한 UI 구성하기

<br>

`index.css`

```css
.toggle {
	border: 2px solid black;
	width: 64px;
	height: 64px;
	border-radius: 32px;
	box-sizing: border-box;
}

.toggle .active {
	background: yellow;
}
```

<Br>

`index.html`

```html
<html>
	<head>
		<link rel="stylesheet" type="text/css" href="index.css" />
	</head>
	<body>
		<div class="toggle"></div>
		<hr />
		<h1>0</h1>
		<button id="increase"></button>
		<button id="decrease"></button>
		<script src="./index.js"></script>
	</body>
</html>
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/191120495-e1f702d0-249c-4999-80b3-76eaaf69aafa.png)

<br>

### 16.2.3 DOM 레퍼런스 만들기

<br>

`index.js`

```js
const divToggle = document.querySelector(".toggle");
const counter = document.querySelector("h1");
const btnIncrease = document.querySelector("#increase");
const btnDecrease = document.querySelector("#decrease");
```

- UI 관리할 때 별도의 라이브러리를 사용하지 않을 것이기 때문에 DOM 을 직접 수정
  <br>

### 16.2.4 액션 타입과 액션 생성 함수 정의

<br>

`index.js`

```js
const divToggle = document.querySelector(".toggle");
const counter = document.querySelector("h1");
const btnIncrease = document.querySelector("#increase");
const btnDecrease = document.querySelector("#decrease");

const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREASE = "INCREASE";
const DECREASE = "DECREASE";

const toggleSwitch = () => ({
	type: TOGGLE_SWITCH,
});

const increase = (difference) => ({
	type: INCREASE,
	difference,
});

const decrease = () => ({
	type: DECREASE,
});
```

- **액션 이름은 문자열 형태로, 주로 대문자로 작성하며 액션 이름은 고유해야 함.**

  - 이름이 중복되면 의도하지 않은 결과가 발생할 수도 있기 때문
    <br>

- **액션 이름을 사용하여 액션 객체를 만드는 액션 생성 함수 작성**
  - type 값을 반드시 가지고 있어야 하며, 그 외에 추후 상태를 업데이트할 때 참고하고 싶은 값은 임의로 작성 가능
    <br>

### 16.2.5 초깃값 설정

`index.js`

```js
const divToggle = document.querySelector(".toggle");
const counter = document.querySelector("h1");
const btnIncrease = document.querySelector("#increase");
const btnDecrease = document.querySelector("#decrease");

const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREASE = "INCREASE";
const DECREASE = "DECREASE";

const toggleSwitch = () => ({
	type: TOGGLE_SWITCH,
});

const increase = (difference) => ({
	type: INCREASE,
	difference,
});

const decrease = () => ({
	type: DECREASE,
});

const initialState = {
	toggle: false,
	counter: 0,
};
```

- 초깃값의 형태는 숫자, 문자열, 객체 등 상관없이 자유롭게 사용 가능
  <br>

### 16.2.6 리듀서 함수 정의

<br>

`index.js`

```js
const divToggle = document.querySelector(".toggle");
const counter = document.querySelector("h1");
const btnIncrease = document.querySelector("#increase");
const btnDecrease = document.querySelector("#decrease");

const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREASE = "INCREASE";
const DECREASE = "DECREASE";

const toggleSwitch = () => ({
	type: TOGGLE_SWITCH,
});

const increase = (difference) => ({
	type: INCREASE,
	difference,
});

const decrease = () => ({
	type: DECREASE,
});

const initialState = {
	toggle: false,
	counter: 0,
};

// state 가 undefined 일 경우 initialState 를 기본값으로 사용
function reducer(state = initialState, action) {
	// action.type 에 따라 다른 작업을 처리
	switch (action.type) {
		case TOGGLE_SWITCH:
			return {
				...state, // 불변성 유지 필수
				toggle: !state.toggle,
			};
		case INCREASE:
			return {
				...state,
				counter: state.counter + action.difference,
			};
		case DECREASE:
			return {
				...state,
				counter: state.counter - 1,
			};
		default:
			return state;
	}
}
```

- 리듀서는 변화를 일으키는 함수로, `state` 와 `action` 값을 파라미터로 받아 옴.
  <br>

- **리듀서 함수가 맨 처음 호출될 때는 state 값이 undefined**
  - 해당 값이 undefined 로 주어졌을 때 initialState 를 기본값으로 설정하기 위해 함수 파라미터에 기본값 설정
    <br>
- **리듀서에서는 상태의 불변성을 유지하면서 데이터에 변화를 일으켜줘야 함.**
  - 주로 스프레드 연산자 (...) 을 사용
    <br>

### 16.2.7 스토어 만들기

<br>

`index.js`

```js
import { createStore } from "redux";

(...)

const store = createStore(reducer);

```

- `createStore` 함수를 사용하여 스토어를 생성하고, 함수 파라미터에는 리듀서 함수를 넣어줘야 함.
  <Br>

### 16.2.8 render 함수 만들기

<br>

`index.js`

```js
import { createStore } from "redux";

const divToggle = document.querySelector(".toggle");
const counter = document.querySelector("h1");
const btnIncrease = document.querySelector("#increase");
const btnDecrease = document.querySelector("#decrease");

const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREASE = "INCREASE";
const DECREASE = "DECREASE";

const toggleSwitch = () => ({
	type: TOGGLE_SWITCH,
});

const increase = (difference) => ({
	type: INCREASE,
	difference,
});

const decrease = () => ({
	type: DECREASE,
});

const initialState = {
	toggle: false,
	counter: 0,
};

// state 가 undefined 일 경우 initialState 를 기본값으로 사용
function reducer(state = initialState, action) {
	// action.type 에 따라 다른 작업을 처리
	switch (action.type) {
		case TOGGLE_SWITCH:
			return {
				...state, // 불변성 유지 필수
				toggle: !state.toggle,
			};
		case INCREASE:
			return {
				...state,
				counter: state.counter + action.difference,
			};
		case DECREASE:
			return {
				...state,
				counter: state.counter - 1,
			};
		default:
			return state;
	}
}

const store = createStore(reducer);

const render = () => {
	const state = store.getState(); // 현재 상태를 불러옴.

	// 토글 처리
	if (state.toggle) {
		divToggle.classList.add("active");
	} else {
		divToggle.classList.remove("active");
	}

	// 카운터 처리
	counter.innerText = state.counter;
};

render();
```

- `render` 라는 함수를 작성하여 상태가 업데이트될 때마다 호출될 수 있도록 함.
  - 리액트의 render 함수와는 다르게 이미 html 을 사용하여 만들어진 UI 의 속성을 상태에 따라 변경해줌.

<br>

### 16.2.9 구독하기

<br>

`index.js`

```js
(...)


render();
store.subscribe(render);
```

- 스토어의 상태가 바뀔 때마다 render 함수가 호출되도록 작성

  - 스토어의 내장 함수 `subscribe` 사용
    <br>

- 컴포넌트에서 리덕스 상태를 조회하는 과정에서 `react-redux` 라이브러리가 위 작업을 대신해주기 때문에 굳이 사용 X
  <br>

### 16.2.10 액션 발생시키기

<br>

`index.js`

```js
(...)

divToggle.onclick = () => {
	store.dispatch(toggleSwitch());
};

btnIncrease.onclick = () => {
	store.dispatch(increase(1));
};

btnDecrease.onclick = () => {
	store.dispatch(decrease());
};
```

- 스토어의 내장 함수 `dispatch` 를 사용하여 액션을 발생시킴

  - 파라미터에는 액션 객체를 넣어주면 됨.
    <br>

- DOM 요소에 클릭 이벤트를 설정하고,
  - **이벤트 함수 내부에서 dispatch 함수를 사용하여 액션을 스토어에 전달**

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/191122954-d440fc38-a8d7-467d-866b-06d776813dbe.png)

<br>

## 16.3 리덕스의 세 가지 규칙

### 16.3.1 단일 스토어

- 하나의 애플리케이션 안에는 하나의 스토어만 존재

<br>

### 16.3.2 읽기 전용 상태

- 리덕스 상태는 읽기 전용이기 때문에 상태를 업데이트할 때 불변성을 유지하면서 값을 교체해줘야 함.
  - 불변성을 유지해야 하는 이유는 내부적으로 데이터가 변경되는 것을 감지하기 위해 얕은 비교 검사를 하기 때문
  - 객체의 변화를 감지할 때 객체의 깊숙한 안쪽까지 비교하는 것이 아니라 겉핥기 식으로 비교하여 좋은 성능을 유지하는 것

<br>

### 16.3.3 리듀서는 순수한 함수

- 변화를 일으키는 리듀서 함수는 순수한 함수여야 하며, 아래 조건을 만족해야 함.
  - 리듀서 함수는 이전 상태와 액션 객체를 파라미터로 받습니다.
  - 파라미터 외의 값에는 의존하면 안 됩니다.
  - 이전 상태는 절대로 건드리지 않고, 변화를 준 새로운 상태 객체를 만들어서 반환합니다.
  - 똑같은 파라미터로 호출된 리듀서 함수는 언제나 똑같은 결과 값을 반환해야 합니다.
    <br>
- ex) 리듀서 함수 내부에서 랜덤 값을 만들거나, Date 함수를 사용하여 현재 시간을 가져오면,
  - 파라미터가 같아도 다른 결과를 만들어 낼 수 있기 때문에 사용 X
  - 이러한 작업은 리듀서 함수 바깥에서 처리해줘야 함.
