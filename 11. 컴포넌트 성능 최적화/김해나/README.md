# 11. 컴포넌트 성능 최적화

## 11.1 많은 데이터 렌더링하기

`App.js`

```jsx
import React, { useCallback, useState, useRef } from 'react';
import TodoInsert from './TodoInsert';
import TodoList from './TodoList';
import TodoTemplate from './TodoTemplate';

function createBulkTodos() {
  const array = [];
  for (let i = 0; i <= 2500; i++) {
    array.push({
      id: 1,
      text: `할 일 ${i}`,
      checked: false,
    });
  }
  return array;
}

const App = () => {
  const [todos, setTodos] = useState(createBullkTodos);

  // 고윳값으로 사용될 id
  // ref 를 사용하여 변수 담기
  const nextId = useRef(2501);

 (...)
};
export default App;

```

- `useState(createBulkTodos)`
  - createBulkTodos 함수를 임의로 만들어서 2,500개의 데이터를 자동으로 생성
  - useState 파라미터에 함수 형태로 넣어주면 컴포넌트가 처음 렌더링될 때만 함수 실행 가능

<br>

## 11.2 크롬 개발자 도구를 통한 성능 모니터링

- 크롬 개발자 도구의 `Performance` 탭에서 성능 분석 결과 확인 가능

<br>

## 11.3 느려지는 원인 분석

- **컴포넌트는 다음과 같은 상황에서 리렌더링 발생**

  - 자신이 전달받은 props 가 변경될 때
  - 자신의 state 가 바뀔 때
  - 부모 컴포넌트가 리렌더링될 때
  - forceUpdate 함수가 실행될 때
    <br>

- '할 일 1' 항목을 체크할 경우 App 컴포넌트의 state 가 변경되면서 컴포넌트 전체가 리렌더링되고 있어 속도가 느려진 것.
  - 리렌더링이 불필요할 때는 리렌더링을 방지해줘야 함.

<br>

## 11.4 React.memo 를 사용하여 컴포넌트 성능 최적화

- 컴포넌트의 리렌더링을 방지할 때는 `shouldComponentUpdate` 라이프사이클 사용

  - 하지만, 함수형 컴포넌트에서는 라이프사이클 메서드 사용 X
  - 대신 React.memo 함수 사용
    <br>

- `React.memo()`
  - 컴포넌트의 props 가 바뀌지 않았다면 리렌더링 X
  - 컴포넌트를 만들고 나서 감싸 주기만 하면 사용 가능

<br>

`TodoListItem.js`

```jsx
import React from 'react';
import './TodoListItem.scss';
import cn from 'classnames';
import {
  MdCheckBoxOutlineBlank,
  MdCheckBox,
  MdRemoveCircleOutline,
} from 'react-icons/md';

const TodoListItem = ({ todo, onRemove, onToggle }) => {
  (...)
};

export default React.memo(TodoListItem);
```

<br>

## 11.5 onToggle, onRemove 함수가 바뀌지 않게 하기

- React.memo 를 사용하는 것만으로는 컴포넌트가 완벽하게 최적화되지 않음.
  - 현재 todos 배열이 업데이트되면 onRemove 와 onToggle 함수도 새롭게 바뀌기 때문
    <br>

`App.js`

```jsx
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
```

- onRemove 와 onToggle 함수는 배열 상태를 업데이트하는 과정에서 최신 상태의 todos 를 참조하기 때문에 todos 배열이 바뀔 때마다 함수가 새로 만들어짐.
  - 이렇게 함수가 계속 만들어지는 상황을 방지하려면 다음 2가지 방법 이용
    - `useState 의 함수형 업데이트 기능 사용`
    - `useReducer 사용`

<br>

### 11.5.1 useState 의 함수형 업데이트

<br>

**※ 함수형 업데이트**

- 새로운 상태를 파라미터로 넣는 대신, 상태 업데이트를 어떻게 할지 정의해 주는 업데이트 함수를 넣는 형태
  <br>

```jsx
const [number, setNumber] = useState(0);
const onIncrease = useCallback(() => setNumber((prevNumber) => prevNumber + 1), []);
```

- setNumber(number + 1) 이 아닌, 어떻게 업데이트할지 정의해 주는 업데이트 함수를 넣어주면
  - useCallback 을 사용할 때 두 번째 파라미터로 넣는 배열에 number 을 넣지 않아도 됨.

<br>

`App.js`

