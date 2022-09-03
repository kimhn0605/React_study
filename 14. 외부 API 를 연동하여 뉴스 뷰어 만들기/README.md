# 14. 외부 API 를 연동하여 뉴스 뷰어 만들기

## 14.1 비동기 작업의 이해

<br>

<img style="height:300px;" src="https://user-images.githubusercontent.com/77706631/187809502-f17a2e2d-3826-4d3e-8268-a166132cc0c9.png">

<br><br>

#### 동기적 처리

- 요청이 끝날 때까지 기다리는 동안 중지 상태가 되기 때문에 다른 작업을 할 수 없음.
- 요청이 끝나야 비로소 그다음 예정된 작업을 할 수 있음.
  <br>

#### 비동기적 처리

- 동시에 여러 가지 요청을 처리할 수도 있고, 기다리는 과정에서 다른 함수 호출도 가능
- ex) setTimeout() 함수를 사용하여 특정 작업을 예약할 때
  <br>

### 14.1.1 콜백 함수

<br>

```js
function increase(number, callback) {
	setTimeout(() => {
		const result = number + 10;
		if (callback) {
			callback(result);
		}
	}, 1000);
}

increase(0, (result) => {
	console.log(result);
});
```

- 파라미터 값이 주어지면 1초 뒤에 10을 더해서 반환하는 함수

<br>

```js
function increase(number, callback) {
	setTimeout(() => {
		const result = number + 10;
		if (callback) {
			callback(result);
		}
	}, 1000);
}

console.log("작업 시작");
increase(0, (result) => {
	console.log(result);
	increase(result, (result) => {
		console.log(result);
		increase(result, (result) => {
			console.log(result);
			increase(result, (result) => {
				console.log(result);
			});
		});
	});
});
```

- 콜백 안에 또 콜백을 넣어서 구현할 경우, `콜백 지옥` 발생
  - 여러 번 중첩될 경우 가독성 ↓
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/187810923-e23979ed-e4f8-4bed-b2d9-c718c2f779c1.png)

<Br>

### 14.1.2 Promise

> 💡 Promise 는 콜백 지옥 같은 코드가 형성되지 않도록 ES6 에 도입된 기능

<br>

```js
function increase(number) {
	const promise = new Promise((resolve, reject) => {
		// resolve 는 성공, reject 는 실패
		setTimeout(() => {
			const result = number + 10;
			if (result > 50) {
				// 50 보다 높으면 에러 발생시키기
				const e = new Error("NumberTooBig");
				return reject(e);
			}
			resolve(result);
		}, 1000);
	});
	return promise;
}

increase(0)
	.then((number) => {
		// Promise 에서 resolve 된 값은 .then 을 통해 받아 올 수 있음
		console.log(number);
		return increase(number); // Promise 를 리턴하면
	})
	.then((number) => {
		// 또 .then 으로 처리 가능
		console.log(number);
		return increase(number);
	})
	.catch((e) => {
		// 도중에 에러가 발생한다면 .catch 를 통해 확인 가능
		console.log(e);
	});
```

- 여러 작업을 연달아 처리한다고 해서 함수를 여러 번 감싸는 것이 아니라
  - `.then` 을 사용하여 그다음 작업을 설정하기 때문에 콜백 지옥 형성 X
    <br>

### 14.1.3 async/await

> 💡 async/await 는 Promise 를 더욱 쉽게 사용할 수 있도록 해 주는 ES8 문법

<BR>

```jsx
function increase(number) {
	const promise = new Promise((resolve, reject) => {
		// resolve 는 성공, reject 는 실패
		setTimeout(() => {
			const result = number + 10;
			if (result > 50) {
				// 50 보다 높으면 에러 발생시키기
				const e = new Error("NumberTooBig");
				return reject(e);
			}
			resolve(result);
		}, 1000);
	});
	return promise;
}

async function runTasks() {
	try {
		// try-catch 구문을 사용하여 에러를 처리
		let result = await increase(0);
		console.log(result);
		result = await increase(result);
		console.log(result);
		result = await increase(result);
		console.log(result);
	} catch (e) {
		console.log(e);
	}
}

runTasks();
```

