# 8. Hooks

## 8.1 useState

> 💡 가장 기본적인 Hook 이며, 함수형 컴포넌트에서도 가변적인 상태를 지닐 수 있도록 함.

<br>

#### ※ useState 를 한 번만 사용한 경우

<br>

`Counter.js`

```jsx
import React, { useState } from "react";

const Counter = () => {
	const [value, setValue] = useState(0);
	return (
		<div>
			<p>
				현재 카운터 값은 <b>{value}</b> 입니다.
			</p>
			<button onClick={() => setValue(value + 1)}>+1</button>
			<button onClick={() => setValue(value - 1)}>-1</button>
		</div>
	);
};

export default Counter;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179495549-004a9407-4a44-4fe8-8646-28deddb0ac08.png)

#### ※ useState 를 여러 번 사용한 경우

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
					<b>이름 : </b> {name} <br />
					<b>닉네임 : </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179496136-861ec70d-f8db-4128-974f-5fa6ee1a5abd.png)

<br>

## 8.2 useEffect

> 💡 리액트 컴포넌트가 렌더링돌 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook 으로, 클래스형 컴포넌트의 componentDidMount 와 componentDidUpdate 를 합친 형태

<br>

#### ※ 렌더링될 때마다 수행

<br>

`Info.js`

```jsx
import React, { useEffect, useState } from "react";

const Info = () => {
	const [name, setName] = useState("");
	const [nickname, setNickname] = useState("");

	useEffect(() => {
		console.log("렌더링이 완료되었습니다!");
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
					<b>이름 : </b> {name} <br />
					<b>닉네임 : </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179499446-24d485bb-5161-4bbe-93cf-212fba24294a.png)

<br>

#### ※ 마운트될 때만 실행하고 싶을 때

<br>

`Info.js`

```jsx
useEffect(() => {
	console.log("마운트될 때만 실행됩니다.");
	console.log([name, nickname]);
}, []);
```

- useEffect 에서 설정한 함수를 컴포넌트가 화면에 맨 처음 렌더링될 때만 실행하고, 업데이트될 때는 실행하지 않으려면 함수의 2번째 파라미터로 비어 있는 배열을 추가

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179499972-ccbffa98-4d28-4eb7-9efe-1e27d1a27a49.png)

<br><br>

#### ※ 특정 값이 업데이트될 때만 실행하고 싶을 때

<br>

```jsx
componentDidUpdate(prevProps, prevState) {
  if (prevProps.value !== this.propls.value) {
    doSomething();
  }
}
```

- props 안에 들어 있는 value 값이 바뀔 때만 특정 작업을 수행
- 이러한 작업을 useEffect 에서 해야 한다면 두 번째 파라미터로 전달되는 배열 안에 검사하고 싶은 값을 넣어주면 됨.
  <br>

`Info.js`

```jsx
useEffect(() => {
	console.log([name, nickname]);
}, [name]);
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179501393-bc26b05e-f27e-47ea-9de2-811a4fc6bca5.png)

<br>

#### ※ 뒷정리 함수

- useEffect 는 기본적으로 렌더링되고 난 직후마다 실행되며,
  - 2번째 파라미터 배열에 무엇을 넣는지에 따라 실행되는 조건이 달라짐.
- 컴포넌트가 언마운트되기 전이나 업데이트되기 직전에 어떠한 작업을 수행하고 싶다면
  - useEffect 에서 뒷정리 (cleanup) 함수를 반환해줘야 함.

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
				{visible ? "숨기기" : "보이기"}
			</button>
			<hr />
			{visible && <Info />}
		</div>
	);
};

export default App;
```

- 컴포넌트가 나타날 때 콘솔에 effect 가 나타나고, 사라질 때 cleanup 이 나타남.
- 렌더링될 때마다 뒷정리 함수가 계속 나타나며, 뒷정리 함수가 호출될 때는 업데이트되기 직전의 값을 보여줌.

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/180723432-784a47f3-c049-47f5-81e5-3c664c9ec1a4.png)

<Br>

#### ※ 언마운트될 때만 뒷정리 함수 호출하고 싶을 때

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

- useEffect 함수의 2번째 파라미터에 비어 있는 배열을 넣으면 됨.
  - 입력값 변화에 상관없이 버튼 클릭 시에만 뒷정리 함수 실행
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179503486-43b3c560-3661-4564-85d7-ded058f9a707.png)

<br>

## 8.3 useReducer

> 💡 useState 보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트해주고 싶을 때 사용하는 Hook

<br>

```jsx
function reducer(state, action) {
  return { ... }  // 불변성을 지키면서 업데이트한 새로운 상태를 반환
}
```

- 리듀서는 현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션 값을 전달받아 새로운 상태를 반환하는 함수
- 리듀서 함수에서 새로운 상태를 만들 때는 반드시 불변성을 지켜줘야 함.

<br>

