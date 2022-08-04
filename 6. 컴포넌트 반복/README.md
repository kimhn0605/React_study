# 6. 컴포넌트 반복

## 6.1 자바스크립트 배열의 map() 함수

```jsx
arr.map(callback, [thisArg]);
```

- **반복되는 컴포넌트를 렌더링할 때 내장 함수 map 함수 이용**
  - 파라미터로 전달된 함수를 사용하여 배열 내 각 요소를 원하는 규칙에 따라 변환한 후 그 결과로 새로운 배열을 생성
    <br>
- 해당 함수의 파라미터는 다음과 같음
  - `callback` : 새로운 배열의 요소를 생성하는 함수로 파라미터는 다음 세 가지
    - currentValue: 현재 처리하고 있는 요소
    - index: 현재 처리하고 있는 요소의 index 값
    - array: 현재 처리하고 있는 원본 배열
  - `thisArg` : callback 함수 내부에서 사용할 this 레퍼런스 (옵션)
    <br>

#### ※ map() 함수 예시

```jsx
var numbers = [1, 2, 3, 4, 5];

var processed = numbers.map(function (num) {
	return num * num;
});

console.log(processed);
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/178270625-001fcc4c-db05-434a-a0b4-ab6c7525a43f.png)

<br>

## 6.2 데이터 배열을 컴포넌트 배열로 변환하기

`IterationSample.js`

```jsx
import React from "react";

const IterationSample = () => {
	const names = ["눈사람", "얼음", "눈", "바람"];
	const nameList = names.map((name) => <li>{name}</li>);
	return <ul>{nameList}</ul>;
};

export default IterationSample;
```

<Br>

`App.js`

```jsx
import React, { Component } from "react";
import IterationSample from "./IterationSample";

class App extends Component {
	render() {
		return <IterationSample />;
	}
}
export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/178264596-68938dee-b2c8-462e-9a34-35bff01f5b71.png)