```jsx
import React, { useCallback, useState, useRef } from "react";
import TodoInsert from "./TodoInsert";
import TodoList from "./TodoList";
import TodoTemplate from "./TodoTemplate";

function createBulkTodos() {
	const array = [];
	for (let i = 0; i <= 2500; i++) {
		array.push({
			id: 1,
			text: `할 일 ${i}`,
			checked: false,
		});
	}
	return array;
}

const App = () => {
	const [todos, setTodos] = useState(createBulkTodos);
	const nextId = useRef(4); // 고윳값으로 사용될 id

	const onInsert = useCallback((text) => {
		const todo = {
			id: nextId.current,
			text,
			checked: false,
		};

		// 함수형 업데이트 적용
		setTodos((todos) => todos.concat(todo));
		nextId.current += 1;
	}, []);

  const onRemove = useCallback(id => {
    // 함수형 업데이트 적용
    setTodos(todos => todos.filter(todo => todo.id !== id));
  }, []);

  const onToggle = useCallback(id => {
    // 함수형 업데이트 적용
    setTodos(todos => todos.map(todo => todo.id === id ? {...todo, checked: !todo.checked } : todo,),
  }, []);

  return (
    <TodoTemplate>
      <TodoInsert onInsert={onInsert} />
      <TodoList todos={todos} onRemove={onRemove} onToggle={onToggle} />
    </TodoTemplate>
  );
};
```

- onInsert, onRemove, onToggle 함수에서 useState 의 함수형 업데이트 사용
  - setTodos 사용 시 `todos =>` 만 앞에 넣어주면 됨.
    <Br>

### 11.5.2 useRedecer

`App.js`

```jsx
import React, { useCallback, useState, useRef } from "react";
import TodoInsert from "./TodoInsert";
import TodoList from "./TodoList";
import TodoTemplate from "./TodoTemplate";

function createBulkTodos() {
	const array = [];
	for (let i = 0; i <= 2500; i++) {
		array.push({
			id: 1,
			text: `할 일 ${i}`,
			checked: false,
		});
	}
	return array;
}

function todoReducer(todos, action) {
	switch (action.type) {
		case "INSERT": // 새로 추가
			// { type : 'INSERT', todo: { id: 1, text: 'todo', checked: false } }
			return todos.concat(action.todo);
		case "REMOVE": // 제거
			// { type : 'REMOVE', id: 1 }
			return todos.filter((todo) => todo.id !== action.id);
		case "TOGGLE": // 토글
			// { type : 'TOGGLE', id: 1 }
			return todos.filter((todo) =>
				todo.id === action.id ? { ...todo, checked: !todo.checked } : todo
			);
		default:
			return todos;
	}
}

const App = () => {
  const [todos, dispatch] = useReducer(todoReducer, undefined, createBulkTodos);
  const nextId = useRef(2501);

  const onInsert = useCallback(text => {
    const todo = {
      id: nextId.current;
      text,
      checked: false,
    };
    dispatch({ type: 'INSERT', todo });
    nextId.current += 1;
  }, []);

  const onRemove = useCallback(id => {
    dispatch({ type: 'REMOVE', id });
  }, []);

  const onToggle = useCallback(id => {
    dispatch({ type: 'TOGGLE', id });
  }, []);

  return (
    <TodoTemplate>
      <TodoInsert onInsert={onInsert} />
      <TodoList todos={todos} onRemove={onRemove} onToggle={onToggle} />
    </TodoTemplate>
  );
}

export default App;
```

- 원래 useReducer() 의 두 번째 파라미터에는 초기 상태를 넣어줘야 하지만,
  - 지금은 그 대신 두 번째 파라미터에 undefined 를 넣고, 세 번째 파라미터에 초기 상태를 만들어 주는 createBulkTodos() 함수를 넣어줌.
  - 이렇게 하면 컴포넌트가 맨 처음 렌더링될 때만 createBulkTodos 함수가 호출됨.
    <br>

## 11.6 불변성의 중요성

- 기존 데이터를 수정할 때 직접 수정하지 않고, 새로운 배열을 만든 다음에 새로운 객체를 만들어서 필요한 부분을 교체해주는 방식으로 구현
  - 업데이트가 필요한 곳에서는 아예 새로운 배열 혹은 객체를 만들기 때문에 React.memo 사용하면 props 가 바뀌었는지 알아내서 리렌더링 성능 최적화 가능

<br>

```jsx
const todos = [
	{ id: 1, checked: true },
	{ id: 2, checked: true },
];
const nextTodos = [...todos];

nextTodos[0].checked = false;
console.log(todos[0] === nextTodos[0]); // 똑같은 객체를 가리키고 있기 때문에 true

nextTodos[0] = {
	...nextTodos[0],
	checked: false,
};
console.log(todos[0] === nextTodos[0]); // 새로운 객체를 할당해줬기 때문에 false
```

- 불변성이 지켜지지 않으면 객체 내부의 값이 새로워져도 감지 X

  - React.memo 에서 서로 비교하여 최적화하는 것이 불가능해짐.
    <br>

- 전개 연산자 (...) 를 사용하여 객체나 배열 내부의 값을 복사할 때는 얕은 복사 수행
  - 내부의 값이 완전히 새로 복사되는 것이 아닌 가장 바깥쪽에 있는 값만 복사되는 것.
  - 따라서, 내부의 값이 객체 혹은 배열이라면 내부의 값 또한 따로 복사해줘야 함.
    <br>

