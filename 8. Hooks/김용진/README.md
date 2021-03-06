# 8. Hooks

> π‘ Hooksλ λ¦¬μ‘νΈ v16.8μ μλ‘ λμλ κΈ°λ₯μΌλ‘ ν¨μ μ»΄ν¬λνΈμμλ μν κ΄λ¦¬λ₯Ό ν  μ μλ useState, λ λλ§ μ§ν μμμ μ€μ νλ useEffect λ±μ κΈ°λ₯μ μ κ³΅νμ¬ κΈ°μ‘΄μ ν¨μ μ»΄ν¬λνΈμμ ν  μ μμλ λ€μν μμμ ν  μ μκ² ν΄μ€

## 8.1 useState

> π‘ ν¨μ μ»΄ν¬λνΈμμλ κ°λ³μ μΈ μνλ₯Ό μ§λ μ μκ² ν¨

```jsx
const [value, setValue] = useState(0);
```

## 8.2 useEffect

> π‘ λ¦¬μ‘νΈ μ»΄ν¬λνΈκ° λ λλ§λ  λλ§λ€ νΉμ  μμμ μννλλ‘ μ€μ ν  μ μλ Hook

π μ»΄ν¬λνΈμ `componentDidMount`μ `componentDidUpdate`λ₯Ό ν©μΉ ννλ‘ λ³΄μλ λ¨

### 8.2.1 λ§μ΄νΈλ  λλ§ μ€ννκ³  μΆμ λ

```jsx
useEffect(() => {
	console.log('λ§μ΄νΈλ  λλ§ μ€νλ©λλ€.');
}, []);
```

### 8.2.2 νΉμ  κ°μ΄ μλ°μ΄νΈλ  λλ§ μ€ννκ³  μΆμ λ

νΉμ  κ°μ΄ λ³κ²½λ  λλ§ νΈμΆνκ³  μΆμ κ²½μ°, ν΄λμ€ν μ»΄ν¬λνΈλΌλ©΄ λ€μκ³Ό κ°μ΄ μμ±

```jsx
componentDidUpdate(prevProps,prevState){
    if(prevProps.value !== this.props.value){
        doSomething();
    }
}
```

ν¨μν μ»΄ν¬λνΈμ κ²½μ° λ€μκ³Ό κ°μ΄ μμ±

```jsx
useEffect(() => {
	console.log(name);
}, [name]);
```

### 8.2.3 λ·μ λ¦¬νκΈ°

> π‘ useEffectλ κΈ°λ³Έμ μΌλ‘ λ λλ§λκ³  λ μ§νλ§λ€ μ€νλλ©°, λ λ²μ§Έ νλΌλ―Έν° λ°°μ΄μ λ¬΄μμ λ£λμ§μ λ°λΌ μ€νλλ μ‘°κ±΄μ΄ λ¬λΌμ§

π μ»΄ν¬λνΈκ° μΈλ§μ΄νΈλκΈ° μ μ΄λ μλ°μ΄νΈ μ§μ μ μ΄λ ν μμμ μννκ³  μΆλ€λ©΄ useEffectμμ λ·μ λ¦¬ ν¨μλ₯Ό λ°ν

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

> π‘ useStateλ³΄λ€ λ λ€μν μ»΄ν¬λνΈ μν©μ λ°λΌ λ€μν μνλ₯Ό λ€λ₯Έ κ°μΌλ‘ μλ°μ΄νΈν  λ μ¬μ©νλ Hook

**Reducer**

- νμ¬ μν, κ·Έλ¦¬κ³  μλ°μ΄νΈλ₯Ό μν΄ νμν μ λ³΄λ₯Ό λ΄μ μ‘μ κ°μ μ λ¬λ°μ μλ‘μ΄ μνλ₯Ό λ°ννλ ν¨μ
- μλ‘μ΄ μνλ₯Ό λ§λ€ λλ λ°λμ λΆλ³μ±μ μ§μΌμΌ ν¨

