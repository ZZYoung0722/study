# Hook의 개요
### 📌 State Hook
```javascript
import React, { useState } from 'react';

function Example() {
  // "count"라는 새 상태 변수를 선언합니다
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
여기서 `useState`가 바로 Hook 입니다.
Hook을 호출해 함수 컴포넌트(function component) 안에 state를 추가했습니다.
이 state는 컴포넌트가 다시 렌더링 되어도 그대로 유지될 것입니다.
`useState`는 ***현재의*** state 값과 이 값을 업데이트하는 함수를 쌍으로 제공합니다.
우리는 이 함수를 이벤트 핸들러나 다른 곳에서 호출할 수 있습니다. 
이것은 class의 `this,setState`와 거의 유사하지만, 이전 state와 새로운 state를 합치지 않는다는 차이점이 있습니다.

`useState`는 인자로 초기 state 갑승ㄹ 하나 받습니다. 카운터는 0부터 시작하기 때문에 위 예시에서는 초기값으로 `0`을 넣어준 것입니다. 
`this.state`와는 달리 `useState` Hook의 state는 객체일 필요가 없습니다. 
이 초기값은 첫 번째 렌더링에만 딱 한번 사용됩니다.

#### 여러 state 변수 선언하기
하나의 컴포넌트 내에서 State Hook을 여러 개 사용할 수도 있습니다.
```javascript
function ExampleWithManyStates() {
  // 상태 변수를 여러 개 선언했습니다!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```
배열 구조 분해(destructuring) 문법은 `useState`로 호출된 state 변수들을 다른 변수명으로 할당할 수 있게 해줍니다.
이 변수명은 `useState` API와 관련이 없습니다.
대신에 React는 매번 렌더링할 때 `useState`가 사용된 순서대로 실행할 것입니다.

#### Hook이 뭔가요?
Hook은 함수 컴포넌트에서 React state와 생명주기 기능을 "연동" 할 수 있게 해주는 함수입니다.(하지만 이미 짜놓은 컴포넌트를 모조리 재작성하는 것은 권장하지 않습니다. 대신 새로 작성하는 컴포넌트부터는 Hook을 이용)

---

### ⚡️Effect Hook
React 컴포넌트 안에서 데이터를 가져오거나 구독하고, DOM을 직접 조작하는 작업을 한 동작을 "side effects"라고 합니다.
왜냐하면 이것은 다른 컴포넌트에 영향을 줄 수도 있고, 렌더링 과정에서는 구현할 수 없는 작업이기 때문입니다.

Effect Hook 즉 `useEffect`는 함수 컴포넌트 내에서 이런 side effects를 수행할 수 있게 해줍니다. 
React class의 `componentDidMount` 나 `componentDidUpdate`, `componentWillUnmount`와 같은 목적으로 제공되지만, 하나의 API로 통합된 것입니다.

예를 들어, 이 예시는 React가 DOM을 업데이트한 뒤에 문서의 타이틀을 바꾸는 컴포넌트입니다.
```javascript
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // componentDidMount, componentDidUpdate와 비슷합니다
  useEffect(() => {
    // 브라우저 API를 이용해 문서의 타이틀을 업데이트합니다
    document.title = `You clicked ${count} times`;
  });

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
`useEffect`를 사용하면, React는 DOM을 바꾼 뒤에 "effect" 함수를 실행할 것입니다.
Effects는 컴포넌트 안에 선언되어있기 때문에 props와 state에 접근할 수 있습니다.
기본적으로 React는 매 렌더링 이후에 effects를 실행합니다.
(첫 번째 렌더링도 포함해서 실행)

Effect를 "해제"할 필요가 있다면, 해제하는 함수를 반환해주면 됩니다.
이는 선택적입니다. 예를들어, 이 컴포넌트는 친구의 접속 상태를 구독하는 effect를 사용했고, 구독을 해지함으로써 해지해줍니다.

```javascript
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```
이 예시에서 컴포넌트가 unmount될 떄 React는 `ChatAPI`에서 구독을 해지할 것입니다. 또한 재렌더링이 일어나 effect를 재실해하기 전에도 마찬가지로 구독을 해지합니다.

`useState`와 마찬가지로 컴포넌트 내에서 여러 개의 effect를 사용할 수 있습니다.

```javascript
function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
  // ...
```
Hook을 사용하면 구독을 추가하고 제거하는 로직과 같이 서로 관련 있는 코드들을 한군데에 모아서 작성할 수 있습니다. 
반면 class 컴포넌트에서는 생명주기 메서드 각각에 쪼개서 넣어야만 했습니다.

---

### ✌️ Hook 사용 규칙
Hook은 그냥 Javascript 함수이지만, 두 가지 규칙을 준수해야한다.
- 최상위에서만 Hook을 호출해야 합니다. 반목문, 조건문, 중접된 함수 내에서 Hook을 실행하지 마세요.
- React 함수 컴포넌트 내에서만 Hook을 호출해야 합니다. 일반 Javascript 함수에서는 Hook을 호출해서는 안됩니다.(직접 작성한 custom Hook 내에서 Hook을 호출할 수 있다.)

---

### 💡 나만의 Hook 만들기
`higher-order components`와 `render props`가 바로 그것입니다. Custom Hook은 이들 둘과는 달리 컴포넌트 트리에 새 컴포넌트를 추가하지 않고도 이것을 가능하게 해줍니다.

친구의 접속 상태를 구독하기 위해서 `useState`와 `useEffect` Hook을 사용한 `FriendStatus` 컴포넌트 예시를 다시 한번 보겠습니다. 이 로직을 다른 컴포넌트에서도 재사용하고 싶다고 가정을 해봅시다.

먼저, 이 로직을 `useFriendStatus`라는 custom Hook으로 뽑아냅니다.
```javascript
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```
이 Hook은 `friendID`를 인자로 받아서 친구의 접속 상태를 반환해줍니다.

이것을 여러 컴포넌트에서 사용할 수 있습니다.

```javascript
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```
```javascript
function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```

각 컴포넌트의 state는 완전히 독립적입니다. 
Hook은 state 그 자체가 아니라, ***상태 관련 로직***을 재사용하는 방법입니다.
실제로 각각의 Hook 호출은 완전히 독립된 state를 가집니다.
그래서 심지어는 한 컴포넌트 안에서 같은 custom Hook을 두번 쓸 수도 있습니다.

Custom Hook은 기능이라기보다는 컨벤션(convention)에 가깝습니다.
이름이 "`use`"로 시작하고, 안에서 다른 Hook을 호출한다면 그 함수를 custom Hook이라고 부를 수 있습니다.
`useSomething`이라는 네이밍 컨벤션은 linter 플러그인이 Hook을 인식하고 버그를 찾을 수 있게 해줍니다.

폼 핸들링, 애니매이션, 선언적 구독(declarative subscriptions), 타이머 등 많은 경우에 custom Hook을 사용할 수 있습니다. 

---

### 🔌 다른 내장 Hook
보편적이지는 않지만 유용하다고 느낄만한 내장 Hook이 몇 가지 더 있습니다. 
예를 들어, `useContext`는 컴포넌트를 중첩하지 않고도 React context를 구독할 수 있게 해줍니다.

```javascript
function Example() {
  const locale = useContext(LocaleContext);
  const theme = useContext(ThemeContext);
  // ...
}
```

그리고 `useReducer`는 복잡한 컴포넌트들의 state를 reducer로 관리할 수 있게 해줍니다.

```javascript
function Todos() {
  const [todos, dispatch] = useReducer(todosReducer);
  // ...
```

---
[React 공식문서](https://ko.reactjs.org/docs/hooks-overview.html)

[Using the State Hook](https://ko.reactjs.org/docs/hooks-state.html)

