# 14. 외부 API를 연동하여 뉴스 뷰어 만들기

## 14.1 비동기 작업의 이해

웹 애플리케이션을 만들다 보면 처리할 때 시간이 걸리는 작업이 있음

**동기**

- 요청이 끝날 때까지 기다리는 동안 중지 상태가 되기 때문에 다른 작업을 할 수 없음
- 요청이 끝나야 비로소 그다음 예정된 작업을 할 수 있음

**비동기**

- 작업을 즉시 처리하는 것이 아니라, 작업이 완료될 때까지 기다렸다가 후의 작업이 완료되면 작업을 처리
- 웹 애플리케이션을 멈추지 않기 때문에 동시에 여러 가지 요청을 처리할 수도 있고, 기다리는 과정에서 다른 함수도 호출할 수 있음

![image](https://user-images.githubusercontent.com/55246584/184595889-a5639d3c-1e41-4c87-9dd8-df3855facfcd.png)

### 14.1.1 콜백 함수

> 특정 함수가 처리된 직후 어떠한 작업을 하고 싶다면 콜백 함수를 활용

```js
function increase(number, callback) {
	setTimeout(() => {
		const result = number + 10;
		if (callback) {
			callback(result);
		}
	}, 1000);
}

console.log('작업 시작');
increase(0, (result) => {
	console.log(result);
	increase(result, (result) => {
		console.log(result);
	});
});
```

### 14.1.2 Promise

```js
function increase(number) {
	const promise = new Promise((resolve, reject) => {
		// resolve는 성공, reject는 실패
		setTimeout(() => {
			const result = number + 10;
			if (result > 50) {
				// 50보다 높으면 에러 발생시키기
				const e = new Error('NumberTooBig');
				return reject(e);
			}
			resolve(result); // number 값에 +10 후 성공 처리
		}, 1000);
	});
	return promise;
}

increase(0)
	.then((number) => {
		// promise에서 resolve된 값은 .then을 통해 받아올 수 있음
		console.log(number);
		return increase(number); // Promise를 리턴
	})
	.then((number) => {
		// .then으로 처리
		console.log(number);
		return increase(number);
	});
```

### 14.1.3 async/await

```js
function increase(number) {
	const promise = new Promise((resolve, reject) => {
		// resolve는 성공, reject는 실패
		setTimeout(() => {
			const result = number + 10;
			if (result > 50) {
				// 50보다 높으면 에러 발생시키기
				const e = new Error('NumberTooBig');
				return reject(e);
			}
			resolve(result); // number 값에 +10 후 성공 처리
		}, 1000);
	});
	return promise;
}

async function runTasks() {
	try {
		let result = await increase(0);
		console.log(result);
		result = await increase(result);
		console.log(result);
	} catch (e) {
		console.log(e);
	}
}
```
