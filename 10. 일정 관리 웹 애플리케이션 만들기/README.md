# 10. 일정 관리 웹 애플리케이션 만들기

## 10.1 프로젝트 준비하기

### 10.1.1 프로젝트 생성 및 필요한 라이브러리 설치

<br>

```bash
npm i node-sass@4.14.1 classnames react-icons
```

- `node-sass`

  - Sass 사용
    <br>

- `classnames`

  - 좀 더 편리하게 조건부 스타일링 가능
    <br>

- `react-icons`

  - https://react-icons.github.io/react-icons/
  - SVG 형태로 이루어진 아이콘을 리액트 컴포넌트처럼 쉽게 사용 가능
  - 아이콘의 크기나 색상은 props 혹은 CSS 스타일로 변경 가능

    <br>

### 10.1.2 Prettier 설정

```jsx
{
  "singleQuote": true,
  "semi": true,
  "useTabs": false,
  "tabWidth": 2,
  "trailingComma": "all",
  "printWidth": 80
}
```

- 프로젝트 최상위 디렉터리에 `.prettierrc` 파일 생성 후 위와 같이 코드 작성
  - 깔끔하게 코드 스타일 정리 가능

<br>

### 10.1.3 index.css 수정

```css
body {
	margin: 0;
	padding: 0;
	background: #e9ecef;
}
```

<br>

### 10.1.4 App 컴포넌트 초기화

`App.js`

```jsx
import React from "react";

const App = () => {
	return <div>Todo App 만들기</div>;
};

export default App;
```

<br>

`index.js`

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
	<div>
		<App />
	</div>,
	document.getElementById("root")
);
```

<br>

## 10.2 UI 구성하기

#### 💡추후 만들 컴포넌트 종류

- **TodoTemplate**

  - 화면을 가운데에 정렬시키며, 앱 타이틀 (일정 관리) 을 보여줌.
  - children 으로 내부 JSX 를 props 로 받아 와서 렌더링
    <br>

- **TodoInsert**

  - 새로운 항목을 입력하고 추가할 수 있는 컴포넌트
  - state 를 통해 인풋의 상태를 관리
    <br>

- **TodoListItem**

  - 각 할 일 항목에 대한 정보를 보여 주는 컴포넌트
  - todo 객체를 props 로 받아 와서 상태에 따라 다른 스타일의 UI 를 보여줌.
    <br>

- **TodoList**
  - todos 배열을 props 로 받아 온 후, 이를 배열 내장 함수 map 을 사용해서 여러 개의 TodoListItem 컴포넌트로 변환해서 보여줌.
    <br>

### 10.2.1 TodoTemplate 만들기

<br>

`TodoTemplate.js`

```jsx
import React from "react";
import "./TodoTemplate.scss";

const TodoTemplate = ({ children }) => {
	return (
		<div>
			<div className='TodoTemplate'>
				<div className='app-title'>일정 관리</div>
				<div className='content'>{children}</div>
			</div>
		</div>
	);
};
export default TodoTemplate;
```

<br>

`TodoTemplate.scss`

```css
.TodoTemplate {
	width: 512px;
	background-color: black;
	margin-left: auto;
	margin-right: auto;
	border-radius: 4px;
	overflow: hidden;
}

.app-title {
	background: #22b8cf;
	color: white;
	height: 4rem;
	font-size: 1.5rem;
	display: flex;
	align-items: center;
	justify-content: center;
}

.content {
	background: white;
}
```

<br>

`App.js`

```jsx
import React from "react";
import TodoTemplate from "./components/TodoTemplate";

const App = () => {
	return <TodoTemplate>Todo App 만들자!</TodoTemplate>;
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/181862726-bf83abdd-7d31-4693-87e9-c8bebf533449.png)

<br>

### 10.2.2 TodoInsert 만들기

<br>

`TodoInsert.js`

