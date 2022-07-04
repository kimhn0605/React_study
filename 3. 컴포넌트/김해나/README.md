# 3. 컴포넌트

## 3.1 클래스형 컴포넌트

`App.js`

```jsx
import React, { Component } from "react";

class App extends Component {
	render() {
		return <div></div>;
	}
}

export default App;
```

- **클래스형 컴포넌트**

  - render() 함수가 반드시 있어야 하고, 그 안에서 보여줘야 할 JSX 를 반환
    <br>

- **클래스형 컴포넌트와 함수형 컴포넌트의 차이점**

  - 클래스형 컴포넌트

    - state 기능 및 라이프사이클 기능 사용 가능
    - 임의 메서드 정의 가능
      <br>

  - 함수형 컴포넌트
    - 사용되는 메모리 자원이 적음.
    - state 와 라이프사이클 API 사용이 불가능했지만 v16.8 업데이트 이후 Hooks 기능이 도입되면서 해결
      <br>

- **ES6의 클래스 문법**
  - ES6 이전에는 자바스크립트에 클래스가 없었기 때문에 prototype 이라는 문법을 사용하여 코드 작성

`ES6 이전`

```jsx
function Dog(name) {
	this.name = name;
}

Dog.prototype.say = function () {
	console.log(this.name + " : 멍멍");
};

var dog = new Dog("검둥이");
dog.say(); // 검둥이 : 멍멍
```

<br>

`ES6 이후`

```jsx
class Dog {
  constructor(name) {
    this.name = name;
  }
  say() {
    console.log(this.name + ' : 멍멍");
  }
}

const dog = new Dog('흰둥이');
dog.say();    // 흰둥이 : 멍멍
```

<br>

## 3.2 첫 컴포넌트 생성

### 3.2.1 src 디렉토리에 MyComponent.js 파일 생성

- 컴포넌트를 만드려면 컴포넌트 코드를 선언할 파일을 먼저 생성
  <br>

### 3.2.2 코드 작성하기

`MyComponent.js`

```jsx
import React from "react";

const MyComponenet = () => {
	return <div>첫 컴포넌트 생성</div>;
};

export default App;
```

- 함수 작성 시 function 키워드 대신 화살표 함수 문법 () => {...} 사용

<br>

**ES6 의 화살표 함수**

- ES6 에서 도입된 화살표 함수는 주로 함수를 파라미터로 전달할 때 유용
  <br>

```jsx
setTimeout(function () {
	console.log("hello world");
}, 1000);

setTimeout(() => {
	console.log("hello world");
}, 1000);
```

- 화살표 함수 문법이 기존의 function 을 대체할 수 없는 이유는 서로 가리키고 있는 this 값이 다름.

<br>

```jsx
function BlackDog() {
  this.name = '흰둥이';

  return {
    name: '검둥이';
    bark: function() {
      console.log(this.name + '멍멍!');
    }
  }
}

const blackDog = new BlackDog();
blackDog.bark();    // 검둥이 멍멍!
```

```jsx
function WhiteDog() {
  this.name = '흰둥이';

  return {
    name: '검둥이';
    bark: () => {
      console.log(this.name + '멍멍!');
    }
  }
}
const whitekDog = new WhiteDog();
whiteDog.bark();    // 흰둥이 멍멍!
```

- 일반 함수는 자신이 종속된 객체를 this 로 가리키지만 화살표 함수는 자신이 종속된 인스턴스를 가리킴.

<br>

```jsx
function twice(value) {
	return value * 2;
}

const triple = (value) => value * 3;
```

- 화살표 함수는 값을 연산하여 바로 반환해야 할 때는 중괄호 {} 를 생략해주면 됨.
  <br>

### 3.2.3 모듈 내보내기 및 불러오기

**1) 모듈 내보내기 (export)**

```jsx
export default MyComponent;
```

- MyComponent.js 맨 아래에 작성되어 있는 이 코드는 <u>다른 파일에서 이 파일을 import 할 때, 위에서 선언한 MyComponent 클래스를 불러오도록 설정</u> 하는 역할\*\*

