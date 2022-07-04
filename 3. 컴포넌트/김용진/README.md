# 3. 컴포넌트

## 3.1 클래스형 컴포넌트

- 컴포넌트를 선언하는 방식은 두 가지

  - 함수 컴포넌트
  - 클래스형 컴포넌트

- 클래스형 컴포넌트와 함수형 컴포넌트의 차이점
  - 클래스 형 컴포넌트의 경우 라이프사이클 기능을 사용할 수 있음

##### 함수형 컴포넌트의 장점

- 클래스형 컴포넌트보다 선언하기가 훨씬 편함
- 메모리 자원도 클래스형 컴포넌트보다 덜 사용
- 프로젝트를 완성하여 빌드한 후 배포할 때도 함수 컴포넌트를 사용하는 것이 결과물의 크기가 더 작음

## 3.2 첫 컴포넌트 생성

## 3.3 props

### 3.3.1 JSX 내부에서 props 렌더링

```jsx
const MyComponent = (props) => {
	return <div>안녕하세요, 제 이름은 {props.name}입니다.</div>;
};

export default MyComponent;
```

### 3.3.2 컴포넌트를 사용할 때 props 값 지정하기

```jsx
import MyComponent from './MyComponent';

const App = () => {
	return <MyComponent name='React' />;
};

export default App;
```

### 3.3.3 props 기본값 설정: defaultProps

```jsx
const MyComponent = (props) => {
	return <div>안녕하세요, 제 이름은 {props.name}입니다.</div>;
};

MyComponent.defaultProps = {
	name: '기본 이름',
};

export default MyComponent;
```

### 3.3.4 태그 사이의 내영을 보여주는 children

> 💡 컴포넌트 태그 사이의 내용을 보여 주는 props

```jsx
// App.js
import MyComponent from './MyComponent';

const App = () => {
	return <MyComponent>리액트</MyComponent>;
};

export default App;
```

```jsx
// MyComponent.js
const MyComponent = (props) => {
	return (
		<div>
			안녕하세요, 제 이름은 {props.name}입니다. children 값은 {props.children}
		</div>
	);
};

MyComponent.defaultProps = {
	name: '기본 이름',
};

export default MyComponent;
```

### 3.3.5 비구조화 할당 문법을 통해 props 내부 값 추출하기

```jsx
// MyComponent.js
const MyComponent = (props) => {
	const { name, children } = props;

	return (
		<div>
			안녕하세요, 제 이름은 {name}입니다. children 값은 {children}
		</div>
	);
};

MyComponent.defaultProps = {
	name: '기본 이름',
};

export default MyComponent;
```

### 3.3.6 propTypes를 통한 props 검증

```jsx
// MyComponent.js
import PropTypes from 'prop-types';

const MyComponent = ({ name, children }) => {
	return (
		<div>
			안녕하세요, 제 이름은 {name}입니다. children 값은 {children}
		</div>
	);
};

MyComponent.defaultProps = {
	name: '기본 이름',
};

MyComponent.propTypes = {
	name: PropTypes.string,
};

export default MyComponent;
```

## 3.4 state

> 💡 리액트에서 state는 컴포넌트 내부에서 바뀔 수 있는 값을 의미

## 3.5 state를 사용할 때 주의 사항

> 💡 state를 사용할 때는 setState 또는 useState를 통해 전달받은 세터 함수를 사용해야 함