## 11.7 TodoList 컴포넌트 최적화하기

- 리스트에 관련된 컴포넌트를 최적화할 때는
  - 리스트 내부에서 사용하는 컴포넌트도 최적화해야 하고,
  - 리스트로 사용되는 컴포넌트 자체도 최적화해 주는 것이 좋음.
    <br>

`TodoList.js`

```jsx
import React from 'react';
import TodoListItem from './TodoListItem';
import './TodoList.scss';

const TodoList = ({ todos, onRemove, onToggle }) => {
  return (
    ...
  );
};

export default TodoList;
```

- 위 최적화 코드는 현재 프로젝트 성능에 전혀 영향 X
  - todos 배열이 업데이트될 때만 App 컴포넌트가 리렌더링되기 때문에
  - 현재 TodoList 컴포넌트는 불필요한 리렌더링이 발생하지 않음.
    <br>

## 11.8 react-virtualized 를 사용한 렌더링 최적화

- 일정 관리 애플리케이션에 초기 데이터가 2,500 개 등록되어 있는데, 실제 화면에 나오는 항목은 9개뿐
  - 초기 화면에 보이는 항목 제외한 나머지 컴포넌트는 스크롤하기 전에는 보이지 않음에도 불구하고 렌더링 발생
    <br>

#### 💡**react-virtualized 라이브러리**

```python
npm install react-virtualized
```

- 리스트 컴포넌트에서 스크롤되기 전에 보이지 않는 컴포넌트는 렌더링하지 않고 크기만 차지하게끔
- 만약 스크롤되면 해당 스크롤 위치에서 보여 주어야 할 컴포넌트를 자연스럽게 렌더링 가능
- react-virtualized 에서 제공하는 List 컴포넌트를 사용하여 TodoList 컴포넌트의 성능 최적화 가능
  - 최적화를 수행하기 전에 각 항목의 실제 크기를 px 단위로 알아내야 함. (개발자 도구 이용)
    <br>

`TodoList.js`

```jsx
import React from "react";
import { List } from "react-virtualized";
import TodoListItem from "./TodoListItem";
import "./TodoList.scss";

const TodoList = ({ todos, onRemove, onToggle }) => {
	const rowRenderer = useCallback(
		({ index, key, style }) => {
			const todo = todos[index];
			return (
				<TodoListItem
					todo={todo}
					key={todo.id}
					onRemove={onRemove}
					onToggle={onToggle}
					style={style}
				/>
			);
		},
		[onRemove, onToggle, todos]
	);

	return (
		<List
			className='TodoList'
			width={512} // 전체 크기
			height={513} // 전체 높이
			rowCount={todos.length} // 항목 개수
			rowHeight={57} // 항목 높이
			rowRenderer={rowRenderer} // 항목을 렌더링할 때 쓰는 함수
			list={todos} // 배열
			style={{ outline: "none" }} // List 에 기본 적용되는 outline 스타일 제거
		/>
	);
};

export default Reace.memo(TodoList);
```

- `rowRenderer()`

  - react-virtualized 의 List 컴포넌트에서 각 TodoItem 을 렌덜이할 때 사용
  - 이 함수를 List 컴포넌트의 props 로 설정해줘야 함.
  - 파라미터에 index, key, style 값을 객체 타입으로 받아 와서 사용
    <br>

- List 컴포넌트를 사용할 때는 다음 항목들을 props 로 전달
  - 해당 리스트의 전체 크기
  - 각 항목의 높이
  - 각 항목을 렌더링할 때 사용해야 하는 함수
  - 배열
    <br>

`TodoListItem.js`

```jsx
import React from "react";
import "./TodoListItem.scss";
import cn from "classnames";
import { MdCheckBoxOutlineBlank, MdCheckBox, MdRemoveCircleOutline } from "react-icons/md";

const TodoListItem = ({ todo, onRemove, onToggle, style }) => {
	const { id, text, checked } = todo;

	return (
		<div className='TodoListItem-virtualized' style={style}>
			<div className='TodoListItem'>
				<div className={cn("checkbox", { checked })} onClick={() => onToggle(id)}>
					{checked ? <MdCheckBox /> : <MdCheckBoxOutlineBlank />}
					<div className='text'>{text}</div>
				</div>
				<div className='remove' onClick={() => onRemove(id)}>
					<MdRemoveCircleOutline />
				</div>
			</div>
		</div>
	);
};

export default React.memo(
	TodoListItem,
	(prevProps, nextProps) => prevProps.todo === nextProps.todo
);
```

- 스타일 깨지는 것을 방지하기 위해 위와 같이 TodoListItem 컴포넌트 수정
  - render 함수에서 기존에 보여 주던 내용을 div 로 한 번 감싸고, - 해당 div 에는 TodoListItem-virtualized 라는 className 을 설정 - props 로 받아 온 style 을 적용