```jsx
{
  type: 'INCREMENT',
  // 다른 값들이 필요하다면 추가로 들어감.
}
```

- 추후에 배울 리덕스는 액션 객체에 어떤 액션인지 알려 주는 type 필드가 꼭 필요하지만
  - useReducer 에서 사용하는 액션 객체는 반드시 type 을 지니고 있을 필요 X
  - 또한, 리듀서에서 type 은 객체가 아닌 문자열이나 숫자여도 무관

<br>

#### ※ 카운터 구현하기

<br>

`Counter.js`

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
	// action.type 에 따라 다른 작업 수행
	switch (action.type) {
		case "INCREMENT":
			return { value: state.value + 1 };
		case "DECREMENT":
			return { value: state.value - 1 };
		default:
			// 아무것도 해당되지 않을 때 기존 상태 반환
			return state;
	}
}

const Counter = () => {
	const [state, dispatch] = useReducer(reducer, { value: 0 });

	return (
		<div>
			<p>
				현재 카운터 값은 <b>{state.value}</b> 입니다.
			</p>
			<button onClick={() => dispatch({ type: "INCREMENT" })}>+1</button>
			<button onClick={() => dispatch({ type: "DECREMENT" })}>-1</button>
		</div>
	);
};

export default Counter;
```

- useReducer 의 첫 번째 파라미터에는 리듀서 함수를 넣고, 두 번째 파라미터에는 해당 리듀서의 기본값을 넣어줌.
  <br>
- useReducer 를 사용했을 때의 가장 큰 장점은 컴포넌트 업데이트 로직을 컴포넌트 바깥으로 빼낼 수 있다는 것
  <br>
- 이 Hook 을 사용하면 state 값과 dispatch 함수를 받아 오는데,
  - state 는 현재 가리키고 있는 상태
  - dispatch 는 액션을 발생시키는 함수
    - dispatch(action) 과 동일한 형태로, 함수 안에 파라미터로 액션 값을 넣어 주면 리듀서 함수가 호출되는 구조
    - dispatch({...}) 의 파라미터인 객체가 reducer 함수의 action 인자로 들어가게 됨.
      <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179505714-f84083c8-5b90-459f-938f-72cdaa22215c.png)

<br>

#### ※ 인풋 상태 관리하기

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
					<b>이름 : </b> {name} <br />
					<b>닉네임 : </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

- useReducer 에서의 액션은 그 어떤 값도 사용 가능
  - 이벤트 객체가 지니고 있는 e.target 값 자체를 액션 값으로 사용
- 이런 식으로 인풋을 관리하면 아무리 인풋의 개수가 많아져도 코드를 짧고 깔끔하게 유지 가능
  <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179496136-861ec70d-f8db-4128-974f-5fa6ee1a5abd.png)

<br>

## 8.4 useMemo

> 💡 useMemo 를 사용하면 함수형 컴포넌트 내부에서 발생하는 연산 최적화 가능

<br>

`Average.js`

```jsx
import React, { useState } from "react";

const getAverage = (numbers) => {
	console.log("평균값 계산 중..");
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
				<b>평균값 : </b> {getAverage(list)}
			</div>
		</div>
	);
};

export default Average;
```

- 리스트에 숫자를 추가하면 추가된 숫자들의 평균을 계산해서 보여주는 함수형 컴포넌트
- 하지만 인풋 내용이 수정될 때도 getAverage 함수가 호출되어 불필요하게 평균값이 계산되는 것을 알 수 있음.
  <br>

#### ※ reduce() 함수

```js
arr.reduce(callback[, initialValue])
```

- `callback function`
  - callback 함수의 반환 값은 accumulator 에 할당되고, 순회하며 계속 누적되어 최종적으로 하나의 값을 반환하며, 아래 4가지 인자를 가짐.
    - `accumulator `
      - callback 함수의 반환값을 누적
    - `currentValue `
      - 배열의 현재 요소
    - `index(Optional)`
      - 배열의 현재 요소의 인덱스
    - `array(Optional)`
      - 호출한 배열

<br>

- `initialValue (Optional)`
  - 최초 callback 함수 실행 시 accumulator 인수에 제공되는 값
  - 초기값을 제공하지 않을 경우 배열의 첫 번째 요소를 사용하고,
    - 빈 배열에서 초기값이 없을 경우 에러 발생
      <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179512554-9c1c49a0-dfb8-426f-bda2-e5cb2d12cb78.png)

<br>

`Average.js`

```jsx
import React, { useState, useMemo } from "react";

const getAverage = (numbers) => {
	console.log("평균값 계산 중..");
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
			<button onClick={onInsert}>등록</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>평균값 : </b> {avg}
			</div>
		</div>
	);
};

