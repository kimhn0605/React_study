# 1. 리액트 시작

## 1.1 왜 리액트인가?

- **리액트**

  - 자바스크립트 라이브러리로 사용자 인터페이스를 만드는 데 사용
  - 구조가 MVC, MVW 등인 프레임워크와 달리 오직 V (View) 만 신경 쓰는 라이브러리  
    <br>

- **컴포넌트**

  - 리액트 프로젝트에서 특정 부분이 어떻게 생길지 정하는 선언체
  - 재사용이 가능한 API 로서 수많은 기능들을 내장하고 있으며, 해당 컴포넌트의 생김새와 작동 방식을 정의
    <br>

- **렌더링**

  - 사용자 화면에 뷰를 보여 주는 것
    <br>

  **1) 초기 렌더링**

  - 뷰의 형태와 작동 방식에 대한 정보를 지닌 객체를 반환, 즉 컴포넌트가 어떻게 생겼는지를 정의
  - 어떤 프레임워크, 라이브러리를 사용하든지 간에 맨 처음 어떻게 보일지를 정하는 **초기 렌더링은 필수**
  - 리액트에서는 _render() {...} 함수를 사용하여 내부에 있는 컴포넌트들까지도 재귀적으로 렌더링_

    <img src="https://blog.kakaocdn.net/dn/Yuq1M/btq2gJJC6sw/KVkee5wdj7rg6Jkt0GRwa0/img.png">

  - 최상위 컴포넌트의 렌더링 작업이 끝나면 지니고 있는 정보들을 HTML 마크업을 만들고, 이를 실제 페이지의 DOM 요소 안에 주입
    <br>

  **2) 리렌더링**

  - 리액트에서 뷰를 업데이트할 때는 '조화 과정 (reconciliation) 을 거친다' 라고 표현
  - 2가지 뷰를 최소한의 연산으로 비교한 후, 둘의 차이를 알아내어 최소한의 연산으로 DOM 트리를 업데이트
    <br>

    <img style="height:300px" src="https://velog.velcdn.com/images%2Fwhite-jang%2Fpost%2F7d1dccc6-f59c-4778-844e-377e3d57366f%2Fimage.png">  
    <br><br>

  - render 함수가 반환하는 결과를 곧바로 DOM 에 반영하지 않고, **이전에 render 함수가 만들었던 컴포넌트 정보와 현재 render 함수가 만든 컴포넌트 정보를 비교**
    <br>

## 1.2 리액트의 특징

- **DOM (Document Object Model)**
  <img style="height:300px" src="https://images.velog.io/images/mollog/post/3ae1fc47-d869-451c-90b6-2530ad4c4549/image.png">
  <br>

  - 객체로 문서 구조를 표현하는 방법으로 XML 이나 HTML 로 작성
  - 웹 브라우저는 DOM 을 활용하여 객체에 자바스크립트와 CSS 적용
  - 트리 형태인 DOM 은 특정 노드를 찾거나 수정/제거/삽입 가능
    <br>

- **DOM 의 단점**

  - 동적 UI 에 최적화 X
  - 웹 브라우저 단에서 DOM 에 변화가 일어나면 웹 브라우저가 CSS 를 다시 연산하고, 레이아웃을 구성하고, 페이지를 리페인트 => 시간 허비
    <br>

- **해결법**

  - DOM 을 최소한으로 조작하여 작업을 처리
  - `Virtual DOM 방식을 사용하여 DOM 업데이트를 추상화함으로써 DOM 처리 횟수를 최소화하고 효율적으로 진행`
    <br>

- **Virtual DOM**

  - 실제 DOM 에 접근하여 조작하는 대신, 이를 추상화한 자바스크립트 객체를 구성하여 사용
  - 리액트에서 데이터 변화로 인해 실제 DOM 을 업데이트할 때 다음 3가지 절차를 밟음.

    1. 데이터를 업데이트하면 전체 UI 를 Virtual DOM 에 리렌더링
    2. 이전 Virtual DOM 에 있던 내용과 현재 내용을 비교
    3. 바뀐 부분만 실제 DOM 에 적용
       <br>
       <img style="height:300px" src="https://velog.velcdn.com/images%2Fwhite-jang%2Fpost%2F7d1dccc6-f59c-4778-844e-377e3d57366f%2Fimage.png">  
       <br>

  - 위 사진에서 오른족의 ‘새로운 DOM 트리’ 가 바로 Virtual DOM 을 의미
    <br>

- **오해**

  - Virtual DOM 을 사용한다고 해서 무조건 빠른 것 X
  - 리액트와 Virtual DOM 이 언제나 제공할 수 있는 것은 바로 **`업데이트 처리 간결성`**
  - UI 를 업데이트하는 과정에서 생기는 복잡함을 모두 해소하고, 더욱 쉽게 업데이트에 접근할 수 있도록 함.
    <br>

- **리액트의 기타 특징**
  - 오직 뷰만 담당하는 라이브러리
  - 프레임워크가 아니기 때문에 기타 기능은 직접 구현해서 사용해야 함.
  - 다른 웹 프레임워크나 라이브러리와 혼용 가능
    <br>

\*\* 이미지 출처

- https://juicyjerry.tistory.com/150
- https://velog.io/@white-jang/React-%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%9D%98-%EC%9D%B4%ED%95%B4%EC%99%80-%ED%8A%B9%EC%A7%95