- 함수 앞부분에 `async` 키워드를 추가하고, 해당 함수 내부에서 Promise 의 앞부분에 `await` 키워드를 사용
  - 이렇게 하면 Promise 가 끝날 때까지 기다리고, 결과 값을 특정 변수에 담을 수 있게 됨.
    <br>

## 14.2 axios 로 API 호출해서 데이터 받아 오기

> 💡 axios 는 현재 가장 많이 사용되고 있는 자바스크립트 HTTP 클라이언트로, Promise 기반으로 HTTP 요청을 처리함.

<br>

`App.js`

```jsx
import React, { useState } from "react";
import axios from "axios";

const App = () => {
	const [data, setData] = useState(null);
	const onClick = () => {
		axios.get("https://jsonplaceholder.typicode.com/todos/1").then((response) => {
			setData(response.data);
		});
	};
	return (
		<div>
			<div>
				<button onClick={onClick}>불러오기</button>
			</div>
			{data && <textarea rows={7} value={JSON.stringify(data, null, 2)} readOnly={true} />}
		</div>
	);
};

export default App;
```

- `axios.get` 함수를 통해 파라미터로 전달된 주소에 GET 요청을 보내고,
  - 이에 대한 결과는 .then 을 통해 비동기적으로 확인
    <br>

### 🔍 실행 화면

<br>

