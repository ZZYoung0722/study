# useMemo

이전 값을 기억해 성능을 최적화하는 용도로 사용한다.

```javascript
import React, { useMemo } from 'react';

const Memo = ({ v1, v2 }) => {
  const value = useMemo(() => {//1
    return v1 + v2;
  }, [v1]);

  return <div>{value}</div>;
};

export default Memo;
```

useMemo 첫번째 매개변수로 함수를 입력하고, 두번째 매개변수로 배열을 입력한다.

- 첫번째 매개변수 return 값을 기억하고 있고 그 값을 value에 할당한다.
- 두번째 매개변수 배열의 값이 변경되지 않으면 이전에 반환했던 값을 재사용한다. 만약 배열의 값이 바뀌었다면 useMemo
- 첫번째 매개변수인 함수를 재실행하여 그 return 값을 기억한다.

결론적으로 위의 코드는 v1 값이 바뀌면 1번 함수가 실행되고
v1 이 안바뀌면 이전 return 값을 재사용한다.