```jsx
import React from "react";
import { MdAdd } from "react-icons/md";
import "./TodoInsert.scss";

const TodoInsert = () => {
	return (
		<form className='TodoInsert'>
			<input placeholder='할 일을 입력하세요' />
			<button type='submit'>
				<MdAdd />
			</button>
		</form>
	);
};

export default TodoInsert;
```

<br>

`TodoInsert.scss`

```css
.TodoInsert {
	display: flex;
	background: #495057;
	input {
		// 기본 스타일 초기화
		background: none;
		outline: none;
		border: none;
		padding: 0.5rem;
		font-size: 1.125rem;
		line-height: 1.5;
		color: white;
		&::placeholder {
			color: #dee2e6;
		}
		// 버튼을 제외한 영역을 모두 차지하기
		flex: 1;
	}
	button {
		// 기본 스타일 초기화
		background: none;
		outline: none;
		border: none;
		background: #868396;
		color: white;
		padding-left: 1rem;
		padding-right: 1rem;
		font-size: 1.5rem;
		display: flex;
		align-items: center;
		cursor: pointer;
		transition: 0.1s background ease-in;
		&:hover {
			background: #adb5bd;
		}
	}
}
```

<br>

`App.js`

```jsx
import React from "react";
import TodoInsert from "./components/TodoInsert";
import TodoTemplate from "./components/TodoTemplate";

const App = () => {
	return (
		<TodoTemplate>
			<TodoInsert />
		</TodoTemplate>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/181863502-cb81bc12-7dee-47ec-a259-8a7680e87108.png)

<br>

### 10.2.3 TodoListItem 과 TodoList 만들기

<br>

`TodoListItem.js`

```jsx
import React from "react";
import "./TodoListItem.scss";

import {
	MdCheckBoxOutlineBlank,
	// MdCheckBox,
	MdRemoveCircleOutline,
} from "react-icons/md";

const TodoListItem = () => {
	return (
		<div className='TodoListItem'>
			<div className='checkbox'>
				<MdCheckBoxOutlineBlank />
				<div className='text'>할 일</div>
			</div>
			<div className='remove'>
				<MdRemoveCircleOutline />
			</div>
		</div>
	);
};

export default TodoListItem;
```

<br>

`TodoListItem.scss`

```css
.TodoListItem {
	padding: 1rem;
	display: flex;
	align-items: center;
	&:nth-child(even) {
		background: #f8f9fa;
	}
	.checkbox {
		cursor: pointer;
		flex: 1;
		display: flex;
		align-items: center;
		svg {
			// 아이콘
			font-size: 1.5rem;
		}
		.text {
			margin-left: 0.5rem;
			flex: 1; // 차지할 수 있는 영역 모두 차지
		}

		// 체크되었을 때 보여 줄 스타일
		&.checked {
			svg {
				color: #22b8cf;
			}
			.text {
				color: #adb5bd;
				text-decoration: line-through;
			}
		}
	}
	.remove {
		display: flex;
		align-items: center;
		font-size: 1.5rem;
		color: #ff6b6b;
		cursor: pointer;
		&:hover {
			color: #ff8787;
		}
	}

	// 엘리먼트 사이사이에 테두리를 넣어 줌
	& + & {
		border-top: 1px solid #dee2e6;
	}
}
```

<br>

`TodoList.js`

```jsx
import React from "react";
import TodoListItem from "./TodoListItem";
import "./TodoList.scss";

const TodoList = () => {
	return (
		<div className='TodoList'>
			<TodoListItem />
			<TodoListItem />
			<TodoListItem />
		</div>
	);
};

export default TodoList;
```

<br>

`TodoList.scss`

```css
.TodoList {
	min-height: 320px;
	max-height: 513px;
	overflow-y: auto;
}
```

<br>

`App.js`

```jsx
import React from "react";
import TodoInsert from "./components/TodoInsert";
import TodoList from "./components/TodoList";
import TodoTemplate from "./components/TodoTemplate";

const App = () => {
	return (
		<TodoTemplate>
			<TodoInsert />
			<TodoList />
		</TodoTemplate>
	);
};

