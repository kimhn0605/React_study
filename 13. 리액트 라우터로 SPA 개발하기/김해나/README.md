# 13. 리액트 라우터로 SPA 개발하기

## 13.1 SPA 란?

> 💡 Single Page Application 의 약어로, 한 개의 페이지로 이루어진 애플리케이션을 의미

<br>

- **`기존 웹 페이지`**

  - 다른 페이지로 이동할 때마다 새로운 html 을 받아 오고, 페이지를 로딩할 때마다 서버에서 리소스를 전달받아 해석한 뒤 화면에 나타냄.
    - 바뀌지 않는 부분까지 새로 불러와서 보여 주어야 하기 때문에 불필요한 로딩 발생
      <Br>
  - 서버 측에서 모든 뷰를 준비하게 되어 성능상의 문제 발생
    - 사용자가 몰리면 서버에 높은 부하가 쉽게 걸릴 수 있음.
      <br>

- **`현재 웹 페이지 (리액트 사용)`**

  - 뷰 렌더링을 사용자의 브라우저가 담당하도록
  - 사용자와의 인터랙션이 발생하면 필요한 부분만 자바스크립트를 사용하여 업데이트 - 새로운 데이터가 필요하다면 서버 API 를 호출하여 필요한 데이터만 새로 불러와서 사용
    <br>

- **`SPA 의 단점`**
  - 앱의 규모가 커지면 자바스크립트 파일이 너무 커진다는 것
    - 코드 스플리팅을 사용하면 라우트별로 파일들을 나눠서 트래픽과 로딩 속도 개선 가능

<br>

## 13.2 프로젝트 준비 및 기본적인 사용법

### 13.2.1 프로젝트 생성 및 라이브러리 설치

<br>

```python
npm i react-router-dom
```

- 리액트 라우터 설치
  <br>

### 13.2.2 프로젝트에 라우터 적용

<br>

`src.index.js`

```jsx
import React from "react";
import { createRoot } from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

root.render(
	<BrowserRouter>
		<App />
	</BrowserRouter>
);
```

- 프로젝트에 라우터를 적용하려면

  - src/index.js 파일에서 react-router-dom 에 내장되어 있는 BrowserRouter 컴포넌트로 감싸줘야 함.
    <br>

- **`BrowserRouter 컴포넌트`**
  - Historty API 를 사용하여 페이지를 새로고침하지 않고도 주소를 변경하고,
  - 현재 주소에 관련된 정보를 props 로 쉽게 조회하거나 사용할 수 있도록 함.
    <br>

### 13.2.3 페이지 만들기

- 라우트로 사용할 페이지 컴포넌트 생성
  - `Home` : 사용자가 웹 사이트에 들어왔을 때 맨 처음 보여 줄 컴포넌트
  - `About` : 웹 사이트를 소개하는 컴포넌트
    <br>

`Home.js`

```jsx
import React from "react";

const Home = () => {
	return (
		<div>
			<h1>홈</h1>
			<p>이 페이지는 웹 접속 시 가장 먼저 보여지는 페이지</p>
		</div>
	);
};

export default Home;
```

<br>

`About.js`

```jsx
import React from "react";

const About = () => {
	return (
		<div>
			<h1>소개</h1>
			<p>이 프로젝트는 리액트 라우터 기초를 실습해 보는 예제 프로젝트</p>
		</div>
	);
};

export default About;
```

<br>

### 13.2.4 Route 컴포넌트로 특정 주소에 컴포넌트 연결

<br>

```jsx
<Route path="주소 규칙" component={보여 줄 컴포넌트} />
```

- Route 컴포넌트를 사용하여 사용자의 현재 경로에 따라 다른 컴포넌트를 보여줌.
  - 어떤 규칙을 가진 경로에 어떤 컴포넌트를 보여 줄지 정의 가능

<br>

`App.js`

```jsx
import React from "react";
import { Route } from "react-router-dom";
import Home from "./Home";
import About from "./About";

const App = () => {
	return (
		<div>
			<Route path='/' component={Home} />
			<Route path='/About' component={About} />
		</div>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/183803420-77906459-6804-4ca1-a89f-f6217dda7f98.png)

<Br>

- /About 경로에서 두 컴포넌트 모두 나타나는 이유는 /About 경로가 / 규칙에도 일치하기 때문
  - 이를 수정하려면 exact 라는 props 를 true 로 설정
    <br>

```jsx
<Route path='/' component={Home} exact={true} />
```

![image](https://user-images.githubusercontent.com/77706631/183804512-8fed5a83-4b0a-4925-a645-5e346c9eeffd.png)

<br>

### 13.2.5 Link 컴포넌트를 사용하여 다른 주소로 이동하기

<br>

- **`a 태그`**

  - 페이지 전환 시 기존의 상태들을 모두 날려 버리고 새로 불러옴.
  - 렌더링된 컴포넌트들도 모두 사라지고 다시 처음부터 렌더링되기 때문에 리액트 라우터에서 사용 X
    <br>

- **`Link 컴포넌트`**

  - 클릭 시 다른 주소로 이동시켜 주는 컴포넌트
  - 페이지 전환 시 새로 불러오지 않고, 애플리케이션은 그대로 유지한 상태에서 페이지의 주소만 변경
  - `Link 컴포넌트 = a 태그 + 페이지 전환 방지 기능`
    <br>

##### ※ Link 컴포넌트 사용 예시

```jsx
<Link to='주소'>내용</Link>
```

<br>

`App.js`

```jsx
import React from "react";
import { Link, Route } from "react-router-dom";
import Home from "./Home";
import About from "./About";

const App = () => {
	return (
		<div>
			<ul>
				<li>
					<Link to='/'>홈</Link>
				</li>
				<li>
					<Link to='/About'>소개</Link>
				</li>
			</ul>
			<hr />
			<Route path='/' component={Home} exact={true} />
			<Route path='/About' component={About} />
		</div>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/183807170-2302ce1f-78b5-4733-ba80-7fd19c08fc20.png)

<br>

## 13.3 Route 하나에 여러 개의 path 설정하기

**`기존 버전`**

```jsx
<Route path="/About" component={About} />
<Route path="/Info" component={About} />
```

- 여러 개의 path 에 같은 컴포넌트를 보여주려면 위와 같이 Route 를 여러 개 사용했어야 함.

<br>

**`최신 버전`**

```jsx
<Route path={["/About", "/Info"]} component={About} />
```

- Route 하나에 여러 개의 path 를 지정하는 것은 최신 버전의 리액트 라우터 v5 부터 적용된 기능
  - path props 를 배열로 설정해 주면 여러 경로에서 같은 컴포넌트를 보여줄 수 있음.

<br>

## 13.4 URL 파라미터와 쿼리

- 페이지 주소를 정의할 때 유동적인 값을 전달해야 할 때

  - **`파라미터`**
    - 특정 아이디 혹은 이름을 사용하여 조회할 때 사용
    - ex) /profile/velopert

  <Br>

  - **`쿼리`**
    - 쿼리는 특정 키워드를 검색하거나 페이지에 필요한 옵션을 전달할 때 사용
    - ex) /about?details=true
      <br>

### 13.4.1 URL 파라미터

## 13.5 서브 라우트

## 13.6 리액트 라우터 부가 기능