<br>

**2) 모듈 불러오기 (import)**

`App.js`

```jsx
import React from "react";
import MyComponent from "./MyComponent";

const App = () => {
	return <MyComponent />;
};

export default App;
```

- import 를 이용하여 App 컴포넌트에서 MyComponent 컴포넌트를 불러와서 사용

<br>

## 3.3 props

- props 는 properties 를 줄인 표현으로, **컴포넌트 속성을 설정할 때 사용하는 요소**
  - props 값은 <u>해당 컴포넌트를 불러와 사용하는 부모 컴포넌트에서 설정 가능</u>

<br>

### 3.3.1 JSX 내부에서 props 렌더링

<br>

`MyComponent.js`

```jsx
import React from "react";

const MyComponent = (props) => {
	return <div>안녕하세요, 제 이름은 {props.name} 입니다. </div>;
};

export default MyComponent;
```

- MyComponent 컴포넌트를 수정하여 해당 컴포넌트에서 name 이라는 props 를 렌더링하도록 설정
  <br>

### 3.3.2 컴포넌트를 사용할 때 props 값 지정하기

<br>

`App.js`

```jsx
import React from "react";
import MyComponent from "./MyComponent";

const App = () => {
	return <MyComponent name='React' />;
};

export default App;
```

- App 컴포넌트에서 MyComponent 의 props 값을 지정

<br>

### 3.3.3 props 기본값 설정 : defaultProps

<br>

`MyComponent.js`

```jsx
import React from 'react';

const MyComponent = props => {
  return <div>안녕하세요, 제 이름은 {props.name} 입니다. </div>;
}

MyComponent.defaultProps = {
  name: '기본 이름';
}

export default MyComponent;
```

- defaultProps 를 통해 App 컴포넌트에서 props 값을 따로 지정하지 않았을 때 보여 줄 기본값을 설정

<br>

### 3.3.4 태그 사이의 내용을 보여주는 children

<br>

`App.js`

```jsx
import React from "react";
import MyComponent from "./MyComponent";

const App = () => {
	return <MyComponent>리액트</MyComponent>;
};

export default App;
```

<br>

`MyComponent.js`

```jsx
import React from 'react';

const MyComponent = props => {
  return (
    <div>
      안녕하세요, 제 이름은 {props.name} 입니다. <br />
      children 값은 {props.children} 입니다.
   </div>
  );
};

MyComponent.defaultProps = {
  name: '기본 이름';
}

export default MyComponent;
```

- **children**

  - 컴포넌트 태그 사이의 내용을 보여주는 props
    <br>

- MyComponent 태그 사이에 작성한 리액트라는 문자열을 MyComponent 내부에서 보여 주려면 props.children 값을 보여줘야 함.
  <br>

### 3.3.5 비구조화 할당 문법을 통해 props 내부 값 추출하기

<br>

`MyComponent.js`

```jsx
import React from 'react';

const MyComponent = props => {
 const { name, children } = props;

 return (
    <div>
      안녕하세요, 제 이름은 {name} 입니다. <br />
      children 값은 {children} 입니다.
   </div>
  );
}

MyComponent.defaultProps = {
  name: '기본 이름';
}

export default MyComponent;
```

- 앞에 props 키워드를 일일이 붙여주는 것보단 **ES6 의 비구조화 할당 문법을 사용하면 내부 값을 바로 추출 가능**
  <br>

```jsx
import React from 'react';

const MyComponent = ({ name, children }) => {

 return (
    <div>
      안녕하세요, 제 이름은 {name} 입니다. <br />
      children 값은 {children} 입니다.
   </div>
  );
};

MyComponent.defaultProps = {
  name: '기본 이름';
}

export default MyComponent;
```

- 만약 함수의 파라미터가 객체라면 비구조화 할당 문법을 통해 해당 값을 바로 비구조화해서 사용하는 것도 가능
  <br>

### 3.3.6 propTypes 를 통한 props 검증

<br>

`MyComponent.js`