![image](https://user-images.githubusercontent.com/77706631/178265036-a7dca38a-d49f-46e1-991e-a9938b9fa1d7.png)

- 원하는 대로 렌더링은 되었지만 콘솔창에서는 "key" prop 이 없다는 경고 메시지가 출력됨.
  <br>

## 6.3 key

- 리액트에서 key 는 컴포넌트 배열을 렌더링했을 때 어떤 원소에 변동이 있었는지 알아내기 위해 사용
  - key 가 없을 때는 Virtual DOM 을 비교하는 과정에서 리스트를 순차적으로 비교하면서 변화를 감지

👉 **key가 있다면 이 값을 사용하여 어떤 변화가 있어났는지 빠르게 알아낼 수 있음.**
<br>

`IterationSample.js`

```jsx
import React from "react";

const IterationSample = () => {
	const names = ["눈사람", "얼음", "눈", "바람"];
	const nameList = names.map((name, index) => <li key={index}>{name}</li>);
	return <ul>{nameList}</ul>;
};

export default IterationSample;
```

- key 값을 설정할 때는 map 함수의 인자로 전달되는 함수 내부에서 컴포넌트 props 를 설정하듯이 설정하면 됨.
- key 값은 언제나 유일해야 하기 때문에 데이터가 가진 고유값을 key 값으로 설정해야 함.
  - 고유 번호가 없다면 map 함수에 전달되는 콜백 함수의 인수인 index 값을 사용하면 되지만,
  - index 를 key 로 사용하면 배열이 변경될 때 효율적으로 리렌더링하지 못하기 때문에 반드시 고유한 값이 없을 때만 index 값을 key 로 사용할 것

## 6.4 응용

#### 1) 초기 상태 설정

`IterationSample.js`

```jsx
import React, { useState } from "react";

const IterationSample = () => {
	const [names, setNames] = useState([
		{ id: 1, text: "눈사람" },
		{ id: 2, text: "얼음" },
		{ id: 3, text: "눈" },
		{ id: 4, text: "바람" },
	]);

	const [inputText, setInputText] = useState("");
	const [nextId, setNextId] = useState(5); // 새로운 항목을 추가할 때 사용할 id

	// key 값을 index 대신 name.id 값으로 지정
	const nameList = names.map((name) => <li key={name.id}>{name.text}</li>);
	return <ul>{nameList}</ul>;
};

export default IterationSample;
```

- 고정된 배열을 렌더링하는 것이 아닌 동적인 배열 렌더링을 구현 예정
- map 함수를 사용할 때 key 값을 index 대신 `name.id` 값으로 지정
  - 리렌더링 효율적
    <br>

#### 2) 데이터 추가 기능 구현하기

`IterationSample.js`

```jsx
import React, { useState } from "react";

const IterationSample = () => {
	const [names, setNames] = useState([
		{ id: 1, text: "눈사람" },
		{ id: 2, text: "얼음" },
		{ id: 3, text: "눈" },
		{ id: 4, text: "바람" },
	]);

	const [inputText, setInputText] = useState("");
	const [nextId, setNextId] = useState(5); // 새로운 항목을 추가할 때 사용할 id

	// key 값을 index 대신 name.id 값으로 지정
	const nameList = names.map((name) => <li key={name.id}>{name.text}</li>);

	const onChange = (e) => setInputText(e.target.value);
	const onClick = () => {
		const nextNames = names.concat({
			id: nextId, // nextId 값을 id 로 설정하고
			text: inputText,
		});

		setNextId(nextId + 1); // nextId 값에 + 1
		setNames(nextNames); // names 값을 업데이트
		setInputText(""); // inputText 값 초기화
	};

	return (
		<>
			<input value={inputText} onChange={onChange} />
			<button onClick={onClick}>추가</button>
			<ul>{nameList}</ul>;
		</>
	);
};

export default IterationSample;
```

- 배열에 새 항목을 추가할 때 배열의 push 함수 대신 concat 함수 사용
  - push() : 기존 배열 자체를 변경
  - concat() : 새로운 배열을 생성하여 반환
    <br>
- **리액트에서 상태를 업데이트할 때는 기존 상태를 그대로 두면서 새로운 값을 상태로 설정해야 함.** => `불변성 유지`
  - 그래야 나중에 리액트 컴포넌트의 성능을 최적화하는 것이 가능
    <Br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/178268518-0e0001c7-2274-4f1b-85c0-d34bda7629a8.png)
<br>

#### 3) 데이터 제거 기능 구현하기

`IterationSample.js`

```jsx
import React, { useState } from "react";

const IterationSample = () => {
	const [names, setNames] = useState([
		{ id: 1, text: "눈사람" },
		{ id: 2, text: "얼음" },
		{ id: 3, text: "눈" },
		{ id: 4, text: "바람" },
	]);

	const [inputText, setInputText] = useState("");
	const [nextId, setNextId] = useState(5); // 새로운 항목을 추가할 때 사용할 id

	const onChange = (e) => setInputText(e.target.value);
	const onClick = () => {
		const nextNames = names.concat({
			id: nextId, // nextId 값을 id 로 설정하고
			text: inputText,
		});

		setNextId(nextId + 1); // nextId 값에 + 1
		setNames(nextNames); // names 값을 업데이트
		setInputText(""); // inputText 값 초기화
	};

	const onRemove = (id) => {
		const nextNames = names.filter((name) => name.id !== id);
		setNames(nextNames);
	};

	const nameList = names.map((name) => (
		<li key={name.id} onDoubleClick={() => onRemove(name.id)}>
			{name.text}
		</li>
	));

	return (
		<>
			<input value={inputText} onChange={onChange} />
			<button onClick={onClick}>추가</button>
			<ul>{nameList}</ul>;
		</>
	);
};

export default IterationSample;
```

- 각 항목을 더블클릭했을 때 해당 항목이 화면에서 사라지는 기능 구현

  - 마찬가지로 불변성을 유지하면서 업데이트해줘야 함.
    <br>

- 불변성을 유지하면서 배열의 특정 항목을 지울 때는 배열의 내장 함수 filter 사용
  - filter() : 배열에서 특정 조건을 만족하는 원소들만 쉽게 분류 가능
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/178270229-695ce5a4-c335-4e92-82ef-f2c676bea9e9.png)
