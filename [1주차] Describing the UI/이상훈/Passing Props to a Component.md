> 이번 파트에서 배울 내용
> - 컴포넌트에 props를 전달하는 방법  
> - 컴포넌트에서 props를 읽는 방법  
> - props의 기본값을 지정하는 방법  
> - 컴포넌트에 JSX를 전달하는 방법  

## 친숙한 props
```jsx
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}
```

사실 위 친근한 `img` 태그에 전달되는 `className`, `src`, `alt` 등등도 props 입니다. (ReactDOM 은 HTML 표준을 준수합니다.)  
그런데 이런 기본 요소 말고도 직접 만든 컴포넌트에도 props 를 전달할 수 있습니다.

## 컴포넌트에 props 전달
### 단계1: 자식 컴포넌트에게 props 전달
```jsx
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

### 단계2: 컴포넌트의 매개변수로 props 읽기
```jsx
function Avatar({ person, size }) {
  // person and size are available here
}
```

## props의 기본 값 지정
```jsx
function Avatar({ person, size = 100 }) {
  // ...
}
```

## 전개 구문으로 props 한 번에 전달
```jsx
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```

```jsx
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

이렇게 props를 간결하게 전달할 수 있습니다.  
그러나 다른 모든 컴포넌트가 이런 구문을 사용한다면 문제가 있는 것이기에 제한적으로 사용해야 합니다.  

## 컴포넌트에 JSX 전달
`children` props 를 통해서 자식 JSX를 통째로 전달할 수 있습니다.  

```jsx
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}
```
