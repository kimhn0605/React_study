# 8. Hooks

## 8.1 useState

> π‘ κ°μ₯ κΈ°λ³Έμ μΈ Hook μ΄λ©°, ν¨μν μ»΄ν¬λνΈμμλ κ°λ³μ μΈ μνλ₯Ό μ§λ μ μλλ‘ ν¨.

<br>

#### β» useState λ₯Ό ν λ²λ§ μ¬μ©ν κ²½μ°

<br>

`Counter.js`

```jsx
import React, { useState } from "react";

const Counter = () => {
	const [value, setValue] = useState(0);
	return (
		<div>
			<p>
				νμ¬ μΉ΄μ΄ν° κ°μ <b>{value}</b> μλλ€.
			</p>
			<button onClick={() => setValue(value + 1)}>+1</button>
			<button onClick={() => setValue(value - 1)}>-1</button>
		</div>
	);
};

export default Counter;
```

<br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179495549-004a9407-4a44-4fe8-8646-28deddb0ac08.png)

#### β» useState λ₯Ό μ¬λ¬ λ² μ¬μ©ν κ²½μ°

<br>

`Info.js`

```jsx
import React, { useState } from "react";

const Info = () => {
	const [name, setName] = useState("");
	const [nickname, setNickname] = useState("");

	const onChangeName = (e) => {
		setName(e.target.value);
	};

	const onChangeNickname = (e) => {
		setNickname(e.target.value);
	};

	return (
		<div>
			<div>
				<input value={name} onChange={onChangeName} /> <br />
				<input value={nickname} onChange={onChangeNickname} /> <br />
				<br />
			</div>
			<div>
				<div>
					<b>μ΄λ¦ : </b> {name} <br />
					<b>λλ€μ : </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

<br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179496136-861ec70d-f8db-4128-974f-5fa6ee1a5abd.png)

<br>

## 8.2 useEffect

> π‘ λ¦¬μ‘νΈ μ»΄ν¬λνΈκ° λ λλ§λ λλ§λ€ νΉμ  μμμ μννλλ‘ μ€μ ν  μ μλ Hook μΌλ‘, ν΄λμ€ν μ»΄ν¬λνΈμ componentDidMount μ componentDidUpdate λ₯Ό ν©μΉ νν

<br>

#### β» λ λλ§λ  λλ§λ€ μν

<br>

`Info.js`

```jsx
import React, { useEffect, useState } from "react";

const Info = () => {
	const [name, setName] = useState("");
	const [nickname, setNickname] = useState("");

	useEffect(() => {
		console.log("λ λλ§μ΄ μλ£λμμ΅λλ€!");
		console.log([name, nickname]);
	});

	const onChangeName = (e) => {
		setName(e.target.value);
	};

	const onChangeNickname = (e) => {
		setNickname(e.target.value);
	};

	return (
		<div>
			<div>
				<input value={name} onChange={onChangeName} /> <br />
				<input value={nickname} onChange={onChangeNickname} /> <br />
				<br />
			</div>
			<div>
				<div>
					<b>μ΄λ¦ : </b> {name} <br />
					<b>λλ€μ : </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

<br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179499446-24d485bb-5161-4bbe-93cf-212fba24294a.png)

<br>

#### β» λ§μ΄νΈλ  λλ§ μ€ννκ³  μΆμ λ

<br>

`Info.js`

```jsx
useEffect(() => {
	console.log("λ§μ΄νΈλ  λλ§ μ€νλ©λλ€.");
	console.log([name, nickname]);
}, []);
```

- useEffect μμ μ€μ ν ν¨μλ₯Ό μ»΄ν¬λνΈκ° νλ©΄μ λ§¨ μ²μ λ λλ§λ  λλ§ μ€ννκ³ , μλ°μ΄νΈλ  λλ μ€ννμ§ μμΌλ €λ©΄ ν¨μμ 2λ²μ§Έ νλΌλ―Έν°λ‘ λΉμ΄ μλ λ°°μ΄μ μΆκ°

<br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179499972-ccbffa98-4d28-4eb7-9efe-1e27d1a27a49.png)

<br><br>

#### β» νΉμ  κ°μ΄ μλ°μ΄νΈλ  λλ§ μ€ννκ³  μΆμ λ

<br>

```jsx
componentDidUpdate(prevProps, prevState) {
  if (prevProps.value !== this.propls.value) {
    doSomething();
  }
}
```