```jsx
import React from "react";
import PropTypes from 'prop-types';

const MyComponent = ({ name, children }) => {
  return (...);
};

MyComponent.defaultProps = {
  name: '기본 이름';
}

MyComponent.propTypes = {
  name: PropTypes.string
};

export default MyComponent;
```

<br>

`App.js`

```jsx
import React from "react";
import MyComponent from "./MyComponent";

const App = () => {
	// return <MyComponent name={3}>리액트</MyComponent>; -> 오류 발생
	return <MyComponent name='React'>리액트</MyComponent>;
};

export default App;
```

- **propTypes 를 사용하면 컴포넌트의 필수 props 를 지정하거나 props 의 타입을 지정 가능**
  - 위 코드는 name 값을 무조건 문자열 형태로 전달해야 함.
  - App 컴포넌트에서 문자열이 아닌 다른 자료형으로 전달하면 오류 발생
    <br>

`MyComponent.js`

```jsx
import React from "react";
import PropTypes from 'prop-types';

const MyComponent = ({ name, favoriteNumber, children }) => {
 return (
    <div>
      안녕하세요, 제 이름은 {name} 입니다. <br />
      children 값은 {children} 입니다. <br />
      제가 좋아하는 숫자는 {favoriteNumber} 입니다.
   </div>
  );
};

MyComponent.defaultProps = {
  name: '기본 이름';
}

MyComponent.propTypes = {
  name: PropTypes.string,
  favoriteNumber: PropTypes.number.isRequired
};

export default MyComponent;
```

<br>

`App.js`

```jsx
import React from "react";
import MyComponent from "./MyComponent";

const App = () => {
	return (
		<MyComponent name='React' favoriteNumber={1}>
			리액트
		</MyComponent>
	);
};

export default App;
```

- **isRequired 를 사용하면 필수 propsTypes 지정 가능**
  - 만약 App 컴포넌트에서 favoriteNumber 값을 주지 않았다면 오류 발생
    <br>

### 3.3.7 클래스형 컴포넌트에서 props 사용하기

<br>

`MyComponent.js`

```jsx
import React from "react";
import PropTypes from 'prop-types';

class MyComponent extends Component {
  render() {
    const { name, favoriteNumber, children } = this.props;  // 비구조화 할당

    return (
      <div>
        안녕하세요, 제 이름은 {name} 입니다. <br />
        children 값은 {children} 입니다. <br />
        제가 좋아하는 숫자는 {favoriteNumber} 입니다.
      </div>
    )
  }
}

MyComponent.defaultProps = {
  name: '기본 이름';
}

MyComponent.propTypes = {
  name: PropTypes.string
  favoriteNumber: PropTypes.number.isRequired
};

export default MyComponent;
```

- **클래스 컴포넌트에서 props 를 사용할 때는 render 함수에서 this.props 를 조회**
  - defaultProps 와 propTypes 는 동일한 방식으로 설정 가능
    <Br>

```jsx
import React from "react";
import PropTypes from 'prop-types';

class MyComponent extends Component {
  static defaultProps = {
    name: '기본 이름';
  };

  static propsTypes = {
    name: PropTypes.string,
    favoriteNumber: PropTypes.number.isrequired
  };

  render() {
    const { name, favoriteNumber, children } = this.props;  // 비구조화 할당

    return (
      <div>
        안녕하세요, 제 이름은 {name} 입니다. <br />
        children 값은 {children} 입니다. <br />
        제가 좋아하는 숫자는 {favoriteNumber} 입니다.
      </div>
    )
  }
}

export default MyComponent;
```

- 위와 같이 class 내부에서 defaultProps 와 propTypes 지정하는 방법도 존재
  - defaultProps 와 propTypes 는 필수 X
    <br>

## 3.4 state

- **리액트에서 state 는 컴포넌트 내부에서 바뀔 수 있는 값을 의미**

  - props 는 부모 컴포넌트가 설정하는 값이며, 컴포넌트 자신은 해당 props 를 읽기 전용으로만 사용 가능
    <br>

- **state 종류**
  - 클래스형 컴포넌트가 지니고 있는 state
  - 함수형 컴포넌트에서 useState 함수를 통해 사용하는 state
    <br>

