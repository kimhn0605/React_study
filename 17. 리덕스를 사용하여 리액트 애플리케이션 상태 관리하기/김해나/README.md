# 17. 리덕스를 사용하여 애플리케이션 상태 관리하기

#### 리덕스 특징

- 상태 업데이트에 관한 로직을 모듈로 따로 분리하여
  - 컴포넌트 파일과 별개로 관리 가능하기 때문에 유지 보수하는데 편리
- 여러 컴포넌트에서 동일한 상태를 공유해야 할 때 매우 유용하며,
  - 실제 업데이트가 필요한 컴포넌트만 리렌더링되도록 쉽게 최적화 가능

<br>

#### react-redux 라이브러리

- 리액트 애플리케이션에서 리덕스를 사용할 때는 store 인스턴스를 직접 사용하기보다는
  - 주로 `react-redux` 라이브러리에서 제공하는 **유틸 함수 (connect)** 와 **컴포넌트 (Provider)** 를 사용

<br>

## 17.1 작업 환경 설정

```shell
npm i redux react-redux
```

- react-redux 라이브러리 설치
  <br>

`.prettierrc`

```js
{
	"singleQuote": true,
	"semi": true,
	"useTabs": false,
	"tabWidth": 2,
	"trailingComma": "all",
	"printWidth": 80
}
```

- Prettier 적용

<br>

## 17.2 UI 준비하기

- **리덕스를 사용할 때 가장 많이 사용하는 패턴**
  <br>
  <img style="height:300px" src="https://user-images.githubusercontent.com/77706631/192259053-935670aa-11a9-443d-9046-a32abaaa6566.png">
  <br>
  - 프레젠테이셔널 컴포넌트와 컨테이너 컴포넌트를 분리하는 것
    - 코드의 재사용성이 높아지고, 관심사의 분리가 이루어져 UI 를 작성할 때 편리
      <br>

#### 💡 프레젠테이셔널 컴포넌트

- 상태 관리가 이루어지지 않고, 그저 props 를 받아 와서 화면에 UI 를 보여 주기만 하는 컴포넌트
  - 주로 `src/components` 경로에 저장

<br>

#### 💡 컨테이너 컴포넌트

- 리덕스로부터 상태를 받아 오기도 하고 리덕스 스토어에 액션을 디스패치하기도 하는 컴포넌트
  - 주로 `src/containers` 컴포넌트에 작성
    <br>

### 17.2.1 카운터 컴포넌트 만들기

<br>

`components/Counter.js`

```jsx
import React from "react";

const Counter = ({ number, onIncrease, onDecrease }) => {
	return (
		<div>
			<h1>{number}</h1>
			<div>
				<button onClick={onIncrease}>+1</button>
				<button onClick={onDecrease}>-1</button>
			</div>
		</div>
	);
};

export default Counter;
```

- 숫자를 더하고 뺄 수 있는 카운터 컴포넌트 작성

<br>

`App.js`

```jsx
import React from "react";
import Counter from "./components/Counter";

const App = () => {
	return (
		<div>
			<Counter number={0} />
		</div>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/192260795-e9b56b99-d5e9-4e8e-81f7-72dcc22a49df.png)

<br>

### 17.2.2 할 일 목록 컴포넌트 만들기

<br>

`components/Todos.js`

```jsx
import React from "react";

const TodoItem = ({ todo, onToggle, onRemove }) => {
	return (
		<div>
			<input type='checkbox' />
			<span>예제 텍스트</span>
			<button>삭제</button>
		</div>
	);
};

const Todos = ({
	input, // 인풋에 입력되는 텍스트
	todos, // 할 일 목록이 들어 있는 객체
	onChangeInput,
	onInsert,
	onToggle,
	onRemove,
}) => {
	const onSubmit = (e) => {
		e.preventDefault();
	};
	return (
		<div>
			<form onSubmit={onSubmit}>
				<input />
				<button type='submit'>등록</button>
			</form>
			<br />
			<div>
				<TodoItem />
				<TodoItem />
				<TodoItem />
			</div>
		</div>
	);
};

export default Todos;
```

- 해야 할 일을 추가하고, 체크하고, 삭제할 수 있는 할 일 목록 컴포넌트 작성
  - 위 컴포넌트들이 받아 오는 props 는 나중에 사용 예정
    <br>

`App.js`

```jsx
import React from "react";
import Counter from "./components/Counter";
import Todos from "./components/Todos";