export default Average;
```

- useMemo Hook 을 사용하여 렌더링
  - 특정 값이 바뀌었을 때만 연산을 수행하고, 원하는 값이 바뀌지 않았다면 이전에 연산했던 결과를 다시 사용하는 방식
  - list 배열의 내용이 바뀔 때만 getAverage 함수 호출하여 평균값 반환
    <br>

#### ※ useMemo

```jsx
useMemo(func, array);
```

- `func`

  - 어떻게 연산할 지 정의하는 함수
    <br>

- `array`
  - 배열 안에 내용이 바뀔 때만 func 인자에 등록한 함수를 호출하여 값 연산
  - 값이 바뀌지 않으면 이전에 연산한 값을 재사용
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179512827-4bfb333c-e961-4f62-a61d-1cb6a8f1d71b.png)

<br>

## 8.5 useCallback

> 💡 useCallback 은 useMemo 와 비슷한 함수로, 주로 렌더링 성능을 최적화해야 하는 상황에서 사용하며 만들어 놨던 함수 재사용 가능

<br>

`Average.js`

```jsx
import React, { useState, useMemo, useCallback } from "react";

const getAverage = (numbers) => {
	console.log("평균값 계산 중..");
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((a, b) => a + b);
	return sum / numbers.length;
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState("");

	const onChange = useCallback((e) => {
		setNumber(e.target.value);
	}, []); // 컴포넌트가 처음 렌더링될 때만 함수 생성

	const onInsert = useCallback(() => {
		const nextList = list.concat(parseInt(number));
		setList(nextList);
		setNumber("");
	}, [number, list]);

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
				<b>평균값 : </b> {avg}
			</div>
		</div>
	);
};

export default Average;
```

- 위에서 useMemo 로 구현했던 Average 컴포넌트에서는 리렌더링될 때마다 새로 만들어진 함수 (onChange, onInsert) 를 사용
  - 컴포넌트의 렌더링이 자주 발생하거나 렌더링해야 할 컴포넌트 개수가 많아지면 최적화해주는 것이 좋음.
    <br>

#### ※ useCallback

```jsx
useCallback(func, array);
```

- `func`

  - 생성하고 싶은 함수
    <br>

- `array`
  - 이 배열에는 어떤 값이 바뀌었을 때 함수를 새로 생성해야 하는지 그 값을 명시해야 함.
  - 함수 내부에서 상태 값에 의존해야 할 때는 그 값을 반드시 두 번째 파라미터 안에 포함시켜줘야 함.
    - onChange 경우 기존의 값을 조회하지 않고 바로 설정만 하기 때문에 배열이 비어 있어도 상관 X
    - onInsert 경우 기존의 number 와 list 를 조회해서 nextList 를 생성하기 때문에 배열 안에 number 와 list 를 꼭 넣어줘야 함.

<br>

#### ※ useMemo / useCallback 차이점

- `useMemo`
  - 특정 결과값을 재사용
    <br>
- `useCallback`
  - 특정 함수를 재사용
    <br>

## 8.6 useRef

> 💡 함수형 컴포넌트에서 ref 를 쉽게 사용할 수 있도록 해주는 Hook

<br>

`Average.js`

```jsx
import React, { useState, useMemo, useCallback, useRef } from "react";

const getAverage = (numbers) => {
	console.log("평균값 계산 중..");
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
	}, []); // 컴포넌트가 처음 렌더링될 때만 함수 생성

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
			<button onClick={onInsert}>등록</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>평균값 : </b> {avg}
			</div>
		</div>
	);
};

export default Average;
```

- '등록' 버튼을 눌렀을 때 포커스가 인풋 쪽으로 넘어가도록 코드 작성
  - useRef 를 사용하여 ref 를 설정하면 useRef 를 통해 만든 객체 안의 current 값이 실제 엘리먼트를 가리킴.

<br>

#### ※ useRef 용도

1. 컴포넌트에서 특정 DOM 선택
2. 컴포넌트 내에서 조회/수정할 수 있는 변수 관리
3. useRef로 관리되는 변수는 값이 바뀌어도 컴포넌트 리렌더링 X
   - useRef() 의 파라미터를 통해 `.current` 의 기본값 지정 가능
   - 값 수정/조회 시 `.current` 사용

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/179515151-b5d2a51c-68b6-4290-be0b-bf192a105714.png)

<br>

## 8.7 커스텀 Hooks 만들기

- 여러 컴포넌트에서 비슷한 기능을 공유할 경우, 이를 자신만의 Hooks 로 작성하여 로직 재사용 가능
  - 아래 예시는 useReducer 로 작성했던 로직을 useInputs 라는 Hook 으로 따로 분리한 코드

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
					<b>이름 : </b> {name} <br />
					<b>닉네임 : </b> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```

<br>

## 8.8 다른 Hooks

- 커스텀 Hooks 를 만들어서 사용했던 것처럼, 다른 개발자가 만든 Hooks 도 라이브러리를 설치해서 사용하는 것도 가능
  - https://nikgraf.github.io/react-hooks
  - https://github.com/rehooks/awesome-react-hooks
