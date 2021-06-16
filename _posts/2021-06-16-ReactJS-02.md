# ReactJS-02



## 1. 리액트 코드 이해하기

- ```react
  // src/App.js
  
  import React from 'react';
  import logo from './logo.svg';
  import './App.css';
  
  function App() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p>
            Edit <code>src/App.js</code> and save to reload.
          </p>
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
        </header>
      </div>
    );
  }
  
  export default App;
  ```



1. ```react
   import React from 'react';
   ```

   - 리액트 프로젝트를 만들 때 **node_modules** 라는 디렉토리도 함께 생성, 이 안에 react 모듈이 있다.

   - 원래 브라우저에서는 모듈을 불러와서 사용할 수 없는 기능이다. 브라우저가 아닌 환경에서 자바스크립트를 실행할 수 있게 하는 환경은 <u>Node.js가 지원하는 기능</u>이다.

   - 이러한 기능을 브라우저에서도 사용하기 위해 **번들러(bundler)** 사용

     - 대표적으로 웹팩, Parcel, browserify
     - 리액트 프로젝트에서는 주로 웹팩 사용

   - ```react
     import logo from './logo.svg';
     import './App.css';
     ```

   - 웹팩을 사용하면 SVG 파일과 CSS 파일도 불러와서 사용 가능

   - 이렇게 파일들을 불러오는 것은 <u>웹팩의 로더(loader)</u>라는 기능이 담당

     - css-loader, file-loader, bable-loader 등의 여러 종류가 있다.

       

2. ```react
   function App() {
     return (
       <div className="App">
         <header className="App-header">
           <img src={logo} className="App-logo" alt="logo" />
           <p>
             Edit <code>src/App.js</code> and save to reload.
           </p>
           <a
             className="App-link"
             href="https://reactjs.org"
             target="_blank"
             rel="noopener noreferrer"
           >
             Learn React
           </a>
         </header>
       </div>
     );
   }
   
   export default App;
   ```

   - 위 코드는 **App** 이라는 <u>컴포넌트를 생성</u>한다.
   - `function` 키워드를 사용하여 컴포넌트를 만든다. → <u>함수형 컴포넌트</u>
   - 프로젝트에서 컴포넌트를 렌더링하면 함수에서 반환하고 있는 내용을 나타낸다.
     - 렌더링이란 '보여준다'는 것을 의미
   - 위와 같은 코드는 HTML이 아니라 **JSX**라고 부른다.



## 2. JSX

> 자바스크립트 확장 문법, 공식적인 자바스크립트 문법은 아니다.



- JSX 형식으로 작성된 코드는 브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 바벨을 사용하여 일반 자바스크립트 형태의 코드로 변환된다.
- 아래와 같이 JSX로 짜여진 코드가 자바스크립트로 변환된다.

- ```react
  // JSX
  function App() {
      return (
      	<div>
          	Hello <b>react</b>
          </div>
      );
  }
  ```

- ```javascript
  // javascript
  function App() {
      return React.createElement("div", null, "Hello", React.createElement("b", null, "react"));
  }
  ```



### 2-1. JSX 장점

- 보기 쉽고 익숙하다.(가독성이 높다)

- 활용도가 높다. HTML 태그 뿐만 아니라, <u>컴포넌트도 JSX 안에서 작성할 수 있다.</u>

  - ```react
    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import reportWebVitals from './reportWebVitals';
    
    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById('root')
    );
    ```

  - `ReactDOM.render`

    - 컴포넌트를 페이지에 렌더링 하는 역할
    - 첫번째 파라미터 : 페이지에 렌더링할 내용을 JSX형태로 작성
    - 두번째 파라미터 : 해당 JSX를 렌더링할 document 내부 요소를 설정

  - `React.StrictMode`

    - 리액트 프로젝트에서 리액트의 레거시 기능들을 사용하지 못하게 하는 기능
    - `ref`, `componentWillMount` 등 나중에 사라지게 될 옛날 기능을 사용할 시 경고를 출력



### 2-2. JSX 문법

1. 컴포넌트에 여러 요소가 있다면 반드시 부모 요소 하나로 감싸야 한다.

   - ```react
     // 이 코드는 요소 여러개가 부모 요소 하나에 감싸져 있지 않아 오류가 발생한다.
     import React from 'react';
     
     function App() {
       return (
     	<h1>리액트 안녕</h1>
         <h2>반갑다</h2>
       );
     }
     
     export default App;
     ```

   - ```react
     // 다음과 같이 작성해야 한다.
     import React from 'react';
     
     function App() {
       return (
           <div>
           	<h1>리액트 안녕</h1>
         	<h2>반갑다</h2>
           </div>
       );
     }
     
     export default App;
     ```

   - Virtual DOM에서 컴포넌트 변화를 감지해 낼 때 효율적으로 비교할 수 있도록 하기 위함(컴포넌트 내부는 **하나의 DOM 트리 구조**)

   - *div* 요소를 사용하고 싶지 않을 때는 `Fragment` 태그를 사용

     

2. JSX 안에서는 자바스크립트 사용 가능

   - ```react
     function App() {
       const name = '리액트';
       return (
         <>
     	  <h1>{name} 안녕</h1>
           <h2>반갑다</h2>
       	</>
       );
     }
     ```

   - `const` 는 변경이 불가능한 상수를 선언

   - `let` 은 동적인 값을 담을 수 있는 변수를 선언

   - ES6 이전에는 `var` 를 사용했다.  그러나 `var` 키워드는 scope가 함수 단위이다.

     

3. If 문 대신 조건부 연산자(삼항 연산자)

   - ```react
     function App() {
       const name = '리액트';
       return (
         <div>
     	  {name === '리액트' ? (
               <h1>리액트입니다.</h1>
            ) : (
               <h1>리액트 아닙니다.</h1>
            )}
       	</div>
       );
     }
     ```

     

4. AND 연산자(&&) 를 사용한 조건부 렌더링

   - ```react
     function App() {
       const name = '저라뎃';
       return <div>{name === '리액트' && <h1>리액트입니다.</h1>}</div>;
     }
     ```

   - 위 코드는 null 을 렌더링한 결과가 나온다.

   - 리액트에서 false를 렌더링할 때는 null과 마찬가지로 아무것도 나타나지 않는다.

     

5. undefined를 렌더링하지 않기

   - ```react
     function App() {
       const name = undefined;
       return name;
     }
     ```

   - 위와 같은 코드는 오류를 발생한다.

   - 이를 방지 하기 위해 OR(||) 연산자를 사용할 수 있다.

   - ```react
     return name || '값이 undefined입니다.';
     ```

     

6. 인라인 스타일링

   - 리액트에서 DOM 요소에 스타일을 적용할 때는 문자열 형태로 넣는 것이 아니라 객체 형태로 넣어준다.

   - 스타일 이름 중에 background-color 처럼 - 문자가 포함되는 이름은 카멜 표기법으로 작성한다.

     - background-color → backgroundColor

       

7. class 대신 className

   - ```react
     <div class="myclass"></div> // X
     <div className="myclass"></div> // O
     ```

     

8. 꼭 닫아야 하는 태그

   - ```react
     <br> // X
     <input> // X
         
     <br></br> // O
     <input></input>	// O
     <input /> // O

