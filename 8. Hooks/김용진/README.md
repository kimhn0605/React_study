# 8. Hooks

> 💡 Hooks는 리액트 v16.8에 새로 도입된 기능으로 함수 컴포넌트에서도 상태 관리를 할 수 있는 useState, 렌더링 직후 작업을 설정하는 useEffect 등의 기능을 제공하여 기존의 함수 컴포넌트에서 할 수 없었던 다양한 작업을 할 수 있게 해줌

## 8.1 useState

> 💡 함수 컴포넌트에서도 가변적인 상태를 지닐 수 있게 함

```jsx
const [value, setValue] = useState(0);
```

## 8.2 useEffect

> 💡 리액트 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook

👉 컴포넌트의 `componentDidMount`와 `componentDidUpdate`를 합친 형태로 보아도 됨

### 8.2.1 마운트될 때만 실행하고 싶을 때

```jsx
useEffect(() => {
	console.log('마운트될 때만 실행됩니다.');
}, []);
```

### 8.2.2 특정 값이 업데이트될 때만 실행하고 싶을 때

특정 값이 변경될 때만 호출하고 싶을 경우, 클래스형 컴포넌트라면 다음과 같이 작성

```jsx
componentDidUpdate(prevProps,prevState){
    if(prevProps.value !== this.props.value){
        doSomething();
    }
}
```

함수형 컴포넌트의 경우 다음과 같이 작성

```jsx
useEffect(() => {
	console.log(name);
}, [name]);
```

### 8.2.3 뒷정리하기

> 💡 useEffect는 기본적으로 렌더링되고 난 직후마다 실행되며, 두 번째 파라미터 배열에 무엇을 넣는지에 따라 실행되는 조건이 달라짐

👉 컴포넌트가 언마운트되기 전이나 업데이트 직전에 어떠한 작업을 수행하고 싶다면 useEffect에서 뒷정리 함수를 반환

```jsx
useEffect(() => {
	console.log('effect');
	console.log(name);

	return () => {
		console.log('cleanup');
		console.log(name);
	};
}, [name]);
```

## 8.3 useReducer

> 💡 useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트할 때 사용하는 Hook

**Reducer**

- 현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션 값을 전달받아 새로운 상태를 반환하는 함수
- 새로운 상태를 만들 때는 반드시 불변성을 지켜야 함

### 8.3.1 카운터 구현하기

```jsx
import React, { useReducer } from 'react';

function reducer(state, action) {
	// action.type에 따라 다른 작업 수행
	switch (action.type) {
		case 'INCREMENT':
			return {
				value: state.value + 1,
			};
		case 'DECREMENT':
			return {
				value: state.value - 1,
			};
		default:
			// 아무것도 해당되지 않을 때 기존 상태 변환
			return state;
	}
}

const Counter = () => {
	const [state, dispatch] = useReducer(reducer, { value: 0 });

	return (
		<div>
			<p>
				현재 카운터 값은 <b>{state.value}</b>
			</p>
			<button onClick={() => dispatch({ type: 'INCREMENT' })}>+1</button>
			<button onClick={() => dispatch({ type: 'DECREMENT' })}>-1</button>
		</div>
	);
};
```

- useReducer의 첫 번째 파라미터에는 리듀서 함수를 넣고, 두 번째 파라미터에는 해당 리듀서의 기본값을 넣음
- 해당 Hook을 사용하면 state값과 dispatch 함수를 받아옴
- state는 현재 가리키고 있는 상태, dispatch는 액션을 발생시키는 함수
- `dispatch(action)`과 같은 형태로, 함수 안에 파라미터로 액션 값을 넣어 주면 리듀서 함수가 호출되는 구조

### 8.3.2 인풋 상태 관리하기

```jsx
import React, { useReducer } from 'react';

function reducer(state, action) {
	return { ...state, [action.name]: action.value };
}

const Info = () => {
	const [state, dispatch] = useReducer(reducer, { name: '', nickname: '' });
	const { name, nickname } = state;
	const onChange = (e) => {
		dispatch(e.target);
	};

	return (
		<div>
			<div>
				<input name='name' value={name} onChange={onChange} />
				<input name='nickname' value={nickname} onChange={onChange} />
			</div>
			<div>
				<div>
					<b>이름: </b> {name}
				</div>
				<div>
					<b>닉네임: </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

## 8.4 useMemo

> 💡 함수 컴포넌트 내부에서 발생하는 연산을 최적화 가능

```jsx
import React, { useState } from 'react';

const getAverage = (numbers) => {
	console.log('평균값 계산 중...');
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((a, b) => a + b);
	return sum / numbers.length;
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState('');

	const onChange = (e) => {
		setNumber(e.target.value);
	};
	const onInsert = (e) => {
		const nextList = list.concat(parseInt(number));
		setList(nextList);
		setNumber('');
	};

	return (
		<div>
			<input value={number} onChange={onChange} />
			<button onClick={onInsert}>등록</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>평균값: </b>
				{getAverage(list)}
			</div>
		</div>
	);
};