- props μμ λ€μ΄ μλ value κ°μ΄ λ°λ λλ§ νΉμ  μμμ μν
- μ΄λ¬ν μμμ useEffect μμ ν΄μΌ νλ€λ©΄ λ λ²μ§Έ νλΌλ―Έν°λ‘ μ λ¬λλ λ°°μ΄ μμ κ²μ¬νκ³  μΆμ κ°μ λ£μ΄μ£Όλ©΄ λ¨.
  <br>

`Info.js`

```jsx
useEffect(() => {
	console.log([name, nickname]);
}, [name]);
```

<br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179501393-bc26b05e-f27e-47ea-9de2-811a4fc6bca5.png)

<br>

#### β» λ·μ λ¦¬ ν¨μ

- useEffect λ κΈ°λ³Έμ μΌλ‘ λ λλ§λκ³  λ μ§νλ§λ€ μ€νλλ©°,
  - 2λ²μ§Έ νλΌλ―Έν° λ°°μ΄μ λ¬΄μμ λ£λμ§μ λ°λΌ μ€νλλ μ‘°κ±΄μ΄ λ¬λΌμ§.
- μ»΄ν¬λνΈκ° μΈλ§μ΄νΈλκΈ° μ μ΄λ μλ°μ΄νΈλκΈ° μ§μ μ μ΄λ ν μμμ μννκ³  μΆλ€λ©΄
  - useEffect μμ λ·μ λ¦¬ (cleanup) ν¨μλ₯Ό λ°νν΄μ€μΌ ν¨.

<br>

`Info.js`

```jsx
useEffect(() => {
	console.log("effect");
	console.log(name);
	return () => {
		console.log("cleanup");
		console.log(name);
	};
}, [name]);
```

<br>

`App.js`

```jsx
import React, { useState } from "react";
import Info from "./Info";

const App = () => {
	const [visible, setVisible] = useState(false);

	return (
		<div>
			<button
				onClick={() => {
					setVisible(!visible);
				}}
			>
				{visible ? "μ¨κΈ°κΈ°" : "λ³΄μ΄κΈ°"}
			</button>
			<hr />
			{visible && <Info />}
		</div>
	);
};

export default App;
```

- μ»΄ν¬λνΈκ° λνλ  λ μ½μμ effect κ° λνλκ³ , μ¬λΌμ§ λ cleanup μ΄ λνλ¨.
- λ λλ§λ  λλ§λ€ λ·μ λ¦¬ ν¨μκ° κ³μ λνλλ©°, λ·μ λ¦¬ ν¨μκ° νΈμΆλ  λλ μλ°μ΄νΈλκΈ° μ§μ μ κ°μ λ³΄μ¬μ€.

<br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/180723432-784a47f3-c049-47f5-81e5-3c664c9ec1a4.png)

<Br>

#### β» μΈλ§μ΄νΈλ  λλ§ λ·μ λ¦¬ ν¨μ νΈμΆνκ³  μΆμ λ

<br>

`Info.js`

```jsx
useEffect(() => {
	console.log("effect");
	return () => {
		console.log("unmount");
	};
}, []);
```

- useEffect ν¨μμ 2λ²μ§Έ νλΌλ―Έν°μ λΉμ΄ μλ λ°°μ΄μ λ£μΌλ©΄ λ¨.
  - μλ ₯κ° λ³νμ μκ΄μμ΄ λ²νΌ ν΄λ¦­ μμλ§ λ·μ λ¦¬ ν¨μ μ€ν
    <br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179503486-43b3c560-3661-4564-85d7-ded058f9a707.png)

<br>

## 8.3 useReducer

> π‘ useState λ³΄λ€ λ λ€μν μ»΄ν¬λνΈ μν©μ λ°λΌ λ€μν μνλ₯Ό λ€λ₯Έ κ°μΌλ‘ μλ°μ΄νΈν΄μ£Όκ³  μΆμ λ μ¬μ©νλ Hook

<br>

```jsx
function reducer(state, action) {
  return { ... }  // λΆλ³μ±μ μ§ν€λ©΄μ μλ°μ΄νΈν μλ‘μ΄ μνλ₯Ό λ°ν
}
```

