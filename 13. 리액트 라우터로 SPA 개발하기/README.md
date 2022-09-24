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

<br>

`Profile.js`

```jsx
import React from "react";

const data = {
	velopert: {
		name: "김민준",
		description: "리액트를 좋아하는 개발자",
	},
	gildong: {
		name: "홍길동",
		description: "고전 소설 홍길동전의 주인공",
	},
};

const Profile = ({ match }) => {
	const { username } = match.params;
	const profile = data[username];
	if (!profile) {
		return <div>존재하지 않는 사용자입니다.</div>;
	}

	return (
		<div>
			<h3>
				{username}({profile.name})
			</h3>
			<p>{profile.description}</p>
		</div>
	);
};

export default Profile;
```

- URL 파라미터를 사용할 때는

  - 라우트로 사용되는 컴포넌트에서 받아 오는 `match` 객체 안의 `params` 값을 참조
  - match 객체 안에는 현재 컴포넌트가 어떤 경로 규칙에 의해 보이는지에 대한 정보가 들어있음.
    <br>

`App.js`

```jsx
import React from "react";

const data = {
	velopert: {
		name: "김민준",
		description: "리액트를 좋아하는 개발자",
	},
	gildong: {
		name: "홍길동",
		description: "고전 소설 홍길동전의 주인공",
	},
};

const Profile = ({ match }) => {
	const { username } = match.params;
	const profile = data[username];
	if (!profile) {
		return <div>존재하지 않는 사용자입니다.</div>;
	}

	return (
		<div>
			<h3>
				{username}({profile.name})
			</h3>
			<p>{profile.description}</p>
		</div>
	);
};

export default Profile;
```

- 라우트 path 규칙에 `/profile/:username` 이라고 써주면
  - `match.params.username` 값을 통해 현재 username 값 조회 가능
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/183809075-58e9c2ac-b947-4139-80c7-12b3f1461bce.png)

<br>

### 13.4.2 URL 쿼리

<br>

```jsx
// http://localhost:3000/About?detail=true 주소로 들어갔을 때의
// location 객체 형태

{
	"pathname": "/About",
	"search": "?detail=true",
	"hash": ""
}
```

- 쿼리는 location 객체에 들어 있는 search 값에서 조회 가능

  - location 객체는 라우트로 사용된 컴포넌트에게 props 로 전달되며,
  - 웹 애플리케이션의 현재 주소에 대한 정보를 지니고 있음.
    <br>

- location 객체의 search 값은 문자열 형태
  - search 값에서 특정 값을 읽어 오려면 이 문자열을 객체 형태로 변환해야 함.
  - 쿼리 문자열을 객체로 변환할 때는 `qs 라이브러리` 사용
    <br>

`About.js`

```jsx
import React from "react";
import qs from "qs";

const About = ({ location }) => {
	const query = qs.parse(location.search, {
		ignoreQueryPrefix: true, // 이 설정을 통해 문자열 맨 앞의 ? 을 생략
	});
	console.log(location);
	console.log(query);
	const showDetail = query.detail === "true"; // 쿼리의 파싱 결과값은 문자열
	return (
		<div>
			<h1>소개</h1>
			<p>이 프로젝트는 리액트 라우터 기초를 실습해 보는 예제 프로젝트</p>
			{showDetail && <p>detail 값을 true 로 설정하셨군요!</p>}
		</div>
	);
};

export default About;
```

- 쿼리 문자열을 객체로 파싱하면 결과 값은 언제나 문자열
  - 숫자나 논리 자료형도 "1", "true" 와 같이 문자열 형태로 받아짐.
  - 숫자를 받아 와야 하면 parseInt 함수를 통해 반드시 숫자로 변환
  - 논리 자료형 값을 사용해야 한다면 "true" 또는 "false" 문자열과 일치하는지 비교
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/184044077-314a3590-fc60-4612-81a0-43dde458ae52.png)

<br>

## 13.5 서브 라우트

- 라우트 내부에 또 라우트를 정의하는 것
  - 라우트로 사용되고 있는 컴포넌트 내부에 Route 컴포넌트를 한 번 더 사용하면 됨.

<br>

`Profiles.js`

```jsx
import React from "react";
import { Link, Route } from "react-router-dom";
import Profile from "./Profile";

const Profiles = () => {
	return (
		<div>
			<h3>사용자 목록</h3>
			<ul>
				<li>
					<Link to='/profiles/velopert'>Velopert</Link>
				</li>
				<li>
					<Link to='/profiles/gildong'>gildong</Link>
				</li>
			</ul>

			<Route path='/profiles' exact render={() => <div>사용자를 선택해주세요</div>} />
			<Route path='/profiles/:username' component={Profile} />
		</div>
	);
};

export default Profiles;
```

- 프로필 링크를 보여 주는 라우트 컴포넌트 Profiles 생성

  - 기존의 Profile 컴포넌트를 Profiles 컴포넌트 내부에서 서브 라우트로 사용
    <BR>

