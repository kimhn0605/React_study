# 15. Context API

### Context API

- 리액트 프로젝트에서 전역적으로 사용할 데이터가 있을 때 유용한 기능
- 리덕스, 리액트 라우터, styled-components 등의 라이브러리는 Context API 를 기반으로 구현되어 있음.

<br>

## 15.1 Context API 를 사용한 전역 상태 관리 흐름 이해하기

#### ※ 일반적인 전역 상태 관리 흐름

<img style="height:400px" src="https://user-images.githubusercontent.com/77706631/190596452-cae33392-b0bb-4e90-bc1a-93bb0ed0d322.png">

<br><br>

- 리액트 애플리케이션은 컴포넌트 간에 데이터를 props 로 전달하기 때문에

  - 주로 최상위 컴포넌트인 App 의 state 에 넣어서 관리
    <br>

- 이런 방식을 사용하면 다루어야 하는 데이터가 많아졌을 때 유지 보수성 ↓
  - 리덕스나 MobX 같은 상태 관리 라이브러리를 사용하여 전역 상태 관리 작업을 처리하거나
  - `Context API` 를 사용하여 별도의 라이브러리 없이도 전역 상태 관리 가능
    <br>

#### ※ Context API 를 사용한 전역 상태 관리 흐름

<img style="height:400px" src="https://user-images.githubusercontent.com/77706631/190596684-ccb43d38-c3e4-4cf5-bad4-39e8b7e52247.png">

<br><br>

- 기존에는 최상위 컴포넌트에서 여러 컴포넌트를 거쳐 props 로 원하는 상태와 함수를 전달했지만,
  - Context API 를 사용하면 단 한 번만에 원하는 값을 Context 로부터 받아 와서 사용 가능

<br>

## 15.2 Context API 사용법 익히기

### 15.2.1 새 Context 만들기

<br>

`contexts/color.js`

```jsx
import { createContext } from "react";

const ColorContext = createContext({ color: "black" });

export default ColorContext;
```

- 새로운 Context 를 만들 때는 `createContext` 함수 사용
  - 파라미터에는 해당 Context 의 기본 상태를 지정

<br>

### 15.2.2 Consumer 사용하기

<br>

`components/ColorBox.js`

```jsx
import React from "react";
import ColorContext from "../contexts/color";

const ColorBox = () => {
	return (
		<ColorContext.Consumer>
			{(value) => (
				<div
					style={{
						width: "64px",
						height: "64px",
						background: value.color,
					}}
				/>
			)}
		</ColorContext.Consumer>
	);
};

export default ColorBox;
```

- ColorContext 안에 들어 있는 색상을 props 로 받아 오는 것이 아니라

  - ColorContext 안에 들어 있는 `Consumer` 라는 컴포넌트를 통해 색상 조회
    <br>

- **Function as a child** 또는 **Render Props**
  - 컴포넌트의 children 이 있어야 할 자리에 일반 JSX 혹은 문자열이 아닌 함수를 전달하는 형태
  - Consumer 사이에 중괄호를 열어서 그 안에 함수를 넣어줌.
    <br>

`App.js`

```jsx
import React from "react";
import ColorBox from "./components/ColorBox";

const App = () => {
	return (
		<div>
			<ColorBox />
		</div>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/190607450-69cc245c-a858-4e19-a6df-c77fef4e473f.png)

<br>

### 15.2.3 Provider

<br>

`App.js`

```jsx
import React from "react";
import ColorBox from "./components/ColorBox";
import ColorContext from "./contexts/color";

const App = () => {
	return (
		<ColorContext.Provider value={{ color: "red" }}>
			<div>
				<ColorBox />
			</div>
		</ColorContext.Provider>
	);
};

