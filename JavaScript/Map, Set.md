# Map, Set

### Map

- {키-값} 쌍인 집합
- 한 Map에서의 키는 오직 단 하나만 존재한다.(Map 집합의 유일성)
- Object는 Map과 유사하나 순회가 불가능, Map은 순회가 가능하다.(for of 반복문 활용)

### Map 객체 생성

```new Map()``` 생성자를 통해 Map 객체 생성한다.

```javascript
const m = new Map([
  ['number', 123],
  ['str', 'hello'],
])
```

### Map 메서드
#### **set()**

Map 객체에 주어진 키와 값 추가

```m.set(['arr', [1,2,3])```

#### **get()**

Map 객체의 value에 접근한다.

```javascript
m.get('arr')
// [1,2,3]
```

#### **has()**

Map 객체가 해당 key를 가지고 있는지를 Boolean으로 반환

```javascript
m.has('arr')
// true
```

#### **delete()**

Map 객체에서 특정 요소를 제거

```m.delete('arr')```

#### **size**

Map 객체의 요소수를 반환

```javascript
const m = new Map([
  	['number', 123],
  	['str', 'hello'],
  ])

m.size()
// 2
```

### Set

자료형과 관계없이 모두 유일한 값을 저장할 수 있는 객체

```javascript
const s = new Set('112233aabbcc');

console.log(s)
// {'1', '2', '3', 'a', 'b', 'c'}
```

> ❗ 알고리즘 문제에서 자주 사용되나 실무에선 자주 사용되지는 않는다고 한다

### Set 객체 생성

```new Set()``` 생성자를 통해 Set 객체를 생성한다