![image](https://user-images.githubusercontent.com/77706631/187816694-8c8a73e7-dd6e-4132-b631-21e97de12805.png)

<br>

`App.js`

```jsx
import React, { useState } from "react";
import axios from "axios";

const App = () => {
	const [data, setData] = useState(null);
	const onClick = async () => {
		try {
			const response = await axios.get("https://jsonplaceholder.typicode.com/todos/1");
			setData(response.data);
		} catch (e) {
			console.log(e);
		}
	};
	return (
		<div>
			<div>
				<button onClick={onClick}>불러오기</button>
			</div>
			{data && <textarea rows={7} value={JSON.stringify(data, null, 2)} readOnly={true} />}
		</div>
	);
};

export default App;
```

- 위 코드에서 async/await 를 적용한 코드

<br>

## 14.3 newsapi API 키 발급받기

- https://newsapi.org/register 에서 고유 API key 발급받기
- 전체 뉴스 불러오기
  - `GET` `https://newsapi.org/v2/top-headlines?country=kr&apiKey=03d25d7e263640b5bbc3af5b604a6e9e`
- 특정 카테고리 뉴스 불러오기
  - `GET` `https://newsapi.org/v2/top-headlines?country=kr&category=business&apiKey=03d25d7e263640b5bbc3af5b604a6e9e`

<br>

### 🔍 실행 화면

<br>

<img style="height:350px" src="https://user-images.githubusercontent.com/77706631/187817743-1a22c44e-d7c6-4968-9837-a73afa514264.png">

<br>

## 14.4 뉴스 뷰어 UI 만들기

- `NewsList`

  - api 를 요청하고 뉴스 데이터가 들어 있는 배열을 컴포넌트 배열로 변환하여 렌더링해 주는 컴포넌트
    <br>

- `NewsItem`
  - 각 뉴스 정보를 보여 주는 컴포넌트
    <br>

#### 14.4.1 NewsItem 만들기

<br>

`NewsItem.js`

```jsx
import React from "react";
import styled from "styled-components";

const NewsItemBlock = styled.div`
	display: flex;

	.thumbnail {
		margin-right: 1rem;
		img {
			display: block;
			width: 160px;
			height: 100px;
			object-fit: cover;
		}
	}
	.contents {
		h2 {
			margin: 0;
			a {
				color: black;
			}
		}
		p {
			margin: 0;
			line-height: 1.5;
			margin-top: 0.5rem;
			white-space: normal;
		}
	}
	& + & {
		margin-top: 3rem;
	}
`;

const NewsItem = ({ article }) => {
	const { title, description, url, urlToImage } = article;
	return (
		<NewsItemBlock>
			{urlToImage && (
				<div className='thumbnail'>
					<a href={url} target='_blank' rel='noopener noreferrer'>
						<img src={urlToImage} alt='thumbnail' />
					</a>
				</div>
			)}
			<div className='contents'>
				<h2>
					<a href={url} target='_blank' rel='noopener noreferrer'>
						{title}
					</a>
				</h2>
				<p>{description}</p>
			</div>
		</NewsItemBlock>
	);
};

export default NewsItem;
```

- NewsItem 컴포넌트는 article 이라는 객체를 props 로 통째로 받아 와서 사용
  <br>

#### 14.4.2 NewsList 만들기

<br>

`NewsList.js`

```jsx
import React from "react";
import styled from "styled-components";
import NewsItem from "./NewsItem";

const NewsListBlock = styled.div`
	box-sizing: border-box;
	padding-bottom: 3rem;
	width: 768px;
	margin: 0 auto;
	margin-top: 2rem;
	@media screen and (max-width: 768px) {
		width: 100%;
		padding-left: 1rem;
		padding-right: 1rem;
	}
`;

const sampleArticle = {
	title: "제목",
	description: "내용",
	url: "https://google.com",
	urlToImage: "https://via.placeholder.com/160",
};

const NewsList = () => {
	return (
		<NewsListBlock>
			<NewsItem article={sampleArticle} />
			<NewsItem article={sampleArticle} />
			<NewsItem article={sampleArticle} />
		</NewsListBlock>
	);
};

export default NewsList;
```

<br>

`App.js`

```jsx
import React, { useState } from "react";
import NewsList from "./components/NewsList";

const App = () => {
	return <NewsList />;
};

export default App;
```

<br>

### 🔍 실행 화면

<br>

<img style="height:350px" src="https://user-images.githubusercontent.com/77706631/187819744-22e5f985-8176-4c9c-a41e-63f1f8ef25b3.png">

<br><br>

## 14.5 데이터 연동하기

- useEffect 를 사용하여 컴포넌트가 처음 렌더링되는 시점에만 API 요청 가능
  - useEffect 에 등록하는 함수에 async 를 붙이면 X
    - 반환해야 하는 값이 뒷정리 함수이기 때문
  - async/await 를 사용하고 싶다면
    - useEffect 함수 내부에 async 키워드가 붙은 또 다른 함수를 만들어서 사용해야 함.

<br>

`NewsList.js`

```jsx
import React, { useEffect, useState } from "react";
import styled from "styled-components";
import NewsItem from "./NewsItem";
import axios from "axios";

const NewsListBlock = styled.div`
	box-sizing: border-box;
	padding-bottom: 3rem;
	width: 768px;
	margin: 0 auto;
	margin-top: 2rem;
	@media screen and (max-width: 768px) {
		width: 100%;
		padding-left: 1rem;
		padding-right: 1rem;
	}
`;

const NewsList = () => {
	const [articles, setArticles] = useState(null);
	const [loading, setLoading] = useState(false);

	useEffect(() => {
		// async 를 사용하는 함수는 useEffect 내부에서 따로 선언
		const fetchData = async () => {
			setLoading(true);
			try {
				const response = await axios.get(
					`https://newsapi.org/v2/top-headlines?country=kr&apiKey=03d25d7e263640b5bbc3af5b604a6e9e`
				);
				setArticles(response.data.articles);
			} catch (e) {
				console.log(e);
			}
			setLoading(false);
		};
		fetchData();
	}, []);

	// 대기 중일 때
	if (loading) {
		return null;
	}
	// 아직 articles 값이 설정되지 않았을 때
	if (!articles) {
		return <NewsListBlock>대기 중...</NewsListBlock>;
	}

	// articles 값이 유효할 때
	return (
		<NewsListBlock>
			{articles.map((article) => {
				<NewsItem key={article.url} article={article} />;
			})}
		</NewsListBlock>
	);
};