const App = () => {
	return (
		<div>
			<Counter number={0} />
			<hr />
			<Todos />
		</div>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/192262611-610cb4e0-9ad3-43b1-b452-bad5a6ec1c9f.png)

<br>

## 17.3 리덕스 관련 코드 작성하기

- 리덕스를 사용할 때는 **액션 타입**, **액션 생성 함수**, **리듀서** 코드 작성
  - 이 코드들을 각각 다른 파일에 작성하는 방법
  - 기능별로 묶어서 파일 하나에 작성하는 방법
    <br>

#### 💡 일반적인 구조

![image](https://user-images.githubusercontent.com/77706631/192264064-0e529d5c-7634-4f4b-8460-4e885631ea3d.png)

- actions, constants, reducers 라는 3개의 디렉터리를 만들고,

  - 그 안에 기능별로 파일을 하나씩 만드는 방식
    <br>

- 코드 종류에 따라 다른 파일에 작성하여 정리할 수 있어서 편리하지만,
  - 새로운 액션을 만들 때마다 세 종류 파일 모두 수정해야 한다는 단점 O

<br>

#### 💡 Ducks 패턴

![image](https://user-images.githubusercontent.com/77706631/192263934-b9f52844-d284-49f5-aa56-c45d27583738.png)

- 액션 타입, 액션 생성 함수, 리듀서 함수를 기능별로 파일 하나에 몰아서 다 작성하는 방식
  - 앞서 설명한 일반적인 구조로 리덕스를 사용하다가 불편함을 느껴 만들어진 패턴
  - 이 교재에서는 Ducks 패턴 사용 예정
    <br>

### 17.3.1 counter 모듈 작성하기

#### 💡 모듈

- Ducks 패턴을 사용하여 액션 타입, 액션 생성 함수, 리듀서를 작성한 코드

<br>

#### 17.3.1.1 액션 타입 정의하기

<br>

`modules/counter.js`

```jsx
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";
```

- **`모듈 이름/액션 이름` 형태로 액션 타입 정의**
  - 대문자로 정의하고, 액션의 이름이 충돌되지 않도록 모듈명 붙여서 기재

<br>

#### 17.3.1.2 액션 생성 함수 만들기

<br>

`modules/counter.js`

```jsx
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";

export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });
```

- **액션 생성 함수 작성**
  - export 키워드를 사용하여 추후 다른 파일에서 이 함수를 불러올 수 있도록 함.
    <br>

#### 17.3.1.3 초기 상태 및 리듀서 함수 만들기

<br>

`modules/counter.js`

```jsx
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";

export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

const initialState = {
	number: 0,
};

function counter(state = initialState, action) {
	switch (action.type) {
		case INCREASE:
			return {
				number: state.number + 1,
			};
		case DECREASE:
			return {
				number: state.number - 1,
			};
		default:
			return state;
	}
}

export default counter;
```

- 이 모듈의 초기 상태에는 number 값을 설정하고,
  - 리듀서 함수에는 현재 상태를 참조하여 새로운 객체를 생성해서 반환하는 코드를 작성
  - 마지막엔 export default 키워드를 사용하여 함수를 내보내줌.
    <br>

#### export 와 export default 차이

- `export `
  - 여러 개를 내보낼 수 있고,
  - 불러올 때 `import { increase, decrease } from ..` 처럼 중괄호 이용해서 가져와야 함.
    <br>
- `export default`
  - 단 한 개만 내보낼 수 있고,
  - 불러올 때 `import counter from ..` 처럼 바로 가져올 수 있음.
    <br>

### 17.3.2 todos 모듈 만들기

#### 17.3.2.1 액션 타입 정의하기

<br>

`modules/todos.js`

```jsx
const CHANGE_INPUT = "todos/CHANGE_INPUT"; // 인풋 값을 변경
const INSERT = "todos/INSERT"; // 새로운 todo 를 등록
const TOGGLE = "todos/TOGGLE"; // todo 를 체크/체크 해제
const REMOVE = "todos/REMOVE"; // todo 제거
```

- 액션 타입 정의

<br>

#### 17.3.2.2 액션 생성 함수 만들기

<br>

`modules/todos.js`

```jsx
(...)

export const changeInput = (input) => ({
  type: CHANGE_INPUT,
  input,
});

let id = 3;
export const insert = (text) => ({
  type: INSERT,
  todo: {
    id: id++,
    text,
    done: false,
  },
});

export const toggle = (id) => ({
  type: TOGGLE,
  id,
});

export const remove = (id) => ({
  type: REMOVE,
  id,
});
```

- 액션 생성 함수 생성

  - 전달받은 파라미터는 액션 객체 안에서 추가 필드로 설정
    <br>

- insert 함수는 파라미터 외에도 사전에 이미 선언되어 있는 id 값에도 의존
  - id 값은 각 todo 객체가 가지는 고윳값으로, 호출될 때마다 1씩 증가
  - 여기선 이미 todo 객체 2개를 미리 넣어둘 거라서 3으로 설정해놓은 것

<br>

#### 17.3.2.3 초기 상태 및 리듀서 함수 만들기

<br>

`modules/todos.js`

```jsx
(...)

const initialState = {
	input: "",
	todos: [
		{
			id: 1,
			text: "리덕스 기초 배우기",
			done: true,
		},
		{
			id: 2,
			text: "리액트와 리덕스 사용하기",
			done: false,
		},
	],
};

const CHANGE_INPUT = 'todos/CHANGE_INPUT'; // 인풋 값을 변경
const INSERT = 'todos/INSERT'; // 새로운 todo 를 등록
const TOGGLE = 'todos/TOGGLE'; // todo 를 체크/체크 해제
const REMOVE = 'todos/REMOVE'; // todo 제거

export const changeInput = (input) => ({
  type: CHANGE_INPUT,
  input,
});

let id = 3;
export const insert = (text) => ({
  type: INSERT,
  todo: {
    id: id++,
    text,
    done: false,
  },
});

export const toggle = (id) => ({
  type: TOGGLE,
  id,
});

export const remove = (id) => ({
  type: REMOVE,
  id,
});

const initialState = {
  input: '',
  todos: [
    {
      id: 1,
      text: '리덕스 기초 배우기',
      done: true,
    },
    {
      id: 2,
      text: '리액트와 리덕스 사용하기',
      done: false,
    },
  ],
};

function todos(state = initialState, action) {
  switch (action.type) {
    case CHANGE_INPUT:
      return {
        ...state,
        input: action.input,
      };
    case INSERT:
      return {
        ...state,
        todos: state.todos.concat(action.todo),
      };
    case TOGGLE:
      return {
        ...state,
        todos: state.todos.map((todo) =>
          todo.id === action.id ? { ...todo, done: !todo.done } : todo,
        ),
      };
    case REMOVE:
      return {
        ...state,
        todos: state.todos.filter((todo) => todo.id !== action.id),
      };
    default:
      return state;
  }
}

export default todos;
```

- 객체에 한 개 이상의 값이 들어가므로 반드시 **불변성 유지** 필요
  - spread 연산자 (...) 활용

<br>

### 17.3.3 루트 리듀서 만들기

<br>

`modules/index.js`

```jsx
import { combineReducers } from "redux";
import counter from "./counter";
import todos from "./todos";

const rootReducer = combineReducers({
	counter,
	todos,
});

export default rootReducer;
```

- 추후에 createstore 함수를 사용하여 스토어를 만들 때
  - 리듀서를 단 하나만 사용해야 함.
    <br>
- 리덕스에서 제공하는 `combineReducers` 유틸 함수를 사용하여
  - 기존에 만들었던 리듀서들을 손쉽게 하나로 합쳐줄 수 있음.
    <br>
- 파일 이름을 index.js 로 설정해 주면
  - 나중에 불러올 때 디렉터리 이름까지만 입력하여 불러올 수 있음.
  - ` import rootReducer from './modules';`
    <br>

## 17.4 리액트 애플리케이션에 리덕스 적용하기

### 17.4.1 스토어 만들기

<br>

```jsx
import { createStore } from "redux";
const store = createStore(rootReducer);
```

- createStore() 함수를 통해 스토어 생성
  <br>

### 17.4.2 Provider 컴포넌트를 사용하여 프로젝트에 리덕스 적용하기

<br>

`src/index.js`

```jsx
import React from "react";
import { createRoot } from "react-dom/client";
import { createStore } from "redux";
import { Provider } from "react-redux";
import App from "./App";
import rootReducer from "./modules/index";

const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

const store = createStore(rootReducer);

root.render(
	<Provider store={store}>
		<App />
	</Provider>
);
```

- 리액트 컴포넌트에서 스토어를 사용할 수 있도록 App 컴포넌트를 react-redux 에서 제공하는 Provider 컴포넌트로 감싸줌.
  - 이 컴포넌트를 사용할 때는 store 를 props 로 전달해줘야 함.

<br>

### 17.4.3 Redux DevTools 의 설치 및 적용

#### Redux DevTools

- 리덕스 개발자 도구로, 크롬 확장 프로그램으로 설치하여 사용 가능
  <br>

```shell
npm i redux-devtools-extension
```

- 리덕스 스토어를 만들어주는 패키지

<br>

`src/index.js`

```jsx
import React from "react";
import { createRoot } from "react-dom/client";
import { createStore } from "redux";
import { Provider } from "react-redux";
import { composeWithDevTools } from "redux-devtools-extension";
import App from "./App";
import rootReducer from "./modules/index";

const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

const store = createStore(rootReducer, composeWithDevTools());

root.render(
	<Provider store={store}>
		<App />
	</Provider>
);
```

- 리덕스 개발자 도구 안의 `State` 탭을 통해 현재 리덕스 스토어 내부의 상태 확인 가능

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/192292697-8401b329-10b3-4e88-aeda-fd37e3086970.png)

<br>

## 17.5 컨테이너 컴포넌트 만들기

### ✔️ 컨테이터 컴포넌트

- 리덕스 스토어와 연동된 컴포넌트
  - 스토어에 접근하여 원하는 상태를 받아 오고, 액션도 디스패치해주는 역할

<br>

### 17.5.1 CounterContainer 만들기

<br>

- 컴포넌트를 리덕스와 연동하려면
  - react-redux 에서 제공하는 `connect() 함수` 사용

<br>

```jsx
connect(mapStateToProps, mapDispatchToProps)(연동할 컴포넌트)

// 예시
const makeContainer = connect(mapStateToProps, mapDispatchToProps)
makeContainer(타깃 컴포넌트)
```

- **mapStateToProps**
  - 리덕스 스토어 안의 상태를 컴포넌트의 props 로 넘겨주기 위해 설정하는 함수
    <br>
- **mapDispatchToProps**
  - 액션 생성 함수를 컴포넌트의 props 로 넘겨주기 위해 사용하는 함수
    <br>
- connect 함수를 호출하고 나면 또 다른 함수를 반환하는데,
  - **반환된 함수에 컴포넌트를 파라미터로 넣어 주면 리덕스와 연동**된 컴포넌트가 만들어짐.
    <br>

`components/Counter.js`

```jsx
import React from "react";

const Counter = ({ number, onIncrease, onDecrease }) => {
	return (
		<div>
			<h1>{number}</h1>
			<div>
				<button onClick={onIncrease}>+1</button>
				<button onClick={onDecrease}>-1</button>
			</div>
		</div>
	);
};

export default Counter;
```

<br>

`container/CounterContainer.js`

```jsx
import React from "react";
import Counter from "../components/Counter";
import { connect } from "react-redux";

const CounterContainer = ({ number, increase, decrease }) => {
	return <Counter number={number} onIncrease={increase} onDecrease={decrease} />;
};

const mapStateToProps = (state) => ({
	number: state.counter.number,
});

const mapDispatchToProps = (dispatch) => ({
	// 임시 함수
	increase: () => {
		console.log("increase");
	},
	decrease: () => {
		console.log("decrease");
	},
});

export default connect(mapStateToProps, mapDispatchToProps)(CounterContainer);
```

- **mapStateToProps 와 mapDispatchToProps 에서 반환하는 객체 내부의 값들은 컴포넌트의 props 로 전달됨.**
  - `mapStateToProps`
    - state 를 파라미터로 받아오며, 이 값은 현재 스토어가 지니고 있는 상태를 가리킴.
  - `mapDispatchToProps`
    - store 의 내장 함수 dispatch 를 파라미터로 받아옴.
      <br>

`App.js`

```jsx
import React from "react";
import Todos from "./components/Todos";
import CounterContainer from "./containers/CounterContainer";

const App = () => {
	return (
		<div>
			<CounterContainer />
			<hr />
			<Todos />
		</div>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/193016082-c2d8368b-d000-40e6-acde-ad670ed6e87c.png)

- +1, -1 버튼 눌러도 숫자 증가 X

<br><br>

`modules/counter.js`

```jsx
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";

export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

const initialState = {
	number: 0,
};

function counter(state = initialState, action) {
	switch (action.type) {
		case INCREASE:
			return {
				number: state.number + 1,
			};
		case DECREASE:
			return {
				number: state.number - 1,
			};
		default:
			return state;
	}
}

export default counter;
```

<br>

`containers/CounterContainer.js`

```jsx
import React from "react";
import Counter from "../components/Counter";
import { connect } from "react-redux";
import { increase, decrease } from "../modules/counter";

const CounterContainer = ({ number, increase, decrease }) => {
	return <Counter number={number} onIncrease={increase} onDecrease={decrease} />;
};

const mapStateToProps = (state) => ({
	number: state.counter.number,
});

const mapDispatchToProps = (dispatch) => ({
	increase: () => {
		dispatch(increase());
	},
	decrease: () => {
		dispatch(decrease());
	},
});

export default connect(mapStateToProps, mapDispatchToProps)(CounterContainer);
```

- console.log 대신 액션 생성 함수를 불러와서 액션 객체를 만들고 디스패치 수행

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/193019302-8aa0c08f-f23b-40c2-8677-084271b7fafd.png)

<br>

`containers/CounterContainer.js`

```jsx
import React from "react";
import Counter from "../components/Counter";
import { connect } from "react-redux";
import { increase, decrease } from "../modules/counter";

const CounterContainer = ({ number, increase, decrease }) => {
	return <Counter number={number} onIncrease={increase} onDecrease={decrease} />;
};

export default connect(
	(state) => ({
		number: state.counter.number,
	}),
	(dispatch) => ({
		increase: () => dispatch(increase()),
		decrease: () => dispatch(decrease()),
	})
)(CounterContainer);
```

- connect 함수를 사용할 때 일반적으로는 mapStateToProps 와 mapDispatchToProps 를 미리 선언해 놓고 사용
  - 하지만 위 코드처럼 **connect 함수 내부에 익명 함수 형태로 선언하는 것도 가능**

<br>

```jsx
increase: () => dispatch(increase()),
increase: () => { return dispatch(increase()) },
```

- 위 코드 두 줄의 작동 방식은 완전히 동일

<br>

`containers/CounterContainer.js`

```jsx
import React from "react";
import Counter from "../components/Counter";
import { bindActionCreators } from "redux";
import { connect } from "react-redux";
import { increase, decrease } from "../modules/counter";

const CounterContainer = ({ number, increase, decrease }) => {
	return <Counter number={number} onIncrease={increase} onDecrease={decrease} />;
};

export default connect(
	(state) => ({
		number: state.counter.number,
	}),
	(dispatch) =>
		bindActionCreators(
			{
				increase,
				decrease,
			},
			dispatch
		)
)(CounterContainer);
```

- 액션을 디스패치하기 위해 각 액션 생성 함수를 호출하고 dispatch 로 감싸는 작업이 번거롭다면
  - 리덕스에서 제공하는 `bindActionCreators` 유틸 함수 사용하면 간편

<br>

`containers/CountContainer.js`

```jsx
import React from "react";
import Counter from "../components/Counter";
import { bindActionCreators } from "redux";
import { connect } from "react-redux";
import { increase, decrease } from "../modules/counter";

const CounterContainer = ({ number, increase, decrease }) => {
	return <Counter number={number} onIncrease={increase} onDecrease={decrease} />;
};

export default connect(
	(state) => ({
		number: state.counter.number,
	}),
	{
		increase,
		decrease,
	}
)(CounterContainer);
```

- bindActionCreators 함수 사용하는 것보다 더 간편한 방법
- mapDispatchToProps 에 해당하는 파라미터를 함수 형태가 아닌
  - **액션 생성 함수로 이루어진 객체 형태로 넣어 주는 것**
    => connect 함수가 내부적으로 bindActionCreators 작업을 대신해줌.

<br>

### 17.5.2 TodosContainer 만들기

<br>

`modules/todos.js`

```jsx
const CHANGE_INPUT = "todos/CHANGE_INPUT"; // 인풋 값을 변경
const INSERT = "todos/INSERT"; // 새로운 todo 를 등록
const TOGGLE = "todos/TOGGLE"; // todo 를 체크/체크 해제
const REMOVE = "todos/REMOVE"; // todo 제거

export const changeInput = (input) => ({
	type: CHANGE_INPUT,
	input,
});

let id = 3;
export const insert = (text) => ({
	type: INSERT,
	todo: {
		id: id++,
		text,
		done: false,
	},
});

export const toggle = (id) => ({
	type: TOGGLE,
	id,
});

export const remove = (id) => ({
	type: REMOVE,
	id,
});

const initialState = {
	input: "",
	todos: [
		{
			id: 1,
			text: "리덕스 기초 배우기",
			done: true,
		},
		{
			id: 2,
			text: "리액트와 리덕스 사용하기",
			done: false,
		},
	],
};

function todos(state = initialState, action) {
	switch (action.type) {
		case CHANGE_INPUT:
			return {
				...state,
				input: action.input,
			};
		case INSERT:
			return {
				...state,
				todos: state.todos.concat(action.todo),
			};
		case TOGGLE:
			return {
				...state,
				todos: state.todos.map((todo) =>
					todo.id === action.id ? { ...todo, done: !todo.done } : todo
				),
			};
		case REMOVE:
			return {
				...state,
				todos: state.todos.filter((todo) => todo.id !== action.id),
			};
		default:
			return state;
	}
}

export default todos;
```

<br>

`containers/TodosContainer.js`

```jsx
import React from "react";
import Todos from "../components/Todos";
import { connect } from "react-redux";
import { changeInput, insert, toggle, remove } from "../modules/todos";

const TodosContainer = ({ input, todos, changeInput, insert, toggle, remove }) => {
	return (
		<Todos
			input={input}
			todos={todos}
			onChangeInput={changeInput}
			onInsert={insert}
			onToggle={toggle}
			onRemove={remove}
		/>
	);
};

export default connect(
	// 비구조화 할당을 통해 todos 를 분리하여
	// state.todos.input 대신 todos.input 을 사용
	({ todos }) => ({
		input: todos.input,
		todos: todos.todos,
	}),
	{
		changeInput,
		insert,
		toggle,
		remove,
	}
)(TodosContainer);
```

- todos 모듈에서 작성했던 액션 생성 함수와 상태 안에 있던 값을 컴포넌트의 props 로 전달

<br>

`App.js`

```jsx
import React from "react";
import CounterContainer from "./containers/CounterContainer";
import TodosContainer from "./containers/TodosContainer";

const App = () => {
	return (
		<div>
			<CounterContainer />
			<hr />
			<TodosContainer />
		</div>
	);
};

export default App;
```

<br>

`components/Todos.js`

```jsx
import React from "react";

const TodoItem = ({ todo, onToggle, onRemove }) => {
	return (
		<div>
			<input
				type='checkbox'
				onClick={() => onToggle(todo.id)}
				checked={todo.done}
				readOnly={true}
			/>
			<span style={{ textDecoration: todo.done ? "line-through" : "none" }}>{todo.text}</span>
			<button onClick={() => onRemove(todo.id)}>삭제</button>
		</div>
	);
};

const Todos = ({
	input, // 인풋에 입력되는 텍스트
	todos, // 할 일 목록이 들어 있는 객체
	onChangeInput,
	onInsert,
	onToggle,
	onRemove,
}) => {
	const onSubmit = (e) => {
		e.preventDefault();
		onInsert(input);
		onChangeInput(""); // 등록 후 인풋 초기화
	};
	const onChange = (e) => onChangeInput(e.target.value);
	return (
		<div>
			<form onSubmit={onSubmit}>
				<input value={input} onChange={onChange} />
				<button type='submit'>등록</button>
			</form>
			<br />
			<div>
				{todos.map((todo) => (
					<TodoItem todo={todo} key={todo.id} onToggle={onToggle} onRemove={onRemove} />
				))}
			</div>
		</div>
	);
};

export default Todos;
```

- Todos 컴포넌트에서 받아 온 props 를 사용하도록 구현

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/193032828-55b10057-775c-4a43-bcfb-d9ab7938da5c.png)

<br>

## 17.6 리덕스 더 편하게 사용하기

## 17.7 Hooks 를 사용하여 컨테이너 컴포넌트 만들기
