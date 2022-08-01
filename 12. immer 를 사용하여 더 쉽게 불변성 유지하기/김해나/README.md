# 12. immer 를 사용하여 더 쉽게 불변성 유지하기

## 12.1 immer 를 설치하고 사용법 알아보기

### 12.1.1 프로젝트 준비

<br>

```python
npm install immer
```

- immer 라이브러리 설치

<br>

#### 12.1.2 immer 를 사용하지 않고 불변성 유지

<br>

`App.js`

```jsx
import React, { useRef, useState, useCallback } from "react";

const App = () => {
	const nextId = useRef(1);
	const [form, setForm] = useState({ username: "", name: "" });
	const [data, setData] = useState({
		array: [],
		uselessValue: null,
	});

	// input 수정을 위한 함수
	const onChange = useCallback(
		(e) => {
			const { name, value } = e.target;
			setForm({
				...form,
				[name]: [value],
			});
		},
		[form]
	);

	// form 등록을 위한 함수
	const onSubmit = useCallback(
		(e) => {
			e.preventDefault();
			const info = {
				id: nextId.current,
				name: form.name,
				username: form.username,
			};

			// array 에 새 항목 등록 (불변성 유지를 위해 전개 연산자 사용)
			setData({
				...data,
				array: data.array.concat(info),
			});

			// form 초기화
			setForm({
				name: "",
				username: "",
			});
			nextId.current += 1;
		},
		[data, form.name, form.username]
	);

	// 항목을 삭제하는 함수
	const onRemove = useCallback(
		(id) => {
			setData({
				...data,
				array: data.array.filter((info) => info.id !== id),
			});
		},
		[data]
	);

	return (
		<div>
			<form onSubmit={onSubmit}>
				<input name='username' placeholder='아이디' value={form.username} onChange={onChange} />
				<input name='name' placeholder='이름' value={form.name} onChange={onChange} />
				<button type='submit'>등록</button>
			</form>
			<div>
				<ul>
					{data.array.map((info) => (
						<li key={info.id} onClick={() => onRemove(info.id)}>
							{info.username} ({info.name})
						</li>
					))}
				</ul>
			</div>
		</div>
	);
};

export default App;
```

- 폼에서 아이디/이름을 입력하면 하단 리스트에 추가되고, 리스트 항목을 클릭하면 삭제되는 컴포넌트
  <br>

- 전개 연산자와 배열의 내장 함수를 사용하면 간단하게 배열 혹은 객체를 복사하고 새로운 값을 덮어 쓸 수 있음.
  - 하지만 객체의 구조가 엄청나게 깊어지면 불변성을 유지하면서 업데이트하는 것이 매우 힘들어짐.

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/182057894-b92bf163-ddaf-47b5-8956-6b771d49522d.png)

<br>

#### 12.1.3 immer 사용법

- immer 를 사용하면 불변성을 유지하는 작업을 매우 간단하게 처리 가능
- 이 라이브러리의 핵심은 <em> 불변성에 신경 쓰지 않는 것처럼 코드를 작성하되, 불변성 관리는 제대로 해 주는 것 </em>

<br>

```jsx
import produce from "immer";
const nextState = produce(originalState, (draft) => {
	// 바꾸고 싶은 값 변경
	draft.somewhere.deep.inside = 5;
});
```

- produce 함수는 2가지 파라미터를 가짐.
  - `originalState` : 수정하고 싶은 상태
  - `draft` : 상태를 어떻게 업데이트할지 정의하는 함수 - draft 함수 내부에서 원하는 값을 변경하면, produce 함수가 불변성 유지를 대신해 주면서 새로운 상태를 생성
    <Br>

#### 12.1.4 App 컴포넌트에 immer 적용하기

<br>

`App.js`

