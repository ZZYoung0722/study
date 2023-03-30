# useRef

> 함수형 컴포넌트에서 돔 요소 접근하기 useRef

```javascript
function App() {
  const inputRef = useRef(null);

  const onClickInputFocus = () => {
    inputRef.current.focus();
  };

  return (
    <div className="App">
      <input type="text" ref={inputRef} />
      <button onClick={onClickInputFocus}>input 포커스 하기</button>
    </div>
  );
}
```

inputRef 변수를 input ref props로 지정한다.
이제 inputRef 변수로 input 에 접근 가능하게 되었다. 
주의할 점은 inputRef로 접근하는것이 아니라 inputRef.current로 접근해야한다.

버튼을 클릭하면 input에 포커스가 된다. 

> 렌더링과 무관한 값 저장하기

기본적으로 리액트에서 화면을 렌더링 하려면 변화되는 값을 state로 지정하고 state가 업데이트 되면
리액트에서 알아서 렌더링을 해준다. 하지만 때때로 랜더링과 관련없는 값을 저장할때가 있다. 이때 useRef를 사용하면된다.
useRef는 값이 바뀌어도 랜더링 되지 않는다.

```javascript
function App() {
  const [count, setCount] = useState(0);
  const intervalID = useRef(0);

  useEffect(() => {
    intervalID.current = setInterval(() => {
      setCount((count) => {
        return count + 1;
      });
    }, 1000);
  }, []);

  const onClickStop = () => {
    clearInterval(intervalID.current);
  };

  return (
    <div className="App">
      <div>{count}</div>
      <button onClick={onClickStop}>Stop</button>
    </div>
  );
}
```

최초 랜더링 후 useEffect에 의해 setInterval이 실행되고 1초마다 화면을 갱신하는 예제이다.

setInterval 사용할때 반환하는 id 저장 후 clear 할때 이 id로 clear를 하게된다.
이때 interval id는 랜더링과 전혀 무관한 값이다. 이런 상황에 useRef를 사용하면 된다.

물론 state를 사용해도 동작에는 문제가 없지만 setInterval id를 state로 업데이트 하는 순간 랜더링과 관련 없는 렌더링이 한번 더 일어나고, 랜더링과 무관한 값을
state로 지정함으로써 가독성 면에서도 떨어지는 단점이 있기 때문에 state보다는 useRef를 사용하는 것이 나은 것 같다.