export default App;
```

<Br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/181865239-5dd5ef21-a8f6-4b97-aacf-0679945d0a93.png)

<br>

## 10.3 기능 구현하기

### 10.3.1 App 에서 todos 상태 사용하기

- 나중에 추가할 일정 항목에 대한 상태들은 모두 App 컴포넌트에서 관리

  - useState 를 사용하여 todos 라는 상태를 정의하고, TodoList 의 props 로 전달
  - TodoList 에서 todos 객체를 받아와서 TodoListItem 으로 변환하여 렌더링하도록 설정
    <br>

`App.js`

```jsx
import React, { useState } from "react";
import TodoInsert from "./components/TodoInsert";
import TodoList from "./components/TodoList";
import TodoTemplate from "./components/TodoTemplate";

const App = () => {
	// eslint-disable-next-line
	const [todos, setTodos] = useState([
		{
			id: 1,
			text: "리액트의 기초 알아보기",
			checked: true,
		},
		{
			id: 2,
			text: "컴포넌트 스타일링해 보기",
			checked: true,
		},
		{
			id: 3,
			text: "일정 관리 앱 만들어 보기",
			checked: false,
		},
	]);

	return (
		<TodoTemplate>
			<TodoInsert />
			<TodoList todos={todos} />
		</TodoTemplate>
	);
};

export default App;
```

- todos 배열 안에 들어 있는 객체
  - `id`
    - ListItem 에서 map 함수 사용을 위한 각 항목의 고유 id
  - `text`
    - 내용
  - `checked`
    - 완료 여부를 알려 주는 값

<Br>

`TodoList.js`

```jsx
import React from "react";
import TodoListItem from "./TodoListItem";
import "./TodoList.scss";

const TodoList = ({ todos }) => {
	return (
		<div className='TodoList'>
			{todos.map((todo) => (
				<TodoListItem todo={todo} key={todo.id} />
			))}
		</div>
	);
};

export default TodoList;
```

- props 로 받아 온 todos 배열을 map 을 통해 TodoListItem 으로 이루어진 배열로 변환하여 렌더링

  - map 을 사용하여 컴포넌트로 변환할 때는 key props 반드시 전달
  - 여기서 사용되는 key 값은 각 항목마다 가지고 있는 고윳값인 id 로 지정
    <br>

- todo 데이터는 통째로 props 로 전달해 주는 것이 나중에 성능 최적화 할 때 편리

<br>

`TodoListItem.js`

```jsx
import React from "react";
import "./TodoListItem.scss";
import cn from "classnames";
import { MdCheckBoxOutlineBlank, MdCheckBox, MdRemoveCircleOutline } from "react-icons/md";

const TodoListItem = ({ todo }) => {
	const { text, checked } = todo;

	return (
		<div className='TodoListItem'>
			<div className={cn("checkbox", { checked })}>
				{checked ? <MdCheckBox /> : <MdCheckBoxOutlineBlank />}
				<div className='text'>{text}</div>
			</div>
			<div className='remove'>
				<MdRemoveCircleOutline />
			</div>
		</div>
	);
};

export default TodoListItem;
```

- TodoList 에서 받아 온 todo 값에 따라 보여줄 UI 를 달리 하기 위해 classnames 를 통해 조건부 스타일링 구현

<Br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/181865844-fdd7dd8b-c19a-48a1-9dc5-af18e29c14aa.png)

<Br>

### 10.3.2 항목 추가 기능 구현하기

- 일정 항목을 추가하는 기능
  - TodoInsert 컴포넌트에서 인풋 상태를 관리
  - App 컴포넌트에는 todos 배열에 새로운 객체를 추가하는 함수 생성

<br>

#### 10.3.2.1 TodoInsert value 상태 관리하기

- TodoInsert 컴포넌트에서 인풋에 입력하는 값을 관리할 수 있도록 useState 를 사용하여 value 라는 상태를 정의
- 인풋에 넣어 줄 onChange 함수는 useCallback Hook 을 사용하여 한 번 선언 후 재사용할 수 있도록 구현
  <br>

`TodoInsert.js`

```jsx
import React, { useCallback, useState } from "react";
import { MdAdd } from "react-icons/md";
import "./TodoInsert.scss";

