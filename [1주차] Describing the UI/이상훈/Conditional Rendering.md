> 이번 파트에서 배울 내용
> - 조건에 따라 다른 JSX를 반환하는 방법  
> - 조건에 따라 JSX 조각을 포함하거나 제외하는 방법  
> - 리액트 코드에서 만나게 될 일반적인 조건부 문법  

## 조건부로 다른 JSX 반환하기  
```jsx
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✔</li>;
  }
  return <li className="item">{name}</li>;
}
```

## JSX 내부에서 삼항 연산자 사용하기
```jsx
return (
  <li className="item">
    {isPacked ? name + ' ✔' : name}
  </li>
);
```

> 위 두 예시 코드는 완전히 동일합니다.  

## 논리 AND 연산자 사용하기
```jsx
return (
  <li className="item">
    {name} {isPacked && '✔'}
  </li>
);
```

### 주의사항  
논리 AND 연산자의 좌변 항에는 숫자가 오면 안됩니다.  
숫자가 `0` 이면 `0` 을 화면에 그리게 됩니다.  
대신에 `messageCount > 0 && <p>New messages</p>` 형태를 사용해야 합니다.  

## 변수에 JSX 조건부 할당
```jsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✔";
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}
```