### 3.4.1 클래스형 컴포넌트의 state

<br>

`Counter.js`

```jsx
import React from "react";

class Counter extends Component {
	constructor(props) {
		super(props);
		// state 초깃값 설정
		this.state = {
			number: 0,
		};
	}

	render() {
		const { number } = this.state; // state 조회

		return (
			<div>
				<h1>{number}</h1>
				<button
					//onClick 을 통해 버튼 클릭 시 호출할 함수를 지정
					onClick={() => {
						// this.setState 를 통해 state 에 새로운 값을 넣어줌.
						this.setState({ number: number + 1 });
					}}
				>
					+1
				</button>
			</div>
		);
	}
}

export default Count;
```

- `constructor` 는 컴포넌트의 생성자 메서드로서, 클래스형 컴포넌트에서 constructor 를 작성 시에는 super(props) 를 반드시 호출
  - 이 함수가 호출되면 상속받고 있는 리액트의 Component 클래스가 지닌 생성자 함수를 호출해줌.
- `this.state` 를 통해 초깃값 설정

  - 컴포넌트의 state 는 객체 형식이어야 함.

- render 함수에서 현재 state 를 조회할 때는 this.state 를 조회
  - button 클릭 시 함수를 호출하여 이벤트를 설정
  - 이벤트로 설정할 함수를 넣어줄 대는 화살표 함수 문법 사용
  - `this.setState` 를 통해 state 값을 변경

<br>

`Counter.js`

```jsx
import React from "react";

class Counter extends Component {
	constructor(props) {
		super(props);
		// state 초깃값 설정
		this.state = {
			number: 0,
			fixedNumber: 0,
		};
	}

	render() {
		const { number } = this.state; // state 조회

		return (
			<div>
				<h1>{number}</h1>
				<h1>바뀌지 않는 값 : {fixedNumber}</h1>
				<button
					//onClick 을 통해 버튼 클릭 시 호출할 함수를 지정
					onClick={() => {
						// this.setState 를 통해 state 에 새로운 값을 넣어줌.
						this.setState({ number: number + 1 });
					}}
				>
					+1
				</button>
			</div>
		);
	}
}

export default Count;
```

- state 객체 안에 여러 값이 있더라도 this.setState 함수에 인자로 전달된 객체 안에 들어 있는 값만 변경

<br>

`Counter.js`

```jsx
import React from "react";

class Counter extends Component {
  state = {
    number: 0,
    fixedNubber: 0
  }
;
	render() {
		...
	}
}

export default Count;
```

- constructor 메서드 대신 위 방식으로도 state 초깃값 지정 가능

<br>

`Counter.js`

```jsx
onClick={() => {
  // this.setState 를 사용하여 state 에 새로운 값 지정
  this.setState({ number : number + 1 });
  this.setState({ number : this.state.number + 1 });
}}
```

- this.setState 를 사용하여 state 값을 업데이트할 때는 비동기적으로 업데이트됨.
  - 하지만 state 값이 바로 바뀌지는 않기 때문에 위 코드를 실행하면 숫자가 1씩 더해짐.
    <br>

```jsx
this.setState((prevState, props) => {
	return {
		// 업데이트하고 싶은 내용
	};
});
```

- this.setState 를 사용할 때 객체 대신에 함수를 인자로 넣어줘야 숫자가 2씩 증가
  - prevState : 기존 상태
  - props : 현재 지니고 있는 props (생략 가능)

<br>

`Counter.js`

```jsx
<button
	//onClick 을 통해 버튼이 클릭되었을 때 호출할 함수를 지정
	onClick={() => {
		this.setState((prevState) => {
			return {
				number: prevState.number + 1,
			};
		});

		// 위 코드와 아래 코드는 완전히 똑같은 기능 수행
		// 아래 코드는 함수에서 바로 객체를 반환한다는 의미
		this.setState((prevState) => ({
			number: prevState.number + 1,
		}));
	}}
>
	+1
</button>
```

<br>

`Counter.js`