export default App;
```

- Provider 를 사용하면 Context 의 value 변경 가능
  - `createContext` 함수 파라미터에 넣어준 기본값은 Provider 를 사용하지 않았을 때만 유효
    <br>
- Provider 를 사용할 때는 value 값을 반드시 명시해줘야 함.
  - 만약 Provider 를 사용하면서 value 를 명시하지 않았다면 오류 발생

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/190607676-e6b67c22-ffa4-464e-b7ef-5cf566ac881e.png)

<br>

## 15.3 동적 Context 사용하기

### 15.3.1 Context 파일 수정하기

<br>

`contexts/color.js`

```jsx
import { createContext, useState } from "react";

const ColorContext = createContext({
	state: { color: "black", subcolor: "red" },
	actions: {
		setColor: () => {},
		setSubcolor: () => {},
	},
});

const ColorProvider = ({ children }) => {
	const [color, setColor] = useState("black");
	const [subcolor, setSubcolor] = useState("red");

	const value = {
		state: { color, subcolor },
		actions: { setColor, setSubcolor },
	};

	return <ColorContext.Provider value={value}>{children}</ColorContext.Provider>;
};

// const ColorConsumer = ColorContext.Consumer 과 같은 의미
const { Consumer: ColorConsumer } = ColorContext;

// ColorProvider 와 ColorConsumer 내보내기
export { ColorProvider, ColorConsumer };

export default ColorContext;
```

- Context 의 value 에는 상태 값뿐만 아니라 함수도 전달 가능
- Provider 의 value 에는 상태는 state 로, 업데이트 함수는 actions 로 묶어서 전달
  - 이렇게 state 와 actions 객체를 따로따로 분리해 주면 다른 컴포넌트에서 Context 값을 사용할 때 편리함.
- createContext 의 기본값을 실제 Provider 의 value 에 넣는 객체의 형태와 일치시켜 주면
  - Context 코드를 볼 때 내부 값이 어떻게 구성되어 있는지 파악하기에 쉬움.
    <br>

### 15.3.2 새로워진 Context 를 프로젝트에 반영하기

<Br>

`App.js`

```jsx
import React from "react";
import ColorBox from "./components/ColorBox";
import { ColorProvider } from "./contexts/color";

const App = () => {
	return (
		<ColorProvider>
			<div>
				<ColorBox />
			</div>
		</ColorProvider>
	);
};

export default App;
```

<br>

`components/ColorBox.js`

```jsx
import React from "react";
import { ColorConsumer } from "../contexts/color";

const ColorBox = () => {
	return (
		<ColorConsumer>
			{({ state }) => (
				<>
					<div
						style={{
							width: "64px",
							height: "64px",
							background: state.color,
						}}
					/>
					<div
						style={{
							width: "32px",
							height: "32px",
							background: state.subcolor,
						}}
					/>
				</>
			)}
		</ColorConsumer>
	);
};

export default ColorBox;
```

- 객체 비구조화 할당 문법을 사용하여 value 조회 생략 가능

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/190629268-519eaf21-8b4c-4e6e-99d8-dc5503e9d48c.png)

<br>

### 15.3.3 색상 선택 컴포넌트 만들기

<br>

`components/SelectColrs.js`

```jsx
import React from "react";

const colors = ["red", "orange", "yellow", "green", "blue", "indigo", "violet"];

const SelectColors = () => {
	return (
		<div>
			<h2>색상을 선택하세요.</h2>
			<div style={{ display: "flex" }}>
				{colors.map((color) => (
					<div
						key={color}
						style={{
							background: color,
							width: "24px",
							height: "24px",
							cursor: "pointer",
						}}
					/>
				))}
			</div>
			<hr />
		</div>
	);
};

export default SelectColors;
```

- Context 의 actions 에 넣어 준 함수를 호출하는 컴포넌트 생성
- <br>

`App.js`

```jsx
import React from "react";
import ColorBox from "./components/ColorBox";
import { ColorProvider } from "./contexts/color";
import SelectColors from "./components/SelectColors";