### 8.3.1 μΉ΄μ΄ν° κ΅¬ννκΈ°

```jsx
import React, { useReducer } from 'react';

function reducer(state, action) {
	// action.typeμ λ°λΌ λ€λ₯Έ μμ μν
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
			// μλ¬΄κ²λ ν΄λΉλμ§ μμ λ κΈ°μ‘΄ μν λ³ν
			return state;
	}
}

const Counter = () => {
	const [state, dispatch] = useReducer(reducer, { value: 0 });

	return (
		<div>
			<p>
				νμ¬ μΉ΄μ΄ν° κ°μ <b>{state.value}</b>
			</p>
			<button onClick={() => dispatch({ type: 'INCREMENT' })}>+1</button>
			<button onClick={() => dispatch({ type: 'DECREMENT' })}>-1</button>
		</div>
	);
};
```

- useReducerμ μ²« λ²μ§Έ νλΌλ―Έν°μλ λ¦¬λμ ν¨μλ₯Ό λ£κ³ , λ λ²μ§Έ νλΌλ―Έν°μλ ν΄λΉ λ¦¬λμμ κΈ°λ³Έκ°μ λ£μ
- ν΄λΉ Hookμ μ¬μ©νλ©΄ stateκ°κ³Ό dispatch ν¨μλ₯Ό λ°μμ΄
- stateλ νμ¬ κ°λ¦¬ν€κ³  μλ μν, dispatchλ μ‘μμ λ°μμν€λ ν¨μ
- `dispatch(action)`κ³Ό κ°μ ννλ‘, ν¨μ μμ νλΌλ―Έν°λ‘ μ‘μ κ°μ λ£μ΄ μ£Όλ©΄ λ¦¬λμ ν¨μκ° νΈμΆλλ κ΅¬μ‘°

### 8.3.2 μΈν μν κ΄λ¦¬νκΈ°

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
					<b>μ΄λ¦: </b> {name}
				</div>
				<div>
					<b>λλ€μ: </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

## 8.4 useMemo

> π‘ ν¨μ μ»΄ν¬λνΈ λ΄λΆμμ λ°μνλ μ°μ°μ μ΅μ ν κ°λ₯

```jsx
import React, { useState } from 'react';

const getAverage = (numbers) => {
	console.log('νκ· κ° κ³μ° μ€...');
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
			<button onClick={onInsert}>λ±λ‘</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>νκ· κ°: </b>
				{getAverage(list)}
			</div>
		</div>
	);
};

export default Average;
```

π μ«μλ₯Ό λ±λ‘ν  λ λΏ μλλΌ μΈν λ΄μ©μ΄ μμ λ  λλ μ°λ¦¬κ° λ§λ  getAverage ν¨μκ° νΈμΆλ¨

- μΈν λ΄μ©μ΄ λ°λ λλ νκ·  κ°μ λ€μ κ³μ°ν  νμκ° μλλ°, λ λλ§ν  λλ§λ€ κ³μ°νλ κ²μ λ­λΉ

```jsx
import React, { useState } from 'react';

const getAverage = (numbers) => {
	console.log('νκ· κ° κ³μ° μ€...');
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
			<button onClick={onInsert}>λ±λ‘</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>νκ· κ°: </b>
				{avg}
			</div>
		</div>
	);
};

export default Average;
```

## 8.5 useCallback

> π‘ λ§λ€μ΄ λ¨λ ν¨μλ₯Ό μ¬μ¬μ©ν  λ μ¬μ©

μμ Average μ»΄ν¬λνΈλ₯Ό λ³΄λ©΄ onChange, onInsertλΌλ ν¨μλ₯Ό μ μΈν¨