export default NewsList;
```

<br>

## 14.6 카테고리 기능 구현하기

- 뉴스 카테고리

  - business
  - science
  - entertainment
  - sports
  - health
  - technology
    <br>

<img style="height:60px" src="https://user-images.githubusercontent.com/77706631/187823362-fe508342-76a3-4ce0-8429-de4847b85248.png">

- 위 카테고리를 보여 줄 때는 한글로 보여 준 뒤,
  - 클릭했을 때는 영어로 된 카테고리 값을 사용하도록 구현

<br>

`Categories.js`

```jsx
import React from "react";
import styled, { css } from "styled-components";

const categories = [
	{
		name: "all",
		text: "전체보기",
	},
	{
		name: "business",
		text: "비즈니스",
	},
	{
		name: "entertainment",
		text: "엔터테인먼트",
	},
	{
		name: "health",
		text: "건강",
	},
	{
		name: "science",
		text: "과학",
	},
	{
		name: "sports",
		text: "스포츠",
	},
	{
		name: "technology",
		text: "기술",
	},
];

const CategoriesBlock = styled.div`
	display: flex;
	padding: 1rem;
	width: 768px;
	margin: 0 auto;
	@media screen and (max-width: 768px) {
		width: 100%;
		overflow-x: auto;
	}
`;

const Category = styled.div`
	font-size: 1.125rem;
	cursor: pointer;
	white-space: pre;
	text-decoration: none;
	color: inherit;
	padding-bottom: 0.25rem;

	&:hover {
		color: #495057;
	}

	${(props) =>
		props.active &&
		css`
			font-weight: 600;
			border-bottom: 2px solid #22b8cf;
			color: #22b8cf;
			&:hover {
				color: #3bc9db;
			}
		`}

	& + & {
		margin-left: 1rem;
	}
`;

const Categories = ({ onSelect, category }) => {
	return (
		<CategoriesBlock>
			{categories.map((c) => (
				<Category key={c.name} active={category === c.name} onClick={() => onSelect(c.name)}>
					{c.text}
				</Category>
			))}
		</CategoriesBlock>
	);
};

export default Categories;
```

- `name`
  - 실제 카테고리 값
    <br>
- `text`
  - 렌더링할 때 사용할 한글 카테고리
    <br>

`NewsList.js`

```jsx
import React, { useEffect, useState } from "react";
import styled from "styled-components";
import NewsItem from "./NewsItem";
import axios from "axios";

const NewsListBlock = styled.div`
	box-sizing: border-box;
	padding-bottom: 3rem;
	width: 768px;
	margin: 0 auto;
	margin-top: 2rem;
	@media screen and (max-width: 768px) {
		width: 100%;
		padding-left: 1rem;
		padding-right: 1rem;
	}
`;

const NewsList = ({ category }) => {
	const [articles, setArticles] = useState(null);
	const [loading, setLoading] = useState(false);

	useEffect(() => {
		// async 를 사용하는 함수는 useEffect 내부에서 따로 선언
		const fetchData = async () => {
			setLoading(true);
			try {
				const query = category === "all" ? "" : `&category=${category}`;
				const response = await axios.get(
					`https://newsapi.org/v2/top-headlines?country=kr${query}&apiKey=03d25d7e263640b5bbc3af5b604a6e9e`
				);
				setArticles(response.data.articles);
			} catch (e) {
				console.log(e);
			}
			setLoading(false);
		};
		fetchData();
	}, [category]);

	// 대기 중일 때
	if (loading) {
		return null;
	}
	// 아직 articles 값이 설정되지 않았을 때
	if (!articles) {
		return <NewsListBlock>대기 중...</NewsListBlock>;
	}

	// articles 값이 유효할 때
	return (
		<NewsListBlock>
			{articles.map((article) => {
				<NewsItem key={article.url} article={article} />;
			})}
		</NewsListBlock>
	);
};