const App = () => {
	return (
		<ColorProvider>
			<div>
				<SelectColors />
				<ColorBox />
			</div>
		</ColorProvider>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/190630167-207e6373-da47-43a8-8bbc-fbd1dd153dda.png)

<br>

`componens/SelectColors.js`

```jsx
import React from "react";
import { ColorConsumer } from "../contexts/color";

const colors = ["red", "orange", "yellow", "green", "blue", "indigo", "violet"];

const SelectColors = () => {
	return (
		<div>
			<h2>색상을 선택하세요.</h2>
			<ColorConsumer>
				{({ actions }) => (
					<div style={{ display: "flex" }}>
						{colors.map((color) => (
							<div
								key={color}
								style={{
									background: color,
									width: "24px",
									height: "24px",
									cursor: "pointer",
								}}
								onClick={() => actions.setColor(color)}
								onContextMenu={(e) => {
									e.preventDefault(); // 마우스 오른쪽 버튼 클릭 시 메뉴 뜨는 것을 무시
									actions.setSubcolor(color);
								}}
							/>
						))}
					</div>
				)}
			</ColorConsumer>
			<hr />
		</div>
	);
};

export default SelectColors;
```

- 해당 SelectColors 에서 마우스 왼쪽 버튼을 클릭하면 큰 정사각형의 색상을 변경하고,
  - 마우스 오른쪽 버튼을 클릭하면 작은 정사각형의 색상을 변경하도록 구현
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/190630691-8a6041f9-34f8-488d-b5c9-fc19d9bf955d.png)

<br>

## 15.4 Consumer 대신 Hook 또는 static contextType 사용하기

### 15.4.1 useContext Hook 사용하기

<br>

`components/ColorBox.js`

```jsx
import React, { useContext } from "react";
import ColorContext from "../contexts/color";

const ColorBox = () => {
	const { state } = useContext(ColorContext);

	return (
		<>
			<div
				style={{
					width: "64px",
					height: "64px",
					background: state.color,
				}}
			/>
			<div
				style={{
					width: "32px",
					height: "32px",
					background: state.subcolor,
				}}
			/>
		</>
	);
};

export default ColorBox;
```

- 리액트에 내장되어 있는 `useContext` 라는 Hook 을 사용하면 함수형 컴포넌트에서 편리하게 Context 사용 가능

  - 단, 클래스형 컴포넌트에서는 Hook 사용 X
    <br>

- children 에 함수를 전달하는 Render Props 패턴이 불편하다면
  - useContext Hook 을 사용하여 훨신 편하게 Context 값 조회 가능
    <br>

### 15.4.2 static contextType 사용하기

<br>

`components/SelectColors.js`

```jsx
import React, { Component } from "react";
import ColorContext from "../contexts/color";

const colors = ["red", "orange", "yellow", "green", "blue", "indigo", "violet"];

class SelectColors extends Component {
	// 클래스 상단에 static contextType 값 지정
	static contextType = ColorContext;

	handleSetColor = (color) => {
		this.context.actions.setColor(color);
	};

	handleSetSubcolor = (subcolor) => {
		this.context.actions.setSubcolor(subcolor);
	};

	render() {
		return (
			<div>
				<h2>색상을 선택하세요.</h2>
				<ColorConsumer>
					{({ actions }) => (
						<div style={{ display: "flex" }}>
							{colors.map((color) => (
								<div
									key={color}
									style={{
										background: color,
										width: "24px",
										height: "24px",
										cursor: "pointer",
									}}
									onClick={() => this.handleSetColor(color)}
									onContextMenu={(e) => {
										e.preventDefault(); // 마우스 오른쪽 버튼 클릭 시 메뉴 뜨는 것을 무시
										actions.handleSetSubcolor(color);
									}}
								/>
							))}
						</div>
					)}
				</ColorConsumer>
				<hr />
			</div>
		);
	}
}

export default SelectColors;
```

- 클래스형 컴포넌트에서 `static contextType` 를 정의하게 되면
  - 클래스 메서드에서도 Context 에 넣어 둔 함수 호출을 할 수 있지만,
  - 한 클래스에 한 개의 Context 밖에 사용하지 못한다는 단점이 있음.