- λ¦¬λμλ νμ¬ μν, κ·Έλ¦¬κ³  μλ°μ΄νΈλ₯Ό μν΄ νμν μ λ³΄λ₯Ό λ΄μ μ‘μ κ°μ μ λ¬λ°μ μλ‘μ΄ μνλ₯Ό λ°ννλ ν¨μ
- λ¦¬λμ ν¨μμμ μλ‘μ΄ μνλ₯Ό λ§λ€ λλ λ°λμ λΆλ³μ±μ μ§μΌμ€μΌ ν¨.

<br>

```jsx
{
  type: 'INCREMENT',
  // λ€λ₯Έ κ°λ€μ΄ νμνλ€λ©΄ μΆκ°λ‘ λ€μ΄κ°.
}
```

- μΆνμ λ°°μΈ λ¦¬λμ€λ μ‘μ κ°μ²΄μ μ΄λ€ μ‘μμΈμ§ μλ € μ£Όλ type νλκ° κΌ­ νμνμ§λ§
  - useReducer μμ μ¬μ©νλ μ‘μ κ°μ²΄λ λ°λμ type μ μ§λκ³  μμ νμ X
  - λν, λ¦¬λμμμ type μ κ°μ²΄κ° μλ λ¬Έμμ΄μ΄λ μ«μμ¬λ λ¬΄κ΄

<br>

#### β» μΉ΄μ΄ν° κ΅¬ννκΈ°

<br>

`Counter.js`

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
	// action.type μ λ°λΌ λ€λ₯Έ μμ μν
	switch (action.type) {
		case "INCREMENT":
			return { value: state.value + 1 };
		case "DECREMENT":
			return { value: state.value - 1 };
		default:
			// μλ¬΄κ²λ ν΄λΉλμ§ μμ λ κΈ°μ‘΄ μν λ°ν
			return state;
	}
}

const Counter = () => {
	const [state, dispatch] = useReducer(reducer, { value: 0 });

	return (
		<div>
			<p>
				νμ¬ μΉ΄μ΄ν° κ°μ <b>{state.value}</b> μλλ€.
			</p>
			<button onClick={() => dispatch({ type: "INCREMENT" })}>+1</button>
			<button onClick={() => dispatch({ type: "DECREMENT" })}>-1</button>
		</div>
	);
};

export default Counter;
```

- useReducer μ μ²« λ²μ§Έ νλΌλ―Έν°μλ λ¦¬λμ ν¨μλ₯Ό λ£κ³ , λ λ²μ§Έ νλΌλ―Έν°μλ ν΄λΉ λ¦¬λμμ κΈ°λ³Έκ°μ λ£μ΄μ€.
  <br>
- useReducer λ₯Ό μ¬μ©νμ λμ κ°μ₯ ν° μ₯μ μ μ»΄ν¬λνΈ μλ°μ΄νΈ λ‘μ§μ μ»΄ν¬λνΈ λ°κΉ₯μΌλ‘ λΉΌλΌ μ μλ€λ κ²
  <br>
- μ΄ Hook μ μ¬μ©νλ©΄ state κ°κ³Ό dispatch ν¨μλ₯Ό λ°μ μ€λλ°,
  - state λ νμ¬ κ°λ¦¬ν€κ³  μλ μν
  - dispatch λ μ‘μμ λ°μμν€λ ν¨μ
    - dispatch(action) κ³Ό λμΌν ννλ‘, ν¨μ μμ νλΌλ―Έν°λ‘ μ‘μ κ°μ λ£μ΄ μ£Όλ©΄ λ¦¬λμ ν¨μκ° νΈμΆλλ κ΅¬μ‘°
    - dispatch({...}) μ νλΌλ―Έν°μΈ κ°μ²΄κ° reducer ν¨μμ action μΈμλ‘ λ€μ΄κ°κ² λ¨.
      <br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179505714-f84083c8-5b90-459f-938f-72cdaa22215c.png)

<br>

#### β» μΈν μν κ΄λ¦¬νκΈ°

<br>

`Info.js`

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
	return {
		...state,
		[action.name]: action.value,
	};
}

const Info = () => {
	const [state, dispatch] = useReducer(reducer, {
		name: "",
		nickname: "",
	});

	const { name, nickname } = state;

	const onChange = (e) => {
		dispatch(e.target);
	};

	return (
		<div>
			<div>
				<input name='name' value={name} onChange={onChange} /> <br />
				<input name='nickname' value={nickname} onChange={onChange} /> <br />
				<br />
			</div>
			<div>
				<div>
					<b>μ΄λ¦ : </b> {name} <br />
					<b>λλ€μ : </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

- useReducer μμμ μ‘μμ κ·Έ μ΄λ€ κ°λ μ¬μ© κ°λ₯
  - μ΄λ²€νΈ κ°μ²΄κ° μ§λκ³  μλ e.target κ° μμ²΄λ₯Ό μ‘μ κ°μΌλ‘ μ¬μ©
- μ΄λ° μμΌλ‘ μΈνμ κ΄λ¦¬νλ©΄ μλ¬΄λ¦¬ μΈνμ κ°μκ° λ§μμ Έλ μ½λλ₯Ό μ§§κ³  κΉλνκ² μ μ§ κ°λ₯
  <br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179496136-861ec70d-f8db-4128-974f-5fa6ee1a5abd.png)

<br>

## 8.4 useMemo

> π‘ useMemo λ₯Ό μ¬μ©νλ©΄ ν¨μν μ»΄ν¬λνΈ λ΄λΆμμ λ°μνλ μ°μ° μ΅μ ν κ°λ₯

<br>

`Average.js`

```jsx
import React, { useState } from "react";

