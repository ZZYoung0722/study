# useEffect

> 생명 주기 함수

기존 클래스형 생명주기 메서드는 연관성이 없는 여러 기능이 하나의 생명주기 메서드에 섞이게 된다. useEffect 훅을 이용하면 비슷한 기능을 한곳으로 모을 수 있어서 가독성이 좋아진다.

```javascript
function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `업데이트 횟수:${count}`;
  });

  const onClickIncrement = () => {
    setCount(count + 1);
  };

  return (
    <div className="App">
      <div>{count}</div>
      <button onClick={onClickIncrement}> 증가</button>
    </div>
  );
}
```

증가를 클릭 할 때마다 렌더링이 다시되고, 렌더링이 끝나면 useEffect 콜백함수가 호출 된다.

useEffect는 렌더링 할때마다 콜백함수가 호출되기 때문에 불필요한 호출을 방지하기 위해 두번째 매개변수로 배열을 입력하고, 
배열의 값이 변경되는 경우에만 useEffect 매개변수로 전달 콜백함수가 호출된다.

```javascript
useEffect(() => {
    document.title = `업데이트 횟수:${count}`;
}, [count]);//count가 변경시에만 콜백함수가 호출된다.
```

> 이벤트 함수 등록하고 해제하기

```javascript
function App() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const onResize = () => {
      setWidth(window.innerWidth);
    };

    window.addEventListener('resize', onResize);

    return () => {//1
      window.removeEventListener('resize', onResize);
    };
  }, []);

  return <div className="App">{`Width is ${width}`}</div>;
}
```

useEffect 두번째 매개변수로 빈배열[]을 넣음으로써 처음 렌더링될때만 호출된다.

1) 리턴하는 함수는 컴포넌트가 언마운트 될때 호출되서, resize 이벤트를 제거한다.





