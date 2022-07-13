# 7 컴포넌트의 라이프사이클 메서드

## 7.1 라이프사이클 메서드의 이해

#### 라이프사이클 메서드의 종류는 총 아홉 가지

- `Will` 접두사가 붙은 메서드: 어떤 작업을 작동하기 전에 실행되는 메서드
- `Did` 접두사가 붙은 메서드: 어떤 작업을 작동한 후에 실행되는 메서드

#### 라이프사이클은 총 세가지

- 마운트, 업데이트, 언마운트로 나뉨

<img width="418" alt="image" src="https://user-images.githubusercontent.com/55246584/178136420-c893bff7-4510-4bd0-8215-48601496ed1d.png">

#### 마운트

- DOM이 생성되고 웹 브라우저 상에 나타나는 것을 마운트(mount)라고 함

<img width="320" alt="image" src="https://user-images.githubusercontent.com/55246584/178136445-131fdbd9-2fbe-4e7f-b510-4c17c0e1e047.png">

- **constructor**
  - 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드
- **getDerivedStateFromProps**
  - props에 있는 값을 state에 넣을 때 사용되는 메서드
- **render**
  - 우리가 준비한 UI를 렌더링하는 메서드
- **componentDidMount**
  - 컴포넌트가 웹 브라우저 상에 나타난 후 호출하는 메서드

#### 업데이트

**컴포넌트는 다음과 같은 총 네 가지 경우에 업데이트를 함**

- props가 바뀔 때
- state가 바뀔 때
- 부모 컴포넌트가 리렌더링될 때
- this.forceUpdate로 강제로 렌더링을 트리거할 때

<img width="456" alt="image" src="https://user-images.githubusercontent.com/55246584/178136579-4ba4c105-7798-46d0-b2bc-5dadd19b8086.png">

- **getDerivedStateFromProps**
  - 마운트 과정에서도 호출되며, 업데이트가 시작하기 전에도 호출됨
  - props의 변화에 따라 state 값에도 변화를 주고 싶을 때 사용
- **shouldComponentUpdate**
  - 컴포넌트가 리렌더링 해야 할지 말아야 할지를 결정하는 메서드
  - true 혹은 false를 반환해야 함
    - true를 반환하면 다음 라이프사이클 메서드를 계속 실행
    - false를 반환하면 작업을 중지, 컴포넌트가 리렌더링되지 않음
    - `this.forceUpdate()`를 호출한다면 이 과정을 생략하고 바로 render 함수를 호출
- **render**
  - 컴포넌트를 리렌더링함
- **getSnapShotBeforeUpdate**
  - 컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드
- **componentDidUpdate**
  - 컴포넌트의 업데이트 작업이 끝난 후 호출하는 메서드

#### 언마운트

- 마운트의 반대 과정, 즉 컴포넌트를 DOM에서 제거하는 것을 언마운트(unmount)라고 함

<img width="455" alt="image" src="https://user-images.githubusercontent.com/55246584/178136655-a51891f6-e9c0-43cd-81f6-c39c19cb9cfe.png">

- **componentWillUnmount**
  - 컴포넌트가 웹 브라우저 상에서 사라지기 전에 호출하는 메서드

## 7.2 라이프사이클 메서드 살펴보기

### 7.2.1 render() 함수

> 💡 컴포넌트 모양새를 정의

```jsx
render() { ... }
```

- this.props와 this.state에 접근할 수 있음
- 리액트 요소를 반환

### 7.2.2 constructor 메서드

> 💡 컴포넌트의 생성자 메서드로 컴포넌트를 만들 때 처음 실행

```jsx
constructor(props){ ... }
```

- 초기 state를 정할 수 있음

### 7.2.3 getDerivedStateFromProps 메서드

```jsx
static getDerivedStateFromProps(nextProps, prevState) {
  if(nextProps.value !== prevState.value){ // 조건에 따라 특정 값 동기화
    return { value: nextProps.value };
  }
  return null;
}
```

- 리액트 v16.3 이후에 새로 만든 라이프사이클 메서드
- props로 받아 온 값을 state에 동기화시키는 용도로 사용
- 컴포넌트가 마운트될 때와 업데이트될 때 호출됨

### 7.2.4 componentDidMount 메서드

> 💡 컴포넌트를 만들고, 첫 렌더링을 다 마친 후 실행

```jsx
componentDidMount() { ... }
```

- 다른 자바스크립트 라이브러리 또는 프레임워크 함수를 호춣하거나 이벤트 등록, setTimeout, setInterval, 네트워크 요청 같은 비동기 작업을 처리하면 됨

### 7.2.5 shouldComponentUpdate 메서드

> 💡 props 또는 state를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 메서드

```jsx
shouldComponentUpdate(nextProps,nextState) { ... }
```

- 반드시 true 또는 false 값을 반환
  - 이 메서드를 따로 생성하지 않는다면 기본적으로 true를 반환
  - 해당 메서드가 false 값을 반환한다면 업데이트 과정을 여기서 중지
- 현재 props와 state는 `this.props`와 `this.state`로 접근 가능
- 새로 설정될 props 또는 state는 `nextProps`와 `nextState`로 접근 가능
- 프로젝트 성능을 최적화할 때, 상황에 맞는 알고리즘을 작성하여 리렌더링을 방지할 때는 false 값을 반환하게 함

### 7.2.6 getSnapshotBeforeUpdate 메서드

> 💡 render에서 만들어진 결과물이 브라우저에 실제로 반영되기 직전에 호출됨

```jsx
getSnapshotBeforeUpdate(prevProps, prevState) {
  if(prevState.array !== this.state.array){
    const { scrollTop, scrollHeight } = this.list;
    return { scrollTop, scrollHeight };
  }
}
```

- 리액트 v16.3 이후 생긴 메서드
- 해당 메서드에서 반환하는 값은 componentDidUpdate에서 세 번째 파라미터인 snapshot 값으로 전달받을 수 있음
- 주로, 업데이트하기 직전의 값을 참조할 일이 있을 때 활용

### 7.2.7 componentDidUpdate 메서드

> 💡 리렌더링을 완료할 후에 실행

```jsx
componentDidUpdate(prevProps, prevState, snapshot) { ... }
```

- 업데이트가 끝난 직후이므로, DOM 관련 처리를 해도 무방
- `prevProps` 또는 `prevState`를 사용하여 컴포넌트가 이전에 가졌던 데이터에 접근할 수 있음
- 또한, `getSnapshotBeforeUpdate`에서 반환한 값이 있다면 여기서 snapshot 값을 전달받을 수 있음

### 7.2.8 componentWillUnmount 메서드

> 💡 컴포넌트를 DOM에서 제거할 때 실행

- componentDidMount에서 등록한 이벤트, 타이머, 직접 생성한 DOM이 있다면 여기서 제거 작업을 해야 함

### 7.2.9 componentDidCatch 메서드

> 💡 컴포넌트 렌더링 도중에 에러가 발생했을 때 애플리케이션이 먹통이 되지 않고 오류 UI를 보여줄 수 있게 해 줌

```jsx
componentDidCatch(error,info) {
  this.setState({
    error: true
  });
  console.log({ error, info });
}
```

- 컴포넌트 자신에게 발생하는 에러를 잡아낼 수 없고 자신의 `this.props.children`으로 전달되는 컴포넌트에서 발생하는 에러만 잡아낼 수 있음