const getAverage = (numbers) => {
	console.log("νκ· κ° κ³μ° μ€..");
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((prev, cur) => prev + cur, 0);
	return sum / numbers.length;
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState("");

	const onChange = (e) => {
		setNumber(e.target.value);
	};

	const onInsert = (e) => {
		const nextList = list.concat(parseInt(number));
		setList(nextList);
		setNumber("");
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
				<b>νκ· κ° : </b> {getAverage(list)}
			</div>
		</div>
	);
};

export default Average;
```

- λ¦¬μ€νΈμ μ«μλ₯Ό μΆκ°νλ©΄ μΆκ°λ μ«μλ€μ νκ· μ κ³μ°ν΄μ λ³΄μ¬μ£Όλ ν¨μν μ»΄ν¬λνΈ
- νμ§λ§ μΈν λ΄μ©μ΄ μμ λ  λλ getAverage ν¨μκ° νΈμΆλμ΄ λΆνμνκ² νκ· κ°μ΄ κ³μ°λλ κ²μ μ μ μμ.
  <br>

#### β» reduce() ν¨μ

```js
arr.reduce(callback[, initialValue])
```

- `callback function`
  - callback ν¨μμ λ°ν κ°μ accumulator μ ν λΉλκ³ , μννλ©° κ³μ λμ λμ΄ μ΅μ’μ μΌλ‘ νλμ κ°μ λ°ννλ©°, μλ 4κ°μ§ μΈμλ₯Ό κ°μ§.
    - `accumulator `
      - callback ν¨μμ λ°νκ°μ λμ 
    - `currentValue `
      - λ°°μ΄μ νμ¬ μμ
    - `index(Optional)`
      - λ°°μ΄μ νμ¬ μμμ μΈλ±μ€
    - `array(Optional)`
      - νΈμΆν λ°°μ΄

<br>

- `initialValue (Optional)`
  - μ΅μ΄ callback ν¨μ μ€ν μ accumulator μΈμμ μ κ³΅λλ κ°
  - μ΄κΈ°κ°μ μ κ³΅νμ§ μμ κ²½μ° λ°°μ΄μ μ²« λ²μ§Έ μμλ₯Ό μ¬μ©νκ³ ,
    - λΉ λ°°μ΄μμ μ΄κΈ°κ°μ΄ μμ κ²½μ° μλ¬ λ°μ
      <br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179512554-9c1c49a0-dfb8-426f-bda2-e5cb2d12cb78.png)

<br>

`Average.js`

```jsx
import React, { useState, useMemo } from "react";

const getAverage = (numbers) => {
	console.log("νκ· κ° κ³μ° μ€..");
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((a, b) => a + b);
	return sum / numbers.length;
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState("");

	const onChange = (e) => {
		setNumber(e.target.value);
	};

	const onInsert = (e) => {
		const nextList = list.concat(parseInt(number));
		setList(nextList);
		setNumber("");
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
				<b>νκ· κ° : </b> {avg}
			</div>
		</div>
	);
};

