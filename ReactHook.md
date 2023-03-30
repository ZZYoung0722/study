# React Hook

### 리액트 훅 알아보기
함수형 컴포넌트에서도 클래스형 컴포넌트의 기능을 사용할 수 있게 하는 기능이다.
훅을 통해 함수형 컴포넌트에서도 컴포넌트 상탯값을 관리할 수 있고, 컴포넌트의 생명 주기 함수를 이용할 수 있다.

### 장점
재사용이 가능한 로직을 쉽게 만든다. 훅이 단순한 함수이므로 함수 안에서 다른 함수를 호출하는 것으로 새로운 훅을 만들 수 있기 때문이다.
리액트 내장 훅과 다른 사람들이 만든 여러 커스텀 훅을 조립해서 쉽게 새로운 훅을 만들 수 있다.
같은 로직을 한곳으로 모을 수 있어 가독성이 좋다.

### 기본 Hook
- [useState](https://github.com/ZZYoung0722/study/blob/main/ReactHook/useState.md)
- [useEffect](https://github.com/ZZYoung0722/study/blob/main/ReactHook/useEffect.md)
- [useContext](https://github.com/ZZYoung0722/study/blob/main/ReactHook/useContext.md)

### 추가 Hook
- [useReducer](https://github.com/ZZYoung0722/study/blob/main/ReactHook/useReducer.md)
- useCallback
- useMemo
- [useRef](https://github.com/ZZYoung0722/study/blob/main/ReactHook/useRef.md)
- useImperativeHandle
- useLayoutEffect
- useDebugValue

### Custom hook
- [custom hook](https://github.com/ZZYoung0722/study/blob/main/ReactHook/Custom%20Hook.md)

### Hook 사용 규칙
1. 하나의 컴포넌트에서 훅을 호출하는 순서는 항상 같아야한다. -> 내부적으로 훅 처리를 호출된 순서로 관리한다.
2. 함수형 컴포넌트, 커스텀 훅 안에서만 호출되어야한다.
