> 이번 파트에서 배울 내용
> - 자바스크립트의 `map()` 함수를 사용하여 배열을 렌더링하는 방법  
> - 자바스크립트의 `filter()` 함수를 사용하여 특정 컴포넌트만 렌더링하는 방법  
> - 리액트에서 `key` 가 언제 필요하고 왜 필요한가?  


## 배열로부터 데이터 렌더링하는 방법  
### 1. 배열 정의
```js
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];
```

### 2. JSX 생성
```jsx
const listItems = people.map(person => <li>{person}</li>);
```

### 3. 컴포넌트에서 반환
```jsx
return <ul>{listItems}</ul>;
```

## 배열을 필터링하여 데이터 렌더링하는 방법
### 1. 배열 정의
```js
const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
}, {
  name: 'Percy Lavon Julian',
  profession: 'chemist',  
}, {
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
}];
```

### 2. JSX 생성
```jsx
const chemists = people.filter(person =>
  person.profession === 'chemist'
);
```

### 3. 컴포넌트에서 반환
```jsx
return <ul>{listItems}</ul>;
```

## Key가 필요한 이유
리액트는 기본적으로 항목에 대해서도 변화가 발생한 항목만 부분 렌더링 합니다.  
기본적으로 목록 아이템에 `key` 속성을 주지 않는다면 인덱스를 사용하는데 항목이 삽입되거나 삭제되면서 배열의 순서가 바뀌면 비효율적이며 종종 미묘하고 혼란스러운 버그가 발생하기도 합니다.  
따라서 각 아이템마다 고유한 값을 `key` 로 지정해주는 것이 중요합니다.  