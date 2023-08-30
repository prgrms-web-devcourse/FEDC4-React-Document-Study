# state 구조 선택

## state 구조화 원칙

1. 관련 state를 그룹화한다.
   > - 두 개의 state 변수가 항상 함꼐 변경되는 경우에는 하나의 state 변수로 통합하는 것이 좋다.
   > - 필요한 state의 조각 수를 모르때 그룹화하는것이 좋다. 예를 들어 사용자가 사용자 정의 필드를 추가할 수 있는 양식이 있을 때 유용하다

```js
const [x, setX] = useState(0);
const [y, setY] = useState(0);
```

다음과 같이 그룹화 한다.

```js
const [position, setPosition] = useState({ x: 0, y: 0 });
```

**Pitfall**

> state 변수가 객체인 경우 다른 필드를 명시적으로 복사하지 않고는 그 안의 한 필드만 업데이트할 수는 없다.

```js
setPosition({ x: 100 }); // 오류
setPosition({ ...position, x: 100 });
```

2. state의 모순을 피한다.
3. 불필요한 state를 피한다.
   > 렌더링 중에 컴포넌트의 props 나 기존 state 변수에서 일부 정보를 계산 할 수 있는 경우 해당 정보를 컴포넌트의 state에 넣지 않아야 한다.

```js
const [firstName, setFirstName] = useState("");
const [lastName, setLastName] = useState("");
const [fullName, setFullName] = useState(""); //불필요

function handleFirstNameChange(e) {
  setFirstName(e.target.value);
  setFullName(e.target.value + " " + lastName); //불필요
}

function handleLastNameChange(e) {
  setLastName(e.target.value);
  setFullName(firstName + " " + e.target.value); //불필요
}
```

fullName state 변수는 렌더링 중에 언제든지 firstName, lastName 에서 fullName 을 계산 할 수 있으므로 불필요한 state 변수임으로 제거해야 한다.

다음과 같이 작성한다.

```js
const [firstName, setFirstName] = useState("");
const [lastName, setLastName] = useState("");

const fullName = firstName + " " + lastName; //fullName 은 state 변수가 아니다. 렌더링 중에 계산된다.

function handleFirstNameChange(e) {
  setFirstName(e.target.value);
}

function handleLastNameChange(e) {
  setLastName(e.target.value);
}
```

**Deep Dive**

- Don't mirror props in state : props 를 state 에 그대로 미러링 하지 마시오.

```js
function Message({ messageColor }) {
 const [color, setColor] = useState(messageColor);
```

 <br>
 부모 컴포넌트가 나중에 다른 messageColor 값을 전달하면 color state 변수가 업데이트 되지 않는다는것에 주의!! state는 첫 번째 렌더링 중에만 초기화된다.

<br>

4. state 중복을 피한다.

5. 깊게 중첩된 state를 피한다.<br>

   중첩된 state 를 업데이트하려면 변경된 부분부터 위쪽까지 객체의 복사본을 만들어야 한다. 깊게 중첩된 장소를 삭제하려면 해당 장소의 상위 장소 체인 전체를 복사해야 한다.

   > state 가 너무 깊게 중첩되어 업데이트하기 어려운 경우 "flat"하게 만들자!
