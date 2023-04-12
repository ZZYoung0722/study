# react css, styled-components

- css 
  - 가장 흔한 방법
  - create-react-app 으로 만든 프로젝트를 보면 src 디렉토리에 App.js와 App.css가 있다. 
  ```javascript
  // App.js
  import React, { Component } from 'react';
  import logo from './logo.svg';
  import './App.css';

  class App extends Component {
    render() {
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
  }

  export default App;
  ```
  
  ```css
  // App.css

  .App {
    text-align: center;
  }

  .App-logo {
    animation: App-logo-spin infinite 20s linear;
    height: 40vmin;
  }

  .App-header {
    background-color: #282c34;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: calc(10px + 2vmin);
    color: white;
  }

  .App-link {
    color: #61dafb;
  }

  @keyframes App-logo-spin {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }
  ```
  
  CSS 작성할 때 가장 중요한 점은 **중복되는 클래스명 생성하지 않기** 이다.

  위에 css를 변환하면 ->
  
  ```css
  .App {
    text-align: center;
  }

  /*.App 안에 들어있는 .logo*/
  .App .logo {
    animation: App-logo-spin infinite 20s linear;
    height: 40vmin;
  }

  /* .App 안에 들어있는 header */
  .App header {
    background-color: #282c34;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: calc(10px + 2vmin);
    color: white;
  }

  /* .App 안에 들어있는 a 태그 */
  .App a {
    color: #61dafb;
  }

  @keyframes App-logo-spin {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }
  ```
  
  App.js 에서 사용된 클래스명들을 밑에 코드처럼 수정가능
  
  ```javascript
  // App.js

  import React, { Component } from "react";
  import logo from "./logo.svg";
  import "./App.css";

  class App extends Component {
    render() {
      return (
        <div className="App">
          <header>
            <img src={logo} className="logo" alt="logo" />
            <p>
              Edit <code>src/App.js</code> and save to reload.
            </p>
            <a
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
  }

  export default App;
  ```
  
  이런식으로, 컴포넌트의 최상위 html 요소에는 컴포넌트 이름과 동일한 이름으로 클래스명을 만들고, 
  그 하단에서는 소문자로 입력하거나, 딱히 클래스명이 불필요한 경우엔 아예 생략 가능하다.
  

