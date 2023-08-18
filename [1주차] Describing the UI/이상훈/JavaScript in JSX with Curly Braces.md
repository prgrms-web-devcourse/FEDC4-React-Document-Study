> 이번 파트에서 배울 내용
> - JSX 문법에서 어떻게 따옴표로 문자열을 전달하는가?  
> - JSX 문법에서 어떻게 중괄호 내에서 자바스크립트 변수 값을 참조하는가?  
> - JSX 문법에서 어떻게 중괄호 내에서 자바스크립트 함수를 호출하는가?  
> - JSX 문법에서 어떻게 중괄호 내에서 자바스크립트 객체를 사용하는가?  

## 따옴표로 문자열 전달
요소의 속성에 들어가는 문자열같은 경우 홑따옴표나 쌍따옴표로 전달할 수 있습니다.

```jsx
export default function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/7vQD0fPs.jpg"
      alt="Gregorio Y. Zara"
    />
  );
}
```

## 중괄호로 변수 값 참조
```jsx
export default function TodoList() {
  const name = 'Gregorio Y. Zara';
  return (
    <h1>{name}'s To Do List</h1>
  );
}
```

## 중괄호로 함수 호출
```jsx
const today = new Date();

function formatDate(date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function TodoList() {
  return (
    <h1>To Do List for {formatDate(today)}</h1>
  );
}
```

## 중괄호로 객체 사용
```jsx
export default function TodoList() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}
```