- 첫 번째 Route 컴포넌트에는 component 대신 render 라는 props 를 전달
  - 컴포넌트 자체를 전달하는 대신 보여 주고 싶은 JSX 를 넣어줄 수도 있음.

<br>

`App.js`

```jsx
import React from "react";
import { Link, Route } from "react-router-dom";
import Home from "./Home";
import About from "./About";
import Profiles from "./Profiles";

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
				<li>
					<Link to='/profiles/velopert'>velopert 프로필</Link>
				</li>
				<li>
					<Link to='/profiles/gildong'>gildong 프로필</Link>
				</li>
			</ul>
			<hr />
			<Route path='/' component={Home} exact />
			<Route path={["/About", "/Info"]} component={About} />
			<Route path='/profiles' component={Profiles} />
		</div>
	);
};

export default App;
```

- 기존의 App 컴포넌트에 있던 프로필 링크를 지우고,
  - Profiles 컴포넌트를 /profiles 경로에 연결

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/184048230-2e63189d-ad95-4456-8290-303c23f58a9b.png)

<br>

## 13.6 리액트 라우터 부가 기능

### 13.6.1 history

- history 객체는 라우트로 사용된 컴포넌트에 match, location 과 함께 전달되는 props 중 하나
  - 이 객체를 통해 컴포넌트 내에 구현하는 메서드에서 라우터 API 호출 가능
  - ex) 로그인 후 화면을 전환하거나, 다른 페이지로 이탈하는 것을 방지해야 할 때 사용
    <br>

`HistorySample.js`

```jsx
import React, { Component } from "react";

class HistorySample extends Component {
	// 뒤로 가기
	handleGoBack = () => {
		this.props.history.goBack();
	};

	// 홈으로 이동
	handleGoHome = () => {
		this.props.history.push("/");
	};

	componentDidMount() {
		// 페이지에 변화가 생기려고 할 때마다 정말 나갈 것인지를 질문
		this.unblock = this.props.history.block("정말 떠나실 건가요?");
	}

	componentWillUnmount() {
		// 컴포넌트가 언마운트되면 질문을 멈춤
		if (this.unblock) {
			this.unblock();
		}
	}

	render() {
		return (
			<div>
				<button onClick={this.handleGoBack}>뒤로</button>
				<button onClick={this.handleGoHome}>홈으로</button>
			</div>
		);
	}
}

export default HistorySample;
```

- 현재 페이지를 이탈하려고 할 때 브라우저 메세지 창이 나타남.

<br>

`App.js`

```jsx
import React from "react";
import { Link, Route } from "react-router-dom";
import Home from "./Home";
import About from "./About";
import Profiles from "./Profiles";
import HistorySample from "./HistorySample";

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
				<li>
					<Link to='/profiles/velopert'>velopert 프로필</Link>
				</li>
				<li>
					<Link to='/profiles/gildong'>gildong 프로필</Link>
				</li>
				<li>
					<Link to='/history'>History 예제</Link>
				</li>
			</ul>
			<hr />
			<Route path='/' component={Home} exact={true} />
			<Route path={["/About", "/Info"]} component={About} />
			<Route path='/profiles' component={Profiles} />
			<Route path='/history' component={HistorySample} />
		</div>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/184049137-7a6691ae-c953-489a-9081-e63258ccf9d4.png)

<br>

### 13.6.2 withRouter

- withRouter 함수는 HoC (Higher-order Component)
  - 라우트로 사용된 컴포넌트가 아니어도 match, location, history 객체를 접근 가능하게 함.

<br>

`WithRouterSample.js`

```jsx
import React from "react";
import { withRouter } from "react-router-dom";

const WithRouterSample = ({ location, match, history }) => {
	return (
		<div>
			<h4>location</h4>
			<textarea value={JSON.stringify(location, null, 2)} rows={7} readOnly={true} />
			<h4>match</h4>
			<textarea value={JSON.stringify(match, null, 2)} rows={7} readOnly={true} />
			<button onClick={() => history.push("/")}>홈으로</button>
		</div>
	);
};

export default withRouter(WithRouterSample);
```

- withRouter 를 사용할 때는 컴포넌트를 내보내 줄 때 함수로 감싸줘야 함.
- JSON.stringify(param1, null, 2) 와 같이 쓰면 JSON 에 들여쓰기가 적용된 상태로 문자열 생성

<br>

`Profiles.js`

```jsx
import React from "react";
import { Link, Route } from "react-router-dom";
import Profile from "./Profile";
import WithRouterSample from "./WithRouterSample";