```jsx
<button
	// onClick 을 통해 버튼이 클릭되었을 때 호출할 함수를 지정
	onClick={() => {
		this.setState(
			{
				number: number + 1,
			},
			() => {
				console.log("방금 setState 가 호출되었습니다.");
				console.log(this.state);
			}
		);
	}}
>
	+1
</button>
```

- setState 를 사용하여 값을 업데이트하고 난 다음 특정 작업을 수행하고 싶을 때는 두 번째 파라미터로 콜백 함수를 등록하여 작업 처리 가능
  <br>

### 3.4.2 함수형 컴포넌트에서 useState 사용하기

- 리액트 16.8 이전 버전에서는 함수형 컴포넌트에서 state 사용 불가
- 이후 Hooks 를 통해 함수형 컴포넌트에서도 useState 함수를 사용하면 state 사용이 가능해짐.
  <br>

```jsx
const array = [1, 2];
const [one, two] = array;
```

- 배열 비구조화 할당을 통해 배열 안에 들어있는 값을 쉽게 추출 가능
  <br>

`Say.js`

```jsx
import React from "react";

const Say = () => {
	const [message, setMessage] = useState("");
	const onClickEnter = () => setMessage("안녕하세요!");
	const onClickLeave = () => setMessage("안녕히 가세요!");

	return (
		<div>
			<button onClick={onClickEnter}>입장</button>
			<button onClick={onClickLeave}>퇴장</button>
			<h1>{message}</h1>
		</div>
	);
};

export default Say;
```

- **useState 함수 인자에는 상태의 초깃값을 지정**

  - 클래스형 컴포넌트에서의 state 초깃값은 객체 형태여야 하지만 useState 에서는 상관 X
    <br>

- **함수 호출 시 배열 반환**
  - 배열의 첫 번째 원소 : 현재 상태
  - 배열의 두 번째 원소 : 상태를 바꾸어 주는 함수
    <br>

`Say.js`

```jsx
import React from "react";

const Say = () => {
	const [message, setMessage] = useState("");
	const onClickEnter = () => setMessage("안녕하세요!");
	const onClickLeave = () => setMessage("안녕히 가세요!");

	const [color, setColor] = useState("black");

	return (
		<div>
			<button onClick={onClickEnter}>입장</button>
			<button onClick={onClickLeave}>퇴장</button>
			<h1 style={{ color }}>{message}</h1>

			<button style={{ color: "red" }} onClick={() => setColor("red")}>
				빨간색
			</button>
			<button style={{ color: "green" }} onClick={() => setColor("green")}>
				초록색
			</button>
			<button style={{ color: "blue" }} onClick={() => setColor("blue")}>
				파란색
			</button>
		</div>
	);
};

export default Say;
```

- 한 컴포넌트에서 useState 를 여러 번 사용해도 무관
  <br>

## 3.5 state 를 사용할 때 주의 사항

- 클래스형 컴포넌트든 함수형 컴포넌트든 <u> state 값을 바꾸어야 할 때는 setState 혹은 useState 를 통해 전달받은 세터 함수를 사용해야 함.</u>
  <br>

```jsx
// 객체 다루기
const object = { a : 1, b : 2, c : 3 };
const nextObject = { ...object, b : 2};   // 사본 생성 후 b 값만 덮어쓰기

// 배열 다루기
const array = [
  { id : 1, value : true },
  { id : 2, value : true },
  { id : 3, value : false }
];

let nextArray = array.concat({ id : 4 });   // 새 항목 추가
nextArray.filter(item => item.id !== 2);    // id 가 2인 항목 제거

// id 가 1인 항목의 value 를 false 로 설정
nextArray.map(item => (item.id === 1 ? { ...item, value : false }));
```

- **배열/객체를 업데이트해야 할 때는 사본을 만들어두고, 그 사본에 값을 업데이트한 후 setState 혹은 세터 함수를 통해 사본의 상태를 업데이트해야 함.**
  - 객체의 사본을 만들 때는 스프레드 연산자 ... 를 사용하여 처리
  - 배열의 사본을 만들 때는 배열의 내장 함수들을 활용
