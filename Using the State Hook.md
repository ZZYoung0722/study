# Using the State Hook

```javascript
import React, { useState } from 'react';

function Example() {
  // 새로운 state 변수를 선언하고, count라 부르겠습니다.
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
예시를 비교하여 Hook 특징에 대해 배우기

---

### Hook과 같은 기능을 하는 클래스 예시

```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

위 코드에서 state는 `{ count: 0 }`이며 사용자가 `this.setState()`를 호출하는 버튼을 클릭했을 때 `state.count`를 증가시킵니다. 
> 주의
> 
> counter 예시를 사용한 이유는, Hook을 잘 이해할 수 있도록 도와주는 가장 기초적인 내용이 될 수 있기 때문입니다.

---

### Hook과 함수 컴포넌트

React함수 컴포넌트 생긴 모양
```javascript
const Example = (props) => {
  // 여기서 Hook을 사용할 수 있습니다!
  return <div />;
}
```
```javascript
function Example(props) {
  // 여기서 Hook을 사용할 수 있습니다!
  return <div />;
}
```

함수 컴포넌트를 "state가 없는 컴포넌트"로 알고 있었을겁니다. 하지만 Hook은 React state를 함수 안에서 사용할 수 있게 해줍니다.

**Hook은 클래스 안에서 동작하지 않습니다.** 하지만 클래스를 작성하지 않고 사용할 수 있습니다.

---

### Hook이란?

```javascript
import React, { useState } from 'react';

function Example() {
  // ...
}
```

**Hook이란?** Hook은 특별한 함수입니다.
예를 들어 `useState`는 state를 함수 컴포넌트 안에서 사용할 수 있게 해줍니다. 

**언제 Hook을 사용할까?** 함수 컴포넌트를 사용하던 중 state를 추가하고 싶을 때 클래스 컴포넌트로 바꾸곤 했을겁니다. 
하지만 이제 함수 컴포넌트 안에서 Hook을 이용하여 state를 사용할 수 있습니다.

>주의
>
>컴포넌트 안에서 Hook을 사용할 때 몇 가지 특별한 규칙이 있습니다. 
>
>[Hook의 규칙](https://ko.reactjs.org/docs/hooks-rules.html)

---

### state 변수 선언하기

클래스를 사용할 때, constructor 안에서 `this.state`를 `{ count: 0 }`로 설정함으로써 `count`를 `0`을 초기화했습니다.

```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
```

함수 컴포넌트는 `this`를 가질 수 없기 때문에 `this.state`를 할당하거나 읽을 수 없습니다. 대신, `useState` Hook을 직접 컴포넌트에 호출합니다.

```javascript
import React, { useState } from 'react';

function Example() {
  // 새로운 state 변수를 선언하고, 이것을 count라 부르겠습니다.
  const [count, setCount] = useState(0);
```

**useState를 호출하는 것은 무엇을 하는 걸까요?** 
“state 변수”를 선언할 수 있습니다. 
위에 선언한 변수는 `count`라고 부르지만 `banana`처럼 아무 이름으로 지어도 됩니다. 
`useState`는 클래스 컴포넌트의 `this.state`가 제공하는 기능과 똑같습니다. 일반적으로 일반 변수는 함수가 끝날 때 사라지지만, state 변수는 React에 의해 사라지지 않습니다.


**useState의 인자로 무엇을 넘겨주어야 할까요?** 
`useState()`Hook의 인자로 넘겨주는 값은 state의 초기 값입니다. 
함수 컴포넌트의 state는 클래스와 달리 객체일 필요는 없고, 숫자 타입과 문자 타입을 가질 수 있습니다. 
위의 예시는 사용자가 버튼을 얼마나 많이 클릭했는지 알기를 원하므로 `0`을 해당 state의 초기 값으로 선언했습니다. 
(2개의 다른 변수를 저장하기를 원한다면 `useState()`를 두 번 호출해야 합니다.)


**useState는 무엇을 반환할까요?**
state 변수, 해당 변수를 갱신할 수 있는 함수 이 두 가지 쌍을 반환합니다. 
이것이 바로 `const [count, setCount] = useState()`라고 쓰는 이유입니다. 
클래스 컴포넌트의 `this.state.count`와 `this.setState`와 유사합니다.

```javascript
import React, { useState } from 'react';

function Example() {
  // 새로운 state 변수를 선언하고, 이것을 count라 부르겠습니다.
  const [count, setCount] = useState(0);
```

`count라고 부르는 state 변수를 선언하고 `0`으로 초기화합니다. 
React는 해당 변수를 리렌더링할 때 기억하고, 가장 최근에 갱신된 값을 제공합니다. 
`count` 변수의 값을 갱신하려면 `setCount`를 호출하면 됩니다.

>주의
>
>왜 `createState`가 아닌, `useState`로 이름을 지었을까요?
> 
>컴포넌트가 렌더링할 때 오직 한 번만 생성되기 때문에 "Create"라는 이름은 꽤 정확하지 않을수 있습니다.
>
>컴포넌트가 다음 렌더링을 하는 동안 `useState`는 현재 state를 줍니다. Hook 이름이 항상 `use`로 시작하는 이유도 있습니다.

---





