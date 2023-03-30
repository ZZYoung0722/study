# Custom Hook

-내장 훅을 이용하여 새로운 커스텀 훅을 제작하고 재사용할 수 있다.
-가독성을 위해 네이밍은 useXXXX를 따른다.

윈도우 창의 너비를 정장하여 제공하는 커스텀 훅

> useWindowWidth 커스텀 훅

```javascript
import { useEffect, useState } from 'react';

const useWindowWidth = () => {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const onResize = () => {
      setWidth(window.innerWidth);
    };

    window.addEventListener('resize', onResize);

    return () => {
      window.removeEventListener('resize', onResize);
    };
  }, []);

  return width;
};
```

커스텀 훅 사용

창의 넓이가 변경되면 새로운창의 넓이를 제공받아 다시 렌더링 된다.

```javascript
function App() {
  const width = useWindowWidth();
  return <div className="App">{`Width is ${width}`}</div>;
}
```

> useHasMounted 커스텀

컴포넌트 마운트 여부를 알려주는 useHasMounted 훅 작성

```javascript
import { useEffect, useState } from 'react';

const useHasMounted = () => {
  const [hasMounted, setHasMounted] = useState(false);
  useEffect(() => {
    setHasMounted(true);
  },[]);
  
  return hasMounted;
};
```

- useHasMounted 훅은 컴포넌트가 마운트 된 후라면 참을 반환한다.
- useEffect의 두번째 매개변수로 빈 배열을 전달하여 업데이트 시에는 setHasMounted 함수가 호출 되지 않는다.
