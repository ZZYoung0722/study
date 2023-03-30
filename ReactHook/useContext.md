# useContext

자식 컴포넌트는 부모 컴포넌트에서 전달된 컨텍스트 데이터 사용시 Consumer 컴포넌트를 사용하지 않고 쓸수 있다.

> 훅 없이 컨텍스트 데이터 받기

- 부모 컴포넌트

```javascript
import React, { createContext } from 'react';
import Child from './Child';

export const UserContext = createContext();
const user = { name: 'A', age: 20 };

function App() {
  return (
    <div className="App">
      <UserContext.Provider value={user}>
        <Child />
      </UserContext.Provider>
    </div>
  );
}

export default App;
```

- 자식 컴포넌트 Consumer 컴포넌트를 사용해야한다. 1번영역에서 컨텍스트 데이터를 쓸 수 없다.

```javascript
const Child = () => {
//1
  return (
    <div>
      <UserContext.Consumer>
        {(user) => {
          console.log(user);
        }}
      </UserContext.Consumer>
    </div>
  );
};

export default Child;
```

> useContext 훅으로 컨텍스트 데이터 받기
```javascript
import React, { useContext } from 'react';
import { UserContext } from './App';

const Child = () => {
//1
  const user = useContext(UserContext);
  return (
    <div>
      {user.name}
      {user.age}
    </div>
  );
};

export default Child;
```

1번영역에서 컨텍스트 데이터 사용가능
