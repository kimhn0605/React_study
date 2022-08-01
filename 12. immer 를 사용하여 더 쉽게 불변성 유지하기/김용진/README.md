# 12 immer를 사용하여 더 쉽게 불변성 유지하지

## 12.1 immer를 사용하여 더 쉽게 불변성 유지하기

### 12.1.3 immer 사용법

```js
import produce from 'immer';
const nextState = produce(originalState, (draft) => {
	// 바꾸고 싶은 값 바꾸기
	draft.somewhere.deep.inside = 5;
});
```

`produce`라는 함수는 두 가지 파라미터를 받음

- 첫 번째 파라미터는 수정하고 싶은 상태
- 상태를 어떻게 업데이트할지 정의하는 함수
  - 함수 내부에서 원하는 값을 변경하면, produce 함수가 불변성 유지를 대신해주면서 새로운 상태를 생성

> 👉 불변성에 신경 쓰지 않는 것처럼 코드를 작성하되 불변성 관리를 제대로 해주는 것

```js
import immer from 'immer';

const originalState = [
	{ id: 1, todo: '전개 연산자와 배열 함수로 불변성 유지하기', checked: true },
	{ id: 2, todo: 'immer로 불변성 유지하기', checked: false },
];

const nextState = produce(orginalState, (draft) => {
	// id가 2인 항목의 checked 값을 true로 설정
	const todo = draft.find((t) => t.id === 2); // id로 항목 찾기
	todo.checked = true;
	// 혹은 draft[1].checked = true;

	// 배열에 새로운 데이터 추가
	draft.push({ id: 3, tood: '일정 관리 앱에 immer 적용하기', checked: false });

	// id = 1인 항목을 제거
	draft.splice(
		draft.findIndex((t) => t.ud === 1),
		1
	);
});
```

> 👉 immer를 사용하여 컴포넌트 상태를 작성할 때는 객체 안에 있는 값을 직접 수정하거나, 배열에 직접적인 변화를 일으키는 push, splice 등의 함수를 사용해도 무방

### 12.1.5 useState의 함수형 업데이트와 immer 함께 쓰기

```js
const [number, setNumber] = useState(0);
// prevNumbers는 현재 number 값을 가리킴

const onIncrease = useCallback(
	() => setNumber((prevNumbrer) => prevNumber + 1),
	[]
);
```

> 👉 immer에서 제공하는 produce 함수를 호출할 때, 첫 번째 파라미터가 함수 형태라면 업데이트 함수를 반환

```jsx
import { useRef, useCallback, useState } from 'react';
import produce from 'immer';

const App = () => {
	const nextId = useRef(1);
	const [form, setForm] = useState({ name: '', username: '' });
	const [data, setData] = useState({
		array: [],
		uselessValue: null,
	});
	// input 수정을 위한 변수
	const onChange = useCallback((e) => {
		const { name, value } = e.target;
		setForm(
			produce((draft) => {
				draft[name] = value;
			})
		);
	}, []);

	// form 등록을 위한 함수
	const onSubmit = useCallback(
		(e) => {
			e.preventDefault();
			const info = {
				id: nextId.current,
				name: form.name,
				username: form.username,
			};

			// array에 새 항목 등록
			setData(
				produce((draft) => {
					draft.array.push(info);
				})
			);

			// form 초기화
			setForm({ name: '', username: '' });
			nextId.current += 1;
		},
		[form.name, form.username]
	);

	// 항목을 삭제하는 함수
	const onRemove = useCallback((id) => {
		setData(
			produce((draft) => {
				draft.array.splice(
					draft.array.findIndex((info) => info.id === id),
					1
				);
			})
		);
	});
	return;
};

export default App;
```
