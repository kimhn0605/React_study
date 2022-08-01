# 11. 컴포넌트 성능 최적화

## 11.5 onToggle, onRemove 함수가 바뀌지 않게 하기

### 11.5.1 useState의 함수형 업데이트

#### AS-IS

```js
const onInsert = useCallback(
	(text) => {
		const todo = { id: nextId.current, text, checked: false };
		setTodos(todos.concat(todo));
		nextId.current += 1;
	},
	[todos]
);
```

#### TO-BE

```js
const onInsert = useCallback((text) => {
	const todo = { id: nextId.current, text, checked: false };
	setTodos((todos) => todos.concat(todo));
	nextId.current += 1;
}, []);
```

### 11.5.2 useReducer 사용하기

```jsx
function todoReducer(todos, action) {
	switch (action.type) {
		case 'INSERT':
			return todos.concat(action.todo);
		default:
			return todos;
	}
}

const App = () => {
	const [todos, dispatch] = useReducer(todoReducer, undefined, createBulkTodos);
	const nextId = useRef(2501);

	const onInsert = useCallback((text) => {
		const todo = { id: nextId.current, text, check: false };
		dispatch({ type: 'INSERT', todo });
		nextId.current += 1;
	});
};
```

## 11.6 불변성의 중요성

> 👉 리액트 컴포넌트에서 상태를 업데이트할 때 불변성을 지키는 것은 매우 중요

업데이트가 필요한 곳에서는 아예 새로운 배열 혹은 새로운 객체를 만들기 때문에, React.memo를 사용했을 때 props가 바뀌었는지 혹은 바뀌지 않았는지를 알아내서 리렌더링 성능의 최적화 가능

## 11.8 react-virtualized를 사용한 렌더링 최적화

> 👉 react-virtualized를 사용하면 리스트 컴포넌트에서 스크롤되기 전에 보이지 않는 컴포넌트는 렌더링하지 않고 크기만 차지하게끔 할 수 있음
