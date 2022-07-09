# 4. 이벤트 핸들링

## 4.1 리액트의 이벤트 시스템

#### **💡 이벤트 사용 시 주의사항**

- ##### <em> 이벤트 이름은 카멜 표기법으로 작성</em>

  - HTML 에서는 onclick, onkeyup
  - 리액트에서는 onClick, onKeyUp
    <br>

- ##### <em> 이벤트에 실행할 자바스크립트 코드를 전달하는 것이 아니라, 함수 형태의 값을 전달 </em>

  - HTML 에서는 이벤트 설정 시 큰따옴표 안에 실행 코드 삽입
  - 리액트에서는 함수 형태의 객체를 전달
    <br>

- ##### <em> DOM 요소에만 이벤트 설정 가능 </em>
  - div, button, input 등 DOM 요소에는 이벤트 설정 가능
  - 직접 만든 컴포넌트에는 이벤트를 자체적으로 설정 불가능 - `<MyComponent onClick={doSomething}/> ` - MyComponent 클릭 시 doSomething 함수를 실행하는 것이 아니라, - 이름이 onClick 인 props 를 MyComponent 에게 전달하게 됨.
    <br>

#### 💡 리액트에서 지원하는 이벤트 종류

- Clipboard, Composition, Keyboard, Focus
- Touch, Wheel, Animation, Trasition 등
  <br>

## 4.2 예제로 이벤트 핸들링 익히기

#### 1) 컴포넌트 생성 및 불러오기

<br>

`EventPractice.js`

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
			</div>
		);
	}
}

export default EventPractice;
```

<br>

`App.js`

```jsx
import React from 'react';
import EventPractice from './EventPractice';

const App = () => {
	return <EventPractice />;
};

export default App;
```

<Br>

### 🔍 실행 화면

<Br>

<img style="height:150px" src="https://user-images.githubusercontent.com/77706631/177721751-aa9a902d-e592-40f7-b57d-ea786968dbeb.png">

#### 2) onChange 이벤트 핸들링하기

##### 2-1) onChange 이벤트 설정

<Br>

`EventPractice.js`

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type='text'
					name='message'
					placeholder='아무거나 입력해보세요'
					onChange={(e) => {
						console.log(e);
					}}
				/>
			</div>
		);
	}
}

export default EventPractice;
```

<br>

### 🔍 실행 화면

<Br>

<img style="height:150px;" src="https://user-images.githubusercontent.com/77706631/177723395-1fb3fef7-8260-405c-ac54-dbe29a9e83c3.png">

<br>

### 🔍 콘솔창

<Br>

![image](https://user-images.githubusercontent.com/77706631/177723284-9643f93c-135b-46d4-a758-60e63dab381f.png)

- 여기서 콘솔에 기록되는 e 객체는 SyntheticEvent 로 웹 브라우저의 네이티브 이벤트를 감싸는 객체
  - SyntheticEvent 는 네이티브 이벤트와 달리 이벤트가 끝나면 초기화되므로 정보를 참조할 수 없게 됨.
  - ex) 0.5 초 뒤에 e 객체를 참조하면 e 객체 내부의 모든 값이 비워지게 됨.
    <br>

`EventPractice.js`

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type='text'
					name='message'
					placeholder='아무거나 입력해보세요'
					onChange={(e) => {
						console.log(e.target.value); // 이 부분 수정
					}}
				/>
			</div>
		);
	}
}

export default EventPractice;
```

- 비동기적으로 이벤트 객체를 참조할 일이 있다면 e.persist() 함수를 호출
  - 위 코드에서는 onChange 이벤트가 발생할 때마다 앞으로 변할 인풋 값인 e.target.value 를 콘솔에 기록
    <br>

### 🔍 콘솔창

<br>

![image](https://user-images.githubusercontent.com/77706631/177724579-3a492c91-f512-40fe-bf1c-e123822899f6.png)

- 값이 바뀔 때마다 바뀌는 값을 콘솔에 기록

<br>

##### 2-2) state 에 input 값 담기

<br>

`EventPractice.js`

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
	state = {
		message: '',
	};

	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type='text'
					name='message'
					placeholder='아무거나 입력해보세요'
					value={this.state.message}
					onChange={(e) => {
						this.setState({
							message: e.target.value,
						});
					}}
				/>
			</div>
		);
	}
}

export default EventPractice;
```

- input 의 value 값을 state 에 있는 값으로 설정
  <br>

##### 2-3) 버튼을 누를 때 comment 값을 공백으로 설정

<br>