const Profiles = () => {
	return (
		<div>
			<h3>사용자 목록</h3>
			<ul>
				<li>
					<Link to='/profiles/velopert'>Velopert</Link>
				</li>
				<li>
					<Link to='/profiles/gildong'>gildong</Link>
				</li>
			</ul>

			<Route path='/profiles' exactrender={() => <div>사용자를 선택해주세요</div>} />
			<Route path='/profiles/:username' component={Profile} />
			<WithRouterSample />
		</div>
	);
};

export default Profiles;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/184049563-0eaffe02-c906-422a-99f9-389447d02de3.png)

<br>

- match 객체의 params 가 비어 있는 이유는
  - withRouter 사용 시 현재 자신을 보여주고 있는 라우트 컴포넌트를 기준으로 match 가 전달되기 때문
    <br>
- Profiles 를 위한 라우트를 설정할 때는 path="/profiles" 라고만 입력했으므 username 파라미터를 읽어 오지 못하는 상태

`Profile.js`

```jsx
import React from "react";
import { withRouter } from "react-router-dom";
import WithRouterSample from "./WithRouterSample";

const data = {
	velopert: {
		name: "김민준",
		description: "리액트를 좋아하는 개발자",
	},
	gildong: {
		name: "홍길동",
		description: "고전 소설 홍길동전의 주인공",
	},
};

const Profile = ({ match }) => {
	const { username } = match.params;
	const profile = data[username];
	if (!profile) {
		return <div>존재하지 않는 사용자입니다.</div>;
	}

	return (
		<div>
			<h3>
				{username}({profile.name})
			</h3>
			<p>{profile.description}</p>
			<WithRouterSample />
		</div>
	);
};

export default withRouter(Profile);
```

- WithRouterSample 컴포넌트를 Profiles 에서 지우고,
  - Profile 컴포넌트에 넣으면 match 쪽에 URL 파라미터가 제대로 보임.

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/184051564-4ed7ce82-644a-474d-af14-2d3561592f11.png)

<br>

### 13.6.3 Switch

- 여러 Router 를 감싸서 그중 일치하는 단 하나의 라우트만을 렌더링
  - Switch 를 사용하면 모든 규칙과 일치하지 않을 때 보여 줄 Not Found 페이지도 구현 가능

<br>

`App.js`

```jsx
import React from "react";
import { Link, Route, Switch } from "react-router-dom";
import Home from "./Home";
import About from "./About";
import Profiles from "./Profiles";
import HistorySample from "./HistorySample";

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
				<li>
					<Link to='/profiles/velopert'>velopert 프로필</Link>
				</li>
				<li>
					<Link to='/profiles/gildong'>gildong 프로필</Link>
				</li>
				<li>
					<Link to='/history'>History 예제</Link>
				</li>
			</ul>
			<hr />
			<Switch>
				<Route path='/' component={Home} exact={true} />
				<Route path={["/About", "/Info"]} component={About} />
				<Route path='/profiles' component={Profiles} />
				<Route path='/history' component={HistorySample} />
				<Route
					// path 를 따로 정의하지 않으면 모든 상황에 렌더링됨
					render={({ location }) => (
						<div>
							<h2>이 페이지는 존재하지 않습니다.</h2>
							<p>{location.pathname}</p>
						</div>
					)}
				/>
			</Switch>
		</div>
	);
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/184050214-f13cfbd3-48a0-4e2c-8331-2329a2348ad8.png)

<br>

### 13.6.4 NavLink

- NavLink 는 현재 경로와 Link 에서 사용하는 경로가 일치하는 경우
  - 특정 스타일 혹은 CSS 클래스를 적용할 수 있는 컴포넌트
    <br>
- NavLink 에서 링크가 활성화되었을 때의 스타일을 적용할 때는 activeStyle 값을,
  - CSS 클래스를 적용할 때는 activeClassName 값을 props 로 넣어주면 됨.
    <br>

`Profile.js`

```jsx
import React from "react";
import { NavLink, Route } from "react-router-dom";
import Profile from "./Profile";
import WithRouterSample from "./WithRouterSample";

const Profiles = () => {
	const activeStyle = {
		background: "black",
		color: "white",
	};

	return (
		<div>
			<h3>사용자 목록</h3>
			<ul>
				<li>
					<NavLink activeStyle={activeStyle} to='/profiles/velopert'>
						Velopert
					</NavLink>
				</li>
				<li>
					<NavLink activeStyle={activeStyle} to='/profile/gildong'>
						gildong
					</NavLink>
				</li>
			</ul>

			<Route path='/profiles' exact render={() => <div>사용자를 선택해주세요</div>} />
			<Route path='/profiles/:username' component={Profile} />
			<WithRouterSample />
		</div>
	);
};

export default Profiles;
```

- Profiles 에서 사용하고 있는 컴포넌트에서 Link 대신 NavLink 를 사용
  - 현재 선택되어 있는 경우 검정색 배경에 흰색 글씨로 스타일을 보여 주게끔 작성
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/184050899-ff0ff24d-aff4-42fb-aea3-38113bbe2578.png)