export default Average;
```

- useMemo Hook μ μ¬μ©νμ¬ λ λλ§
  - νΉμ  κ°μ΄ λ°λμμ λλ§ μ°μ°μ μννκ³ , μνλ κ°μ΄ λ°λμ§ μμλ€λ©΄ μ΄μ μ μ°μ°νλ κ²°κ³Όλ₯Ό λ€μ μ¬μ©νλ λ°©μ
  - list λ°°μ΄μ λ΄μ©μ΄ λ°λ λλ§ getAverage ν¨μ νΈμΆνμ¬ νκ· κ° λ°ν
    <br>

#### β» useMemo

```jsx
useMemo(func, array);
```

- `func`

  - μ΄λ»κ² μ°μ°ν  μ§ μ μνλ ν¨μ
    <br>

- `array`
  - λ°°μ΄ μμ λ΄μ©μ΄ λ°λ λλ§ func μΈμμ λ±λ‘ν ν¨μλ₯Ό νΈμΆνμ¬ κ° μ°μ°
  - κ°μ΄ λ°λμ§ μμΌλ©΄ μ΄μ μ μ°μ°ν κ°μ μ¬μ¬μ©
    <br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179512827-4bfb333c-e961-4f62-a61d-1cb6a8f1d71b.png)

<br>

## 8.5 useCallback

> π‘ useCallback μ useMemo μ λΉμ·ν ν¨μλ‘, μ£Όλ‘ λ λλ§ μ±λ₯μ μ΅μ νν΄μΌ νλ μν©μμ μ¬μ©νλ©° λ§λ€μ΄ λ¨λ ν¨μ μ¬μ¬μ© κ°λ₯

<br>

`Average.js`

```jsx
import React, { useState, useMemo, useCallback } from "react";

const getAverage = (numbers) => {
	console.log("νκ· κ° κ³μ° μ€..");
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((a, b) => a + b);
	return sum / numbers.length;
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState("");

	const onChange = useCallback((e) => {
		setNumber(e.target.value);
	}, []); // μ»΄ν¬λνΈκ° μ²μ λ λλ§λ  λλ§ ν¨μ μμ±

	const onInsert = useCallback(() => {
		const nextList = list.concat(parseInt(number));
		setList(nextList);
		setNumber("");
	}, [number, list]);

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
				<b>νκ· κ° : </b> {avg}
			</div>
		</div>
	);
};

export default Average;
```

- μμμ useMemo λ‘ κ΅¬ννλ Average μ»΄ν¬λνΈμμλ λ¦¬λ λλ§λ  λλ§λ€ μλ‘ λ§λ€μ΄μ§ ν¨μ (onChange, onInsert) λ₯Ό μ¬μ©
  - μ»΄ν¬λνΈμ λ λλ§μ΄ μμ£Ό λ°μνκ±°λ λ λλ§ν΄μΌ ν  μ»΄ν¬λνΈ κ°μκ° λ§μμ§λ©΄ μ΅μ νν΄μ£Όλ κ²μ΄ μ’μ.
    <br>

#### β» useCallback

```jsx
useCallback(func, array);
```

- `func`

  - μμ±νκ³  μΆμ ν¨μ
    <br>

- `array`
  - μ΄ λ°°μ΄μλ μ΄λ€ κ°μ΄ λ°λμμ λ ν¨μλ₯Ό μλ‘ μμ±ν΄μΌ νλμ§ κ·Έ κ°μ λͺμν΄μΌ ν¨.
  - ν¨μ λ΄λΆμμ μν κ°μ μμ‘΄ν΄μΌ ν  λλ κ·Έ κ°μ λ°λμ λ λ²μ§Έ νλΌλ―Έν° μμ ν¬ν¨μμΌμ€μΌ ν¨.
    - onChange κ²½μ° κΈ°μ‘΄μ κ°μ μ‘°ννμ§ μκ³  λ°λ‘ μ€μ λ§ νκΈ° λλ¬Έμ λ°°μ΄μ΄ λΉμ΄ μμ΄λ μκ΄ X
    - onInsert κ²½μ° κΈ°μ‘΄μ number μ list λ₯Ό μ‘°νν΄μ nextList λ₯Ό μμ±νκΈ° λλ¬Έμ λ°°μ΄ μμ number μ list λ₯Ό κΌ­ λ£μ΄μ€μΌ ν¨.

<br>

#### β» useMemo / useCallback μ°¨μ΄μ 

- `useMemo`
  - νΉμ  κ²°κ³Όκ°μ μ¬μ¬μ©
    <br>
- `useCallback`
  - νΉμ  ν¨μλ₯Ό μ¬μ¬μ©
    <br>

## 8.6 useRef

> π‘ ν¨μν μ»΄ν¬λνΈμμ ref λ₯Ό μ½κ² μ¬μ©ν  μ μλλ‘ ν΄μ£Όλ Hook

<br>

`Average.js`

```jsx
import React, { useState, useMemo, useCallback, useRef } from "react";