export default Average;
```

👉 숫자를 등록할 때 뿐 아니라 인풋 내용이 수정될 때도 우리가 만든 getAverage 함수가 호출됨

- 인풋 내용이 바뀔 때는 평균 값을 다시 계산할 필요가 없는데, 렌더링할 때마다 계산하는 것은 낭비

```jsx
import React, { useState } from 'react';

const getAverage = (numbers) => {
	console.log('평균값 계산 중...');
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((a, b) => a + b);
	return sum / numbers.length;
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState('');

	const onChange = (e) => {
		setNumber(e.target.value);
	};
	const onInsert = (e) => {
		const nextList = list.concat(parseInt(number));
		setList(nextList);
		setNumber('');
	};

	const avg = useMemo(() => getAverage(list), [list]);

	return (
		<div>
			<input value={number} onChange={onChange} />
			<button onClick={onInsert}>등록</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>평균값: </b>
				{avg}
			</div>
		</div>
	);
};

export default Average;
```

## 8.5 useCallback

> 💡 만들어 놨던 함수를 재사용할 때 사용

앞의 Average 컴포넌트를 보면 onChange, onInsert라는 함수를 선언함

- 이렇게 선언하면 컴포넌트가 리렌더링될 때마다 새로 만들어진 함수를 사용하게 됨
- 컴포넌트의 렌더링이 자주 발생하거나 렌더링해야할 컴포넌트의 개수가 많아지면 이 부분을 최적화해 주는 것이 좋음

```jsx
import React, { useState, useMemo, useCallback } from 'react';

const getAverage = (numbers) => {
	console.log('평균값 계산 중...');
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((a, b) => a + b);
	return sum / numbers.length;
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState('');

	const onChange = useCallback((e) => {
		setNumber(e.target.value);
	}, []);
	const onInsert = useCallback(
		(e) => {
			const nextList = list.concat(parseInt(number));
			setList(nextList);
			setNumber('');
		},
		[number, list]
	); // number 혹은 list가 바뀌었을 때만 함수 생성

	const avg = useMemo(() => getAverage(list), [list]);

	return (
		<div>
			<input value={number} onChange={onChange} />
			<button onClick={onInsert}>등록</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>평균값: </b>
				{avg}
			</div>
		</div>
	);
};

export default Average;
```

- useCallback의 첫 번째 파라미터에는 생성하고 싶은 함수를 넣고, 두 번째 파라미터에는 배열을 넣으면 됨
- 함수 내부에서 상태 값에 의존해야 할 때는 그 값을 반드시 두 번째 파라미터 안에 포함시켜야 함
  - onInsert는 기존의 number와 list를 조회해서 nextList를 생성하기 때문에 배열 안에 numbers와 list를 꼭 넣어 주어야 함

## 8.6 useRef

> 💡 ref를 쉽게 사용할 수 있도록 해 줌

```jsx
import React, { useState, useMemo, useCallback } from 'react';

const getAverage = (numbers) => {
	console.log('평균값 계산 중...');
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((a, b) => a + b);
	return sum / numbers.length;
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState('');
	const iputEl = useRef(null);

	const onChange = useCallback((e) => {
		setNumber(e.target.value);
	}, []);
	const onInsert = useCallback(
		(e) => {
			const nextList = list.concat(parseInt(number));
			setList(nextList);
			setNumber('');
		},
		[number, list]
	); // number 혹은 list가 바뀌었을 때만 함수 생성

	const avg = useMemo(() => getAverage(list), [list]);

	return (
		<div>
			<input value={number} onChange={onChange} ref={inputEl} />
			<button onClick={onInsert}>등록</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>평균값: </b>
				{avg}
			</div>
		</div>
	);
};

export default Average;
```

👉 useRef를 사용하면 ref를 설정하면 useRef를 통해 만든 객체 안의 current 값이 실제 엘리먼트를 가리킴

### 8.6.1 로컬 변수 사용하기

```jsx
import React, { useRef } from 'react';

const RefSample = () => {
	const id = useRef(1);
	const setId = (n) => {
		id.current = n;
	};
	const printId = () => {
		console.log(id.current);
	};

	return <div>refSample</div>;
};

export default RefSample;
```

👉 이렇게 ref 안의 값이 바뀌어도 컴포넌트가 렌더링되지 않음

- 렌더링과 관련되지 않은 값을 관리할 때만 이러한 방식으로 코드를 작성

## 8.7 커스텀 Hooks 만들기

> 💡 여러 컴포넌트에서 비슷한 기능을 공유할 경우, 이를 여러분만의 Hook으로 작성하여 로직을 재사용할 수 있음

## 8.8 다른 Hooks

- https://nikgraf.github.io/react-hooks
- https://github.com/rehooks/awesome-react-hooks