`EventPractice.js`

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
	state = {
		message: '',
	};

	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type='text'
					name='message'
					placeholder='아무거나 입력해보세요'
					value={this.state.message}
					onChange={(e) => {
						this.setState({
							message: e.target.value,
						});
					}}
				/>
				<button
					onClick={() => {
						alert(this.state.message);
						this.setState({
							message: '',
						});
					}}
				>
					확인
				</button>
			</div>
		);
	}
}

export default EventPractice;
```

<br>

### 🔍 실행 화면

<Br>

![image](https://user-images.githubusercontent.com/77706631/177726597-274012ca-b38a-443e-9ff7-b7504f82789c.png)

- 입력한 값이 state 에 잘 들어갔음을 알 수 있고, alert 창에서 '확인' 을 누르면 comment 값이 다시 공백으로 설정됨.

<br>

#### 3) 임의 메서드 만들기

- 이벤트에 실행할 자바스크립트 코드를 전달하는 것이 아니라, 함수 형태의 값을 전달해야 함.
  - 이벤트 처리할 때 렌더링을 하는 동시에 함수를 만들어서 전달하는 방법도 있지만,
  - 함수를 미리 준비하여 전달하는 방법이 훨씬 가독성이 높음.
    <br>

##### 3-1) 기본 방식

`EventPractice.js`

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
	state = {
		message: '',
	};

	constructor(props) {
		super(props);
		// 아래와 같이 바인딩하지 않게 되면 this 가 undefined 를 가리키게 됨.
		this.handleChange = this.handleChange.bind(this);
		this.handleClick = this.handleClick.bind(this);
	}

	handleChange(e) {
		this.setState({
			message: e.target.value,
		});
	}

	handleClick() {
		alert(this.state.message);
		this.setState({
			message: '',
		});
	}

	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type='text'
					name='message'
					placeholder='아무거나 입력해보세요'
					value={this.state.message}
					onChange={this.handleChange}
				/>
				<button onClick={this.handleClick}>확인</button>
			</div>
		);
	}
}

export default EventPractice;
```

- 함수가 호출될 때 this 는 호출부에 따라 결정되므로, 클래스의 임의 메서드가 특정 HTML 요소의 이벤트로 등록되는 과정에서 메서드와 this 의 관계가 끊어져 버림.
  <br>

- 그렇기 때문에 임의 메서드가 이벤트로 등록되어도 this 를 컴포넌트 자신으로 제대로 가리키기 위해서는 메서드를 this 와 바인딩하는 작업 필요
  - 바인딩하지 않는 경우에는 this 가 undefined 를 가리키게 됨.
    <br>

##### 3-2) Property Initializer Syntax 를 사용한 메서드 작성

`EventPractice.js`

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
	state = {
		message: '',
	};

	handleChange = (e) => {
		// 화살표 함수 사용
		this.setState({
			message: e.target.value,
		});
	};

	handleClick = () => {
		// 화살표 함수 사용
		alert(this.state.message);
		this.setState({
			message: '',
		});
	};

	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type='text'
					name='message'
					placeholder='아무거나 입력해보세요'
					value={this.state.message}
					onChange={this.handleChange}
				/>
				<button onClick={this.handleClick}>확인</button>
			</div>
		);
	}
}

export default EventPractice;
```

- 메서드 바인딩은 생성자 메서드에서 하는 것이 정석이지만 새 메서드를 만들 때마다 constructor 를 수정해야 한다는 불편함 O
- 바벨의 transform-class-properties 문법을 사용하여 화살표 함수 형태로 메서드를 정의하면 더 간편하게 나타낼 수 있음.

<br>

#### 4) input 여러 개 다루기

- input 이 여러 개일 때는 event 객체의 e.target.name 값을 활용하면 됨.
  - onChange 이벤트 핸들러에서 e.target.name 은 해당 인풋의 name 을 가리키는데, 이 값을 사용하여 state 를 설정하면 쉽게 해결할 수 있음.

<br>

`EventPractice.js`

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
	state = {
		username: '',
		message: '',
	};

	handleChange = (e) => {
		this.setState({
			[e.target.name]: e.target.value,
		});
	};

	handleClick = () => {
		alert(this.state.username + ' : ' + this.state.message);
		this.setState({
			username: '',
			message: '',
		});
	};

	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type='text'
					name='username'
					placeholder='사용자명'
					value={this.state.username}
					onChange={this.handleChange}
				/>
				<input
					type='text'
					name='message'
					placeholder='아무거나 입력해보세요'
					value={this.state.message}
					onChange={this.handleChange}
				/>
				<button onClick={this.handleClick}>확인</button>
			</div>
		);
	}
}

export default EventPractice;
```

