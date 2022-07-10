# 6. 컴포넌트 반복

## 6.1 자바스크립트 배열의 map() 함수

> 💡 자바스크립트 배열 객체의 내장 함수인 map 함수를 사용하여 반복되는 컴포넌트를 렌더링 할 수 있음

👉 map 함수는 파라미터로 전달된 함수를 사용해서 배열 내 각 요소를 원하는 규칙에 따라 변환한 후 그 결과로 새로운 배열을 생성

### 6.1.1 문법

> arr.map(callback, [thisArg])

- 해당 함수의 파라미터는 다음과 같음
  - callback: 새로운 배열의 요소를 생성하는 함수로 파라미터는 다음 세 가지
    - currentValue: 현재 처리하고 있는 요소
    - index: 현재 처리하고 있는 요소의 index 값
    - array: 현재 처리하고 있는 원본 배열
  - thisArg(선택 항목): callback 함수 내부에서 사용할 this 레퍼런스

### 6.1.2 예제

```js
const numbers = [1, 2, 3, 4, 5];

const processed = numbers.map((num) => num * num);
console.log(processed); // [1, 4, 9, 16, 25]
```

## 6.2 데이터 배열을 컴포넌트 배열로 변환하기

### 컴포넌트 수정하기

```jsx
const IterationSample = () => {
	const names = ['눈사람', '얼음', '눈', '바람'];
	const nameList = names.map((name) => <li>{name}</li>);
	return <ul>{nameList}</ul>;
};

export default IterationSample;
```

### 6.2.2 App 컴포넌트에서 예제 컴포넌트 렌더링

```jsx
import React, { Component } from 'react';
import IterationSample from './IterationSample';

class App extends Component {
	render() {
		return <IterationSample />;
	}
}

export default App;
```

## 6.3 key

- 유동적인 데이터를 다룰 때는 원소를 새로 생성할 수도 있고, 제거할 수도 있고, 수정할 수도 있음
- key가 없을 때는 Virtual DOM을 비교하는 과정에서 순차적으로 비교하면서 변화를 감지
  👉 하지만 key가 있다면 이 값을 사용하여 어떤 변화가 있어났는지 빠르게 알아낼 수 있음

### 6.3.1 key 설정

- key 값은 언제나 유일해야 함

```jsx
const articleList = articles.map((article) => (
	<Article title={article.title} writer={article.writer} key={article.id} />
));
```

```jsx
const IterationSample = () => {
	const names = ['눈사람', '얼음', '눈', '바람'];
	const nameList = names.map((name, index) => <li key={index}>{name}</li>);
	return <ul>{nameList}</ul>;
};

export default IterationSample;
```

👉 고유한 값이 없을 때만 index 값을 key로 사용해야함

- index를 key로 사용하면 배열이 변경될 때 효율적으로 리렌더링하지 못함
