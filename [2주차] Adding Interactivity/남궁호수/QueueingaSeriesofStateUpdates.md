## 여러 state 업데이트를 큐에 담기

react는 state를 업데이트하기 전, 이벤트 헨들러의 모든 코드가 실행될 때까지 기다린다. 그리고, 리렌더링은 모든 setState 설정자 함수가 호출이 완료된 이후에 일어난다.

export default function Counter() {
  const [number, setNumber] = useState(0);


```jsx
  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(n => n + 1);
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>+3</button>
    </>
  )
}
```
큐에서 이전 state 값을 가져와 계산을 할 수 있다.
n+n+1 을 업데이터 함수라고 한다.

업데이터 함수는 렌더링 중에 실행되며, 순수해야 한다.

### 명명규칙
업데이터 함수 인수의 이름은 해당 state 변수의 첫 글자로 보통 지정한다.
```jsx

setEnabled(e => !e);
setLastName(ln => ln.reverse());
setFriendCount(fc => fc * 2);
```

###정리
state를 설정해도 기존(전) 렌더링의 변수는 변경되지 않는다. 새로운 렌더링을 요청.
하나의 이벤트에서 일부 state를 여러 번 업데이트하려면 업데이터 함수를 사용한다.