- styled-components
  - styled-components는 현존하는 리액트 CSs-in-JS 관련 라이브러리 중에서 가장 잘나가는 라이브러리 입니다. 
  - CSS-in-JS 는 이름이 그렇듯, 자바스크립트 파일 안에 CSS를 작성하는 형태 

   > 📌 styled-components 설치
   >
   > npm install styled-components
   >
   > 설치 후 적용할 프로젝트 파일에 import 해주기
   >
   > import styled from ‘styled-components’;

   - 이 라이브러리를 이용하면 그냥 하나의 자바스크립트 파일안에 스타일까지 작성 할 수 있기 때문에 
   - .css/.scss 파일 같은걸 만들 고민은 안하셔도 된다는게 큰 이점이다.

   ```javascript
    // src/StyledComponent.js
    import React from 'react';
    import styled, { css } from 'styled-components';

    const Box = styled.div`
      /* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
      background: ${props => props.color || 'blue'};
      padding: 1rem;
      display: flex;
    `;

    const Button = styled.button`
      background: white;
      color: black;
      border-radius: 4px;
      padding: 0.5rem;
      display: flex;
      align-items: center;
      justify-content: center;
      box-sizing: border-box;
      font-size: 1rem;
      font-weight: 600;

      /* & 문자를 사용하여 Sass 처럼 자기 자신 선택 가능 */
      &:hover {
        background: rgba(255, 255, 255, 0.9);
      }

      /* 다음 코드는 inverted 값이 true 일 때 특정 스타일을 부여해줍니다. */
      ${props =>
        props.inverted &&
        css`
          background: none;
          border: 2px solid white;
          color: white;
          &:hover {
            background: white;
            color: black;
          }
        `};
      & + button {
        margin-left: 1rem;
      }
    `;

    const StyledComponent = () => (
      <Box color="black">
        <Button>안녕하세요</Button>
        <Button inverted={true}>테두리만</Button>
      </Box>
    );

    export default StyledComponent;
   ```
   
  >✅ 적용 방법
  >`styled.태그명` 과 같이 작성한 뒤, 적용하고자 하는 CSS 스타일을 작성
  >`…` → `` 안에 스타일 넣어주기
  >
  >Ex)
  >
  >```jsx
  >const Wrapper = styled.div`
  >display: flex;
  >justify-content: center;
  >align-items: center;
  >width: 100%;
  >height: 15vh;
  >`;
  >```

  - 스타일링 된 엘리먼트 만들기
    - 스타일링 된 엘리먼트를 만들 땐, 상단에서 styled를 불러오고 styled.태그명 사용하여 구현

    ```javascript
    import styled from 'styled-components';

    const MyComponent = styled.div`
      font-size: 2rem;
    `;
    ```
    - 만약 보여줘야 할 태그 형식이 유동적이거나, 아니면 특정 컴포넌트에 스타일링을 해야 하는 상황이라면 아래 코드 형태로 구현 가능하다.
    
    ```javascript
    // 문자열로 styled 의 인자로 전달
    const MyInput = styled('input')`
      background: gray;
    `
    // 아예 컴포넌트 형식의 값을 넣어줌
    const StyledLink = styled(Link)`
      color: blue;
    `
    ```
    
   - 스타일에서 props 조회하기
     - 스타일링 한 컴포넌트에 전달하는 props 값을 스타일쪽에서 그대로 사용 가능

     ```javascript
      const Box = styled.div`
      /* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
      background: ${props => props.color || 'blue'};
      padding: 1rem;
      display: flex;
       `;
     ```
     
   - props에 따른 조건부 스타일링
     - 일반 css클래스를 사용했더라면 주로 클래스이름으로 조건부 스타일링을 해왔다.
     - styled-components 에서는 그냥 props로도 처리 가능

     ```javascript
      import styled, { css } from 'styled-components';
      /* 단순 변수의 형태가 아니라 여러줄의 스타일 구문을 조건부로 설정해야 하는 경우엔
      css 를 불러와야합니다. 
      */
      const Button = styled.button`
        background: white;
        color: black;
        border-radius: 4px;
        padding: 0.5rem;
        display: flex;
        align-items: center;
        justify-content: center;
        box-sizing: border-box;
        font-size: 1rem;
        font-weight: 600;

        /* & 문자를 사용하여 Sass 처럼 자기 자신 선택 가능 */
        &:hover {
          background: rgba(255, 255, 255, 0.9);
        }

        /* 다음 코드는 inverted 값이 true 일 때 특정 스타일을 부여해줍니다. */
        ${props =>
          props.inverted &&
          css`
            background: none;
            border: 2px solid white;
            color: white;
            &:hover {
              background: white;
              color: black;
            }
          `};
        & + button {
          margin-left: 1rem;
        }
      `;
             `;
     ```

   - 반응형 디자인
     - src/StyledComponent.js 의 Box 컴포넌트

     ```javascript
      const Box = styled.div`
      /* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
      background: ${props => props.color || 'blue'};
      padding: 1rem;
      display: flex;
      /* 기본적으로는 1024px 에 가운데 정렬을 하고
        가로 크기가 작아짐에 따라 사이즈를 줄이고
        768px 미만으로 되면 꽉 채웁니다 */
      width: 1024px;
      margin: 0 auto;
      @media (max-width: 1024px) {
        width: 768px;
      }
      @media (max-width: 768px) {
        width: 100%;
      }
      `;
     ```
     
   - 함수화 하여 더 쉽게 할 수 있다.
     - src/StyledComponent.js 상단부

      ```javascript
      import React from 'react';
      import styled, { css } from 'styled-components';

      const sizes = {
        desktop: 1024,
        tablet: 768
      };

      // 위에있는 size 객체에 따라 자동으로 media 쿼리 함수를 만들어줍니다.
      // 참고: https://www.styled-components.com/docs/advanced#media-templates
      const media = Object.keys(sizes).reduce((acc, label) => {
        acc[label] = (...args) => css`
          @media (max-width: ${sizes[label] / 16}em) {
            ${css(...args)};
          }
        `;

        return acc;
      }, {});

      const Box = styled.div`
        /* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
        background: ${props => props.color || 'blue'};
        padding: 1rem;
        display: flex;
        width: 1024px;
        margin: 0 auto;
        ${media.desktop`width: 768px;`}
        ${media.tablet`width: 768px;`};
      `;
     ```



  