const TodoInsert = () => {
	const [value, setValue] = useState("");
	const onChange = useCallback((e) => {
		setValue(e.target.value);
	}, []);

	return (
		<form className='TodoInsert'>
			<input placeholder='할 일을 입력하세요' value={value} onChange={onChange} />
			<button type='submit'>
				<MdAdd />
			</button>
		</form>
	);
};

export default TodoInsert;
```

<br>

#### 10.3.2.2 리액트 개발자 도구

- 브라우저에 나타난 리액트 컴포넌트를 심층 분석할 수 있도록 하는 도구
- https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi/related?hl=ko 에서 설치 가능

<br>

#### 10.3.2.3 todos 배열에 새 객체 추가하기

- App 컴포넌트에서 todos 배열에 새 객체를 추가하는 onInsert 함수 생성
  - 새로운 객체를 만들 때마다 id 값에 1씩 더해주는데, useState 가 아닌 useRef 사용
  - id 값은 렌더링 되는 정보가 아닌 그저 새로운 항목을 만들 때 참조되는 값이기 때문
    <br>

`App.js`

```jsx
import React, { useCallback, useState, useRef } from "react";
import TodoInsert from "./components/TodoInsert";
import TodoList from "./components/TodoList";
import TodoTemplate from "./components/TodoTemplate";

const App = () => {
	const [todos, setTodos] = useState([
		{
			id: 1,
			text: "리액트의 기초 알아보기",
			checked: true,
		},
		{
			id: 2,
			text: "컴포넌트 스타일링해 보기",
			checked: true,
		},
		{
			id: 3,
			text: "일정 관리 앱 만들어 보기",
			checked: false,
		},
	]);

	// 고윳값으로 사용될 id
	// ref 를 사용하여 변수 담기
	const nextId = useRef(4);
	const onInsert = useCallback(
		(text) => {
			const todo = {
				id: nextId.current,
				text,
				checked: false,
			};
			setTodos(todos.concat(todo));
			nextId.current += 1; // nextId 1씩 더하기
		},
		[todos]
	);

	return (
		<TodoTemplate>
			<TodoInsert onInsert={onInsert} />
			<TodoList todos={todos} />
		</TodoTemplate>
	);
};

export default App;
```

<br>

#### 10.3.2.4 TodoInsert 에서 onSubmit 이벤트 설정하기

- App 에서 TodoInsert 에 넣어 준 onInsert 함수에 현재 useState 를 통해 관리하고 있는 value 값을 파라미터로 넣어서 호출

<br>

`TodoInsert.js`

```jsx
import React, { useCallback, useState } from "react";
import { MdAdd } from "react-icons/md";
import "./TodoInsert.scss";

const TodoInsert = ({ onInsert }) => {
	const [value, setValue] = useState("");
	const onChange = useCallback((e) => {
		setValue(e.target.value);
	}, []);

	const onSubmit = useCallback(
		(e) => {
			onInsert(value); // App.js 의 onInsert() 인자 text 로 들어가게 되어 todos 배열 갱신
			setValue(""); // value 값 초기화
			// submit 이벤트는 브라우저에서 새로고침을 발생시키는데,
			// 이를 방지하기 위해 아래 함수 호출
			e.preventDefault();
		},
		[onInsert, value]
	);

	return (
		<form className='TodoInsert' onSubmit={onSubmit}>
			<input placeholder='할 일을 입력하세요' value={value} onChange={onChange} />
			<button type='submit'>
				<MdAdd />
			</button>
		</form>
	);
};