const getAverage = (numbers) => {
	console.log("νκ· κ° κ³μ° μ€..");
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((a, b) => a + b);
	return sum / numbers.length;
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState("");
	const inputEl = useRef(null);

	const onChange = useCallback((e) => {
		setNumber(e.target.value);
	}, []); // μ»΄ν¬λνΈκ° μ²μ λ λλ§λ  λλ§ ν¨μ μμ±

	const onInsert = useCallback(() => {
		const nextList = list.concat(parseInt(number));
		setList(nextList);
		setNumber("");
		inputEl.current.focus();
	}, [number, list]);

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
				<b>νκ· κ° : </b> {avg}
			</div>
		</div>
	);
};

export default Average;
```

- 'λ±λ‘' λ²νΌμ λλ μ λ ν¬μ»€μ€κ° μΈν μͺ½μΌλ‘ λμ΄κ°λλ‘ μ½λ μμ±
  - useRef λ₯Ό μ¬μ©νμ¬ ref λ₯Ό μ€μ νλ©΄ useRef λ₯Ό ν΅ν΄ λ§λ  κ°μ²΄ μμ current κ°μ΄ μ€μ  μλ¦¬λ¨ΌνΈλ₯Ό κ°λ¦¬ν΄.

<br>

#### β» useRef μ©λ

1. μ»΄ν¬λνΈμμ νΉμ  DOM μ ν
2. μ»΄ν¬λνΈ λ΄μμ μ‘°ν/μμ ν  μ μλ λ³μ κ΄λ¦¬
3. useRefλ‘ κ΄λ¦¬λλ λ³μλ κ°μ΄ λ°λμ΄λ μ»΄ν¬λνΈ λ¦¬λ λλ§ X
   - useRef() μ νλΌλ―Έν°λ₯Ό ν΅ν΄ `.current` μ κΈ°λ³Έκ° μ§μ  κ°λ₯
   - κ° μμ /μ‘°ν μ `.current` μ¬μ©

<br>

### π μ€ν νλ©΄

<br>

![image](https://user-images.githubusercontent.com/77706631/179515151-b5d2a51c-68b6-4290-be0b-bf192a105714.png)

<br>

## 8.7 μ»€μ€ν Hooks λ§λ€κΈ°

- μ¬λ¬ μ»΄ν¬λνΈμμ λΉμ·ν κΈ°λ₯μ κ³΅μ ν  κ²½μ°, μ΄λ₯Ό μμ λ§μ Hooks λ‘ μμ±νμ¬ λ‘μ§ μ¬μ¬μ© κ°λ₯
  - μλ μμλ useReducer λ‘ μμ±νλ λ‘μ§μ useInputs λΌλ Hook μΌλ‘ λ°λ‘ λΆλ¦¬ν μ½λ

<br>

`useInputs.js`

```jsx
import { useReducer } from "react";

function reducer(state, action) {
	return {
		...state,
		[action.name]: action.value,
	};
}

export default function useInputs(initialForm) {
	const [state, dispatch] = useReducer(reducer, initialForm);
	const onChange = (e) => {
		dispatch(e.target);
	};
	return [state, onChange];
}
```

<br>

`Info.js`

```jsx
import React, { useReducer } from "react";
import useInputs from "./useInputs";

const Info = () => {
	const [state, onChange] = useInputs({
		name: "",
		nickname: "",
	});

	const { name, nickname } = state;

	return (
		<div>
			<div>
				<input name='name' value={name} onChange={onChange} /> <br />
				<input name='nickname' value={nickname} onChange={onChange} /> <br />
				<br />
			</div>
			<div>
				<div>
					<b>μ΄λ¦ : </b> {name} <br />
					<b>λλ€μ : </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

<br>

## 8.8 λ€λ₯Έ Hooks

- μ»€μ€ν Hooks λ₯Ό λ§λ€μ΄μ μ¬μ©νλ κ²μ²λΌ, λ€λ₯Έ κ°λ°μκ° λ§λ  Hooks λ λΌμ΄λΈλ¬λ¦¬λ₯Ό μ€μΉν΄μ μ¬μ©νλ κ²λ κ°λ₯
  - https://nikgraf.github.io/react-hooks
  - https://github.com/rehooks/awesome-react-hooks
