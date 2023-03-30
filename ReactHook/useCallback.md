# useCallback

리액트의 렌더링 성능을 위해 제공되는 훅이다.

훅을 사용하면서 컴포넌트가 렌더링 될때마다 함수를 생성해서 자식 컴포넌트의 속성으로 넘겨주게 된다.

```javascript
function App() {
  const [name, setName] = useState('');
  const onSave = () => {};

  return (
    <div className="App">
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <Profile onSave={onSave} />
    </div>
  );
}
```

name이 변경되어 렌더링을 하게 될때 onSave 함수가 새로 만들어지고 Profile 속성으로 새로운 함수를 넣어주고 있다.
이때 Profile 컴포넌트에서 React.memo를 사용해도 이전 onSave 와 이후 onSave 가 매번 다르게 되어 매번 렌더링이 된다.
이때 Profile 재 렌더링을 방지하기 위해 useCallback을 사용한다.

> useCallback 사용하기

```javascript
import React, { useCallback, useState } from 'react';
import Profile from './Profile';


function App() {
  const [name, setName] = useState('');
  const onSave = useCallback(() => {
    console.log(name);
  }, []);

  return (
    <div className="App">
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <Profile onSave={onSave} />
    </div>
  );
}
```

useCallback 첫번째 파라미터로 기존 함수를 넣어주고, 두번째 파라미터로 [] 배열을 넣어주면 된다.
이전 코드와의 다른점은 onSave 함수를 재사용하게되어 profile 컴포넌트 재 렌더링을 방지할 수 있다.

**주의 할 점은 useCallback 두번째 파라미터의 배열 값에 함수가 재 생성할 기준을 할당 해야한다는 것이다.
onSave 함수내의 console.log(name)은 몇번이 호출되어도 "" 빈 문자열만 출력된다.**

**-> onSave 함수 생성당시 name은 "" 빈값이었기 때문에**

**따라서 onSave 안에서 다른 변수들을 참조하게 된다면, 그 변수들을 useCallback 두번째 파라미터 배열 값으로 넣어준다.
그럼 이제 onSave 함수는 name이 바뀔때만 재 생성하게 된다.**

```javascript
  const onSave = useCallback(() => {
    console.log(name);
  }, [name]);//name이 변경될 때에만 함수 재생성.
```