export default TodoInsert;
```

- onSubmit 함수가 호출되면 props 로 받아 온 onInsert 함수에 현재 value 값을 파라미터로 넣어서 호출하고, 현재 value 값을 초기화
- `e.preventDefault()` 함수를 호출하여 onSubmit 이벤트의 기본 동작인 브라우저 새로고침 방지

<Br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/181870927-698d5b6c-39be-435d-af19-f760e0227c1a.png)

<br>

### 10.3.3 지우기 기능 구현하기

- 리액트 컴포넌트에서 배열의 불변성을 지키면서 배열 원소를 제거해야 할 경우, 배열 내장 함수인 filter 사용하는 것이 편리
  <br>

#### 10.3.3.1 배열 내장 함수 filter

<br>

```jsx
const array = [1, 2, 3, 4, 5, 6, 7, 8, 9];
const biggerThanFive = array.filter((number) => number > 5);
// 결과 : [6, 7, 8, 9]
```

- filter 함수는 기존의 배열은 그대로 둔 상태에서 특정 조건을 만족하는 원소들만 따로 추출하여 새로운 배열을 생성
  - 조건을 확인해 주는 함수를 파라미터로 전달
  - 파라미터로 넣는 함수는 true 혹은 false 값을 반환
    - true 를 반환하는 경우만 새로운 배열에 포함됨.

<br>

#### 10.3.3.2 todos 배열에서 id 로 항목 지우기

`App.js`

```jsx
import React, { useCallback, useState, useRef } from "react";
import TodoInsert from "./components/TodoInsert";
import TodoList from "./components/TodoList";
import TodoTemplate from "./components/TodoTemplate";

const App = () => {
	const [todos, setTodos] = useState([
		{
			id: 1,
			text: "리액트의 기초 알아보기",
			checked: true,
		},
		{
			id: 2,
			text: "컴포넌트 스타일링해 보기",
			checked: true,
		},
		{
			id: 3,
			text: "일정 관리 앱 만들어 보기",
			checked: false,
		},
	]);

	// 고윳값으로 사용될 id
	// ref 를 사용하여 변수 담기
	const nextId = useRef(4);
	const onInsert = useCallback(
		(text) => {
			const todo = {
				id: nextId.current,
				text,
				checked: false,
			};
			setTodos(todos.concat(todo));
			nextId.current += 1; // nextId 1씩 더하기
		},
		[todos]
	);

	const onRemove = useCallback(
		(id) => {
			setTodos(todos.filter((todo) => todo.id !== id));
		},
		[todos]
	);

	return (
		<TodoTemplate>
			<TodoInsert onInsert={onInsert} />
			<TodoList todos={todos} onRemove={onRemove} />
		</TodoTemplate>
	);
};

export default App;
```

- filter 함수를 사용하여 onRemove 함수 생성
  - App 컴포넌트에 id 를 파라미터로 받아 와서 같은 id 를 가진 항목을 todos 배열에서 지워 주는 함수
    <br>

#### 10.3.3.3 TodoListItem 에서 삭제 함수 호출하기

<br>

`TodoList.js`

```jsx
import React from "react";
import TodoListItem from "./TodoListItem";
import "./TodoList.scss";

const TodoList = ({ todos, onRemove }) => {
	return (
		<div className='TodoList'>
			{todos.map((todo) => (
				<TodoListItem todo={todo} key={todo.id} onRemove={onRemove} />
			))}
		</div>
	);
};

export default TodoList;
```

- TodoListItem 에서 onRemove() 함수를 사용하려면 우선 TodoList 컴포넌트를 거쳐야 함.
  - props 로 받아 온 onRemove 함수를 TodoListItem 에 그대로 전달

<br>

`TodoListItem.js`

```jsx
import React from "react";
import "./TodoListItem.scss";
import cn from "classnames";
import { MdCheckBoxOutlineBlank, MdCheckBox, MdRemoveCircleOutline } from "react-icons/md";

const TodoListItem = ({ todo, onRemove }) => {
	const { id, text, checked } = todo;

	return (
		<div className='TodoListItem'>
			<div className={cn("checkbox", { checked })}>
				{checked ? <MdCheckBox /> : <MdCheckBoxOutlineBlank />}
				<div className='text'>{text}</div>
			</div>
			<div className='remove' onClick={() => onRemove(id)}>
				<MdRemoveCircleOutline />
			</div>
		</div>
	);
};