- input 이 여러 개여도 메서드 하나만으로 각 state 에 값을 넣어줄 수 있게 됨.

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/177732139-454e0390-f23f-4e07-95dc-d2f39785eea9.png)
<br>

---

<br>

```jsx
const name = 'variantKey';
const object = {
	[name]: 'value',
};

// 결과
/*
{
  'varientKey': 'value'
}
*/
```

- 객체 안에서 key 를 [ ] 로 감싸면 그 안에 넣은 레퍼런스가 가리키는 실제 값이 key 값으로 사용됨.
  <br>

#### 5) onKeyPress 이벤트 핸들링하기

<br>

`EventPractice.js`

```jsx
import React, { Component } from 'react';

class EventPractice extends Component {
	state = {
		username: '',
		message: '',
	};

	handleChange = (e) => {
		this.setState({
			[e.target.name]: e.target.value,
		});
	};

	handleClick = () => {
		alert(this.state.username + ' : ' + this.state.message);
		this.setState({
			username: '',
			message: '',
		});
	};

	handleKeyPress = (e) => {
		if (e.key === 'Enter') {
			this.handleClick();
		}
	};

	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type='text'
					name='username'
					placeholder='사용자명'
					value={this.state.username}
					onChange={this.handleChange}
				/>
				<input
					type='text'
					name='message'
					placeholder='아무거나 입력해보세요'
					value={this.state.message}
					onChange={this.handleChange}
					onKeyPress={this.handleKeyPress}
				/>
				<button onClick={this.handleClick}>확인</button>
			</div>
		);
	}
}

export default EventPractice;
```

- 키를 눌렀을 때 발생하는 KeyPress 이벤트를 처리하는 방법
  - comment 인풋에서 Enter 키를 눌렀을 때 handleClick 메서드를 호출하도록 코드 작성
    <br>

## 4.3 함수형 컴포넌트로 구현해 보기

<br>

`EventPractice.js`

```jsx
import React, { useState } from 'react';

const EventPractice = () => {
	const [username, setUsername] = useState('');
	const [message, setMessage] = useState('');

	const onChangeUsername = (e) => setUsername(e.target.value);
	const onChangeMessage = (e) => setMessage(e.target.value);
	const onClick = () => {
		alert(username + ' : ' + message);
		setUsername('');
		setMessage('');
	};

	const onKeyPress = (e) => {
		if (e.key === 'Enter') {
			onClick();
		}
	};

	return (
		<div>
			<h1>이벤트 연습</h1>
			<input
				type='text'
				name='username'
				placeholder='사용자명'
				value={username}
				onChange={onChangeUsername}
			/>
			<input
				type='text'
				name='message'
				placeholder='아무거나 입력해보세요'
				value={message}
				onChange={onChangeMessage}
				onKeyPress={onKeyPress}
			/>
			<button onClick={onClick}>확인</button>
		</div>
	);
};

export default EventPractice;
```

- 위 코드에서는 `e.target.name` 을 활용하지 않고 onChange 관련 함수 2개 (onChangeUsername, onChangeMessage) 를 따로 만들어줌.
  - 인풋이 많아질수록 함수 개수가 늘어나면서 가독성이 떨어짐.
    <br>

`EventPractice.js`

```jsx
import React, { useState } from 'react';

const EventPractice = () => {
	const [form, setForm] = useState({
		username: '',
		message: '',
	});

	const { username, message } = form;

	const onChange = (e) => {
		const nextForm = {
			...form, // 기존의 form 내용을 이 자리에 복사한 뒤
			[e.target.name]: e.target.value, // 원하는 값으로 덮어 씌우기
		};
		setForm(nextForm); // setForm 함수를 통해 현재 form 갱신
	};

	const onClick = () => {
		alert(username + ' : ' + message);
		setForm({
			username: '',
			message: '',
		});
	};

	const onKeyPress = (e) => {
		if (e.key === 'Enter') {
			onClick();
		}
	};

	return (
		<div>
			<h1>이벤트 연습</h1>
			<input
				type='text'
				name='username'
				placeholder='사용자명'
				value={username}
				onChange={onChange}
			/>
			<input
				type='text'
				name='message'
				placeholder='아무거나 입력해보세요'
				value={message}
				onChange={onChange}
				onKeyPress={onKeyPress}
			/>
			<button onClick={onClick}>확인</button>
		</div>
	);
};

export default EventPractice;
```

- useState 를 통해 사용하는 상태에 문자열이 아닌 객체를 넣어줌으로써 간결하게 작성할 수 있음.
  - `e.target.name` 값을 활용하려면 위와 같이 useState 를 쓸 때 인풋 값들이 들어 있는 form 객체를 사용해주면 됨.
