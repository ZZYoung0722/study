# useState

함수형 컴포넌트에서 state를 사용할 수 있게 해준다.
증가, 감소 액션에 따라 state가 바뀌고 업데이트가 된다.

```javascript
function App() {
  const [count ,setCount] = useState(0);// 초기값

  const onClickIncrement = () => {
    setCount(count+1)
  };

  const onClickDecrement = () => {
    setCount(count-1)
  };

  return (
    <div className="App">
      <div>{count}</div>
      <button onClick={onClickIncrement}>증가</button>
      <button onClick ={onClickDecrement}>감소</button>
    </div>
  );
}
```
