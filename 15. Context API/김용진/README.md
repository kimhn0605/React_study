# 15. Context API

> 💡 Context API는 리액트 프로젝트에서 전역적으로 사용할 데이터가 있을 때 유용한 기능

## 15.1 Context API를 사용한 전역 상태 관리 흐름 이해하기

App 컴포넌트에서 state를 관리하고 props를 통해 전달하는 방식

- 컴포넌트가 많아지면 여러 컴포넌트를 거쳐야 함
- 유지 보수성이 낮아짐

![image](https://user-images.githubusercontent.com/55246584/187640319-f59a4879-599b-4c47-bdf5-e19b2addd9a9.png)

![image](https://user-images.githubusercontent.com/55246584/187640371-9a92fc39-2253-491b-af77-120afc835432.png)

## 15.2 Context API 사용법 익히기

### 15.2.1 새 Context 만들기

```jsx
// color.js
import { createContext } from 'react';

const ColorContext = createContext({ color: 'black' });

export default ColorContext;
```

### 15.2.2 Consumer 사용하기

```jsx
// components/ColorBox.js
import ColorContext from '../contexts/color';

const ColorBox = () => {
	return (
		<ColorContext.Consumer>
			{(value) => (
				<div
					style={{ width: '64px', height: '64px', background: value.color }}
				></div>
			)}
		</ColorContext.Consumer>
	);
};

export default ColorBox;
```

### 15.2.3 Provider

> Provider를 사용하면 Context의 value를 변경할 수 있음

```jsx
import ColorBox from './components/ColorBox';
import ColorContext from './components/color';

const App = () => {
	return (
		<ColorContext.Provider value={{ color: 'red' }}>
			<div>
				<ColorBox />
			</div>
		</ColorContext.Provider>
	);
};
```