export default NewsList;
```

- category 값이 무엇인지에 따라 요청할 주소가 동적으로 바뀌게 함.
  - category 값이 all 이라면 query 값을 공백으로,
  - all 이 아니라면 해당 카테고리를 주소에 포함시킴.
    <br>

`App.js`

```jsx
import React, { useCallback, useState } from "react";
import NewsList from "./components/NewsList";
import Categories from "./components/Categories";

const App = () => {
	const [category, setCategory] = useState("all");
	const onSelect = useCallback((category) => setCategory(category), []);

	return (
		<>
			<Categories category={category} onSelect={onSelect} />
			<NewsList category={category} />;
		</>
	);
};

export default App;
```

<br>

## 14.7 리액트 라우터 적용하기

#### 14.7.1 리액트 라우터의 설치 및 적용

```shell
npm i react-router-dom
```

- 리액트 라우터 설치
  <br>

`index.js`

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

- index.js 에서 리액트 라우터 적용

<br>

#### 14.7.2 NewsPage 생성

`NewsPage.js`

```jsx
import React from "react";
import Categories from "../components/Categories";
import NewsList from "../components/NewsList";

const NewsPage = ({ match }) => {
	// 카테고리가 선택되지 않았으면 기본값 all 로 사용
	const category = match.params.category || "all";

	return (
		<>
			<Categories />
			<NewsList Category={category} />
		</>
	);
};

export default NewsPage;
```

- 현재 선택된 category 값을 URL 파라미터를 통해 사용할 것이므로
  - Categories 컴포넌트에서 현재 선택된 카테고리 값을 알려 줄 필요 X
    <br>

`App.js`

```jsx
import React from "react";
import { Route } from "react-router-dom";
import NewsPage from "./pages/NewsPage";

const App = () => {
	return <Route path='/:category?' component={NewsPage} />;
};

export default App;
```

- App 의 기존 내용을 모두 지우고 Route 정의
  - `/:category?` 에서 물음표 문자는 category 값이 선택적이라는 의미
  - category URL 파라미터가 없다면 전체 카테고리를 선택한 것으로 간주
    <br>

#### 14..3 Categories 에서 NavLink 사용하기

`Categories.js`

```jsx
import React from "react";
import styled from "styled-components";
import { NavLink } from "react-router-dom";

const categories = [
	{
		name: "all",
		text: "전체보기",
	},
	{
		name: "business",
		text: "비즈니스",
	},
	{
		name: "entertainment",
		text: "엔터테인먼트",
	},
	{
		name: "health",
		text: "건강",
	},
	{
		name: "science",
		text: "과학",
	},
	{
		name: "sports",
		text: "스포츠",
	},
	{
		name: "technology",
		text: "기술",
	},
];

const CategoriesBlock = styled.div`
	display: flex;
	padding: 1rem;
	width: 768px;
	margin: 0 auto;
	@media screen and (max-width: 768px) {
		width: 100%;
		overflow-x: auto;
	}
`;

const Category = styled(NavLink)`
	font-size: 1.125rem;
	cursor: pointer;
	white-space: pre;
	text-decoration: none;
	color: inherit;
	padding-bottom: 0.25rem;

	&:hover {
		color: #495057;
	}

	&.active {
		font-weight: 600;
		border-bottom: 2px solid #22b8cf;
		color: #22b8cf;
		&:hover {
			color: #3bc9db;
		}
	}

	& + & {
		margin-left: 1rem;
	}
`;

const Categories = () => {
	return (
		<CategoriesBlock>
			{categories.map((c) => (
				<Category
					key={c.name}
					activeClassName='active'
					exact={c.name === "all"}
					to={c.name === "all" ? "/" : `/${c.name}`}
				>
					{c.text}
				</Category>
			))}
		</CategoriesBlock>
	);
};

export default Categories;
```

- NavLink 로 만들어진 Category 컴포넌트에 to 값은 `"/카테고리 이름"` 으로 설정
- to 값이 "/" 를 가리키고 있을 때는 exact 값을 true 로 설정
  - 다른 카테고리가 선택되었을 때 `전체보기` 링크에 active 스타일이 적용되지 않도록