export default TodoListItem;
```

- 삭제 버튼을 누르면 TodoListItem 에서 onRemove 함수에 현재 자신이 가진 id 를 넣어서 삭제 함수를 호출하도록 설정
  <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/181870952-c4772f54-5e22-4a99-bf49-9ea631c39021.png)

<br>

### 10.3.4 수정 기능

- App 컴포넌트에 onToggle 함수를 생성하고, 해당 함수를 TodoList 컴포넌트에게 props 로 전달하여 TodoListItem 까지 전달

<br>

#### 10.3.4.1 onToggle 구현하기

<br>

`App.js`

```jsx
import React, { useCallback, useState, useRef } from "react";
import TodoInsert from "./components/TodoInsert";
import TodoList from "./components/TodoList";
import TodoTemplate from "./components/TodoTemplate";

const App = () => {
	const [todos, setTodos] = useState([
		{
			id: 1,
			text: "리액트의 기초 알아보기",
			checked: true,
		},
		{
			id: 2,
			text: "컴포넌트 스타일링해 보기",
			checked: true,
		},
		{
			id: 3,
			text: "일정 관리 앱 만들어 보기",
			checked: false,
		},
	]);

	// 고윳값으로 사용될 id
	// ref 를 사용하여 변수 담기
	const nextId = useRef(4);
	const onInsert = useCallback(
		(text) => {
			const todo = {
				id: nextId.current,
				text,
				checked: false,
			};
			setTodos(todos.concat(todo));
			nextId.current += 1; // nextId 1씩 더하기
		},
		[todos]
	);

	const onRemove = useCallback(
		(id) => {
			setTodos(todos.filter((todo) => todo.id !== id));
		},
		[todos]
	);

	const onToggle = useCallback(
		(id) => {
			setTodos(todos.map((todo) => (todo.id === id ? { ...todo, checked: !todo.checked } : todo)));
		},
		[todos]
	);

	return (
		<TodoTemplate>
			<TodoInsert onInsert={onInsert} />
			<TodoList todos={todos} onRemove={onRemove} onToggle={onToggle} />
		</TodoTemplate>
	);
};

export default App;
```

- 배열 내장 함수 map 을 사용하여 특정 id 를 가지고 있는 객체의 checked 값을 반전시킴.
- 변화가 필요한 원소만 업데이트되고, 나머지는 그대로 남아있게 하기 위해 map 을 통해 배열 전체를 순회하며 검사하는 것
  <br>

#### 10.3.4.2 TodoListItem 에서 토글 함수 호출하기

<br>

`TodoList.js`

```jsx
import React from "react";
import TodoListItem from "./TodoListItem";
import "./TodoList.scss";

const TodoList = ({ todos, onRemove, onToggle }) => {
	return (
		<div className='TodoList'>
			{todos.map((todo) => (
				<TodoListItem todo={todo} key={todo.id} onRemove={onRemove} onToggle={onToggle} />
			))}
		</div>
	);
};

export default TodoList;
```

- App 에서 생성한 onToggle 함수를 TodoListItem 에서도 호출할 수 있도록 TodoList 를 거쳐 전달

<Br>

`TodoListItem.js`

```jsx
import React from "react";
import "./TodoListItem.scss";
import cn from "classnames";
import { MdCheckBoxOutlineBlank, MdCheckBox, MdRemoveCircleOutline } from "react-icons/md";

const TodoListItem = ({ todo, onRemove, onToggle }) => {
	const { id, text, checked } = todo;

	return (
		<div className='TodoListItem'>
			<div className={cn("checkbox", { checked })} onClick={() => onToggle(id)}>
				{checked ? <MdCheckBox /> : <MdCheckBoxOutlineBlank />}
				<div className='text'>{text}</div>
			</div>
			<div className='remove' onClick={() => onRemove(id)}>
				<MdRemoveCircleOutline />
			</div>
		</div>
	);
};

export default TodoListItem;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/181871033-70bde4d7-d6c2-4608-8a4f-453046a16787.png)