- μ΄λ κ² μ μΈνλ©΄ μ»΄ν¬λνΈκ° λ¦¬λ λλ§λ  λλ§λ€ μλ‘ λ§λ€μ΄μ§ ν¨μλ₯Ό μ¬μ©νκ² λ¨
- μ»΄ν¬λνΈμ λ λλ§μ΄ μμ£Ό λ°μνκ±°λ λ λλ§ν΄μΌν  μ»΄ν¬λνΈμ κ°μκ° λ§μμ§λ©΄ μ΄ λΆλΆμ μ΅μ νν΄ μ£Όλ κ²μ΄ μ’μ

```jsx
import React, { useState, useMemo, useCallback } from 'react';

const getAverage = (numbers) => {
	console.log('νκ· κ° κ³μ° μ€...');
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
	); // number νΉμ listκ° λ°λμμ λλ§ ν¨μ μμ±

	const avg = useMemo(() => getAverage(list), [list]);

	return (
		<div>
			<input value={number} onChange={onChange} />
			<button onClick={onInsert}>λ±λ‘</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>νκ· κ°: </b>
				{avg}
			</div>
		</div>
	);
};

export default Average;
```

- useCallbackμ μ²« λ²μ§Έ νλΌλ―Έν°μλ μμ±νκ³  μΆμ ν¨μλ₯Ό λ£κ³ , λ λ²μ§Έ νλΌλ―Έν°μλ λ°°μ΄μ λ£μΌλ©΄ λ¨
- ν¨μ λ΄λΆμμ μν κ°μ μμ‘΄ν΄μΌ ν  λλ κ·Έ κ°μ λ°λμ λ λ²μ§Έ νλΌλ―Έν° μμ ν¬ν¨μμΌμΌ ν¨
  - onInsertλ κΈ°μ‘΄μ numberμ listλ₯Ό μ‘°νν΄μ nextListλ₯Ό μμ±νκΈ° λλ¬Έμ λ°°μ΄ μμ numbersμ listλ₯Ό κΌ­ λ£μ΄ μ£Όμ΄μΌ ν¨

## 8.6 useRef

> π‘ refλ₯Ό μ½κ² μ¬μ©ν  μ μλλ‘ ν΄ μ€

```jsx
import React, { useState, useMemo, useCallback } from 'react';

const getAverage = (numbers) => {
	console.log('νκ· κ° κ³μ° μ€...');
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
	); // number νΉμ listκ° λ°λμμ λλ§ ν¨μ μμ±

	const avg = useMemo(() => getAverage(list), [list]);

	return (
		<div>
			<input value={number} onChange={onChange} ref={inputEl} />
			<button onClick={onInsert}>λ±λ‘</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>νκ· κ°: </b>
				{avg}
			</div>
		</div>
	);
};

export default Average;
```

π useRefλ₯Ό μ¬μ©νλ©΄ refλ₯Ό μ€μ νλ©΄ useRefλ₯Ό ν΅ν΄ λ§λ  κ°μ²΄ μμ current κ°μ΄ μ€μ  μλ¦¬λ¨ΌνΈλ₯Ό κ°λ¦¬ν΄

### 8.6.1 λ‘μ»¬ λ³μ μ¬μ©νκΈ°

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

π μ΄λ κ² ref μμ κ°μ΄ λ°λμ΄λ μ»΄ν¬λνΈκ° λ λλ§λμ§ μμ

- λ λλ§κ³Ό κ΄λ ¨λμ§ μμ κ°μ κ΄λ¦¬ν  λλ§ μ΄λ¬ν λ°©μμΌλ‘ μ½λλ₯Ό μμ±

## 8.7 μ»€μ€ν Hooks λ§λ€κΈ°

> π‘ μ¬λ¬ μ»΄ν¬λνΈμμ λΉμ·ν κΈ°λ₯μ κ³΅μ ν  κ²½μ°, μ΄λ₯Ό μ¬λ¬λΆλ§μ HookμΌλ‘ μμ±νμ¬ λ‘μ§μ μ¬μ¬μ©ν  μ μμ

## 8.8 λ€λ₯Έ Hooks

- https://nikgraf.github.io/react-hooks
- https://github.com/rehooks/awesome-react-hooks