```jsx
import React, { useRef, useState, useCallback } from "react";
import produce from "immer";

const App = () => {
	const nextId = useRef(1);
	const [form, setForm] = useState({ username: "", name: "" });
	const [data, setData] = useState({
		array: [],
		uselessValue: null,
	});

	// input 수정을 위한 함수
	const onChange = useCallback(
		(e) => {
			const { name, value } = e.target;
			setForm(
				produce(form, (draft) => {
					draft[name] = value;
				})
			);
		},
		[form]
	);

	// form 등록을 위한 함수
	const onSubmit = useCallback(
		(e) => {
			e.preventDefault();
			const info = {
				id: nextId,
				name: form.name,
				username: form.username,
			};

			// array 에 새 항목 등록 (불변성 유지를 위해 전개 연산자 사용)
			setData(
				produce(data, (draft) => {
					draft.array.push(info);
				})
			);

			// form 초기화
			setForm({
				name: "",
				username: "",
			});
			nextId.current += 1;
		},
		[data, form.name, form.username]
	);

	// 항목을 삭제하는 함수
	const onRemove = useCallback(
		(id) => {
			setData(
				produce(data, (draft) => {
					draft.array.splice(
						draft.array.findIndex((info) => info.id === id),
						1
					);
				})
			);
		},
		[data]
	);
	console.log(data);
	return (
		<div>
			<form onSubmit={onSubmit}>
				<input name='username' placeholder='아이디' value={form.username} onChange={onChange} />
				<input name='name' placeholder='이름' value={form.name} onChange={onChange} />
				<button type='submit'>등록</button>
			</form>
			<div>
				<ul>
					{data.array.map((info) => (
						<li key={info.id} onClick={() => onRemove(info.id)}>
							{info.username} ({info.name})
						</li>
					))}
				</ul>
			</div>
		</div>
	);
};

export default App;
```

- immer 를 사용하여 컴포넌트 상태를 작성할 때는
  - 객체 안에 있는 값을 직접 수정하거나, 배열에 직접적인 변화를 일으키는 push, splice 등의 함수를 사용해도 무방

<br>

#### 12.1.5 useState 의 함수형 업데이트와 immer 함께 쓰기

<br>

```jsx
const [number, setNumber] = useState(0);

// prevNumbers 는 현재 number 값을 가리킴.
const onIncrease = useCallback(() => setNumber((prevNumber) => prevNumber + 1), []);
```

- useState 의 함수형 업데이트

<br>

```jsx
const update = produce((draft) => {
	draft.value = 2;
});

const originalState = {
	value: 1,
	foo: "bar",
};

const nextState = update(originalState);
console.log(nextState); // { value: 2, foo: 'bar' }
```

- immer 에서 제공하는 produce 함수를 호출할 때,
  - 첫 번째 파라미터가 함수 형태라면 업데이트 함수를 반환

<br>

`App.js`

```jsx
import React, { useRef, useState, useCallback } from "react";
import produce from "immer";

const App = () => {
	const nextId = useRef(1);
	const [form, setForm] = useState({ username: "", name: "" });
	const [data, setData] = useState({
		array: [],
		uselessValue: null,
	});

	// input 수정을 위한 함수
	const onChange = useCallback(
		(e) => {
			const { name, value } = e.target;
			setForm(
				produce((draft) => {
					draft[name] = value;
				})
			);
		},
		[form]
	);

	// form 등록을 위한 함수
	const onSubmit = useCallback(
		(e) => {
			e.preventDefault();
			const info = {
				id: nextId.current,
				name: form.name,
				username: form.username,
			};

			// array 에 새 항목 등록 (불변성 유지를 위해 전개 연산자 사용)
			setData(
				produce((draft) => {
					data.array.push(info);
				})
			);

			// form 초기화
			setForm({
				name: "",
				username: "",
			});
			nextId.current += 1;
		},
		[data, form.name, form.username]
	);

	// 항목을 삭제하는 함수
	const onRemove = useCallback(
		(id) => {
			setData(
				produce((draft) => {
					draft.array.splice(
						draft.array.findIndex((info) => info.id === id),
						1
					);
				})
			);
		},
		[data]
	);

	return (
		<div>
			<form onSubmit={onSubmit}>
				<input name='username' placeholder='아이디' value={form.username} onChange={onChange} />
				<input name='name' placeholder='이름' value={form.name} onChange={onChange} />
				<button type='submit'>등록</button>
			</form>
			<div>
				<ul>
					{data.array.map((info) => (
						<li key={info.id} onClick={() => onRemove(info.id)}>
							{info.username} ({info.name})
						</li>
					))}
				</ul>
			</div>
		</div>
	);
};

export default App;
```

- immer 의 속성과 useState 의 함수형 업데이트를 함께 활용하면 코드를 더욱 깔끔하게 작성 가능
